+++
Description = ""
Tags = []
date = "2017-04-04T19:15:19+05:30"
title = "Sending ARP packets using rust"

+++

While casually browsing about ARP, I stumbled upon [kickthemout](https://github.com/k4m4/kickthemout) project on github. I decided to do something similar in [Rust](https://www.rust-lang.org/en-US/).

<!--more-->

Some info about ARP.

- Operates at the Data link layer in the OSI model.
- It is used for resolution of Internet layer addresses into link layer addresses. (Mapping Ip address to Mac address).

Scanning rust crates.io I came across [libpnet](https://github.com/libpnet/libpnet) which allowed devs to do low level networking using Rust, which is
perfect for the job.

So the goal was to be able to send 3 types of packets. 

1. ARP Request
2. ARP Reply
3. ARP Gratuitous Request

I started looking at the examples provided in the repository. Well, it had only 2 examples (at the time of writing) which were not very useful. So I 
started checking their [doc](https://octarineparrot.com/assets/libpnet/doc/pnet/).

I had to do these 2 things. 

1. Hand craft the ARP packet.
2. Transmit the packet.

As the first thing I sat down to figure out how to craft an ARP packet (Request/Reply.) Checking the docs, I understood that there is no straightforward way of doing this. There were 2 ways of doing it. Capture a packet, clone from it and then modify or the other solution was to hand craft the hex values. 
I decided to go with the 2nd solution, as I did not want to capture any packets.

```rust
pub struct EthernetPacket<'p> {
    packet: &'p [u8],
}
```

Ethernet header
```rust
pub struct Ethernet {
    destination: MacAddr,
    source: MacAddr,
    ethertype: EtherType,
    payload: Vec<u8>,
}
```

    MacAddr `ff:ff:ff:ff:ff:ff` (6 octets)
    EtherType is used to indicate which protocol is encapsulated in the payload of the frame. ARP (0x0806)
    For detailed information on EtherTypes checkout this [doc](https://www.iana.org/assignments/ieee-802-numbers/ieee-802-numbers.xhtml)

So the custom hex array looks like this 

```rust
[
  0xff, 0xff, 0xff, 0xff, 0xff, 0xff, // Destination mac address
  0xce, 0xaf, 0x23, 0xde, 0x31, 0xae, // Source mac address
  0x08, 0x06 // Ether type (ARP 0806)
]
```
We have constructed only the ethernet header. The payload will hold the ARP header. Lets create it now.

```rust
pub struct ArpPacket<'p> {
    packet: &'p [u8],
}
```

Arp header
```rust
pub struct Arp {
    hardware_type: ArpHardwareType,
    protocol_type: EtherType,
    hw_addr_len: u8,
    proto_addr_len: u8,
    operation: ArpOperation,
    sender_hw_addr: MacAddr,
    sender_proto_addr: Ipv4Addr,
    target_hw_addr: MacAddr,
    target_proto_addr: Ipv4Addr,
    payload: Vec<u8>,
}
```

    hardware_type - (1) Ethernet [Refer](https://www.iana.org/assignments/arp-parameters/arp-parameters.xhtml#arp-parameters-2)
    protocol_type - Ether type (IPv4 0800).
    hw_addr_len - 6
    proto_addr_len - 4
    operation - There are 2 ArpOperations. Request and Reply.
    sender_hw_addr, target_hw_addr - mac address of source and destination.
    sender_proto_addr, target_proto_addr - IPv4 address of source and destination.
    payload - This can be ignored.

So the custom hex values look like this

```rust
[
  0x00, 0x01, // hardware type
  0x08, 0x00, // Ether type (IPv4)
  0x06, // hardware address length
  0x04, // protocol address length
  0x00, 0x01, // Request(0x01)/Reply(0x02)
  0xce, 0xaf, 0x23, 0xde, 0x31, 0xae, // sender hardware address (mac address)
  0xc0, 0xa8, 0x00, 0x66, // Sender proto address Ipv4(192.168.0.102)
  0xff, 0xff, 0xff, 0xff, 0xff, 0xff, // target hardware address (Broadcast)
  0xc0, 0xa8, 0x00, 0x65 // Target proto address Ipv4(192.168.0.101)
]
```

Now that we have the packets ready, its time to transmit them. For transmission I am going to use `pnet::datalink` which provides support for sending
and receiving data link layer packets. We can send the packet using `send_to` method.

Everything looks good and we are able to send the packets. But there is a problem with this implementation. Since we are hardcoding all the hex values,
we lose the ability of dynamic IP and mac values. So I went ahead and asked for help on [#libpnet freenode](http://webchat.freenode.net/?channels=%23libpnet) to find out if any other techniques existed. One of the [contributors](https://github.com/faern) suggested that I create a 
MutableEthernetPacket with a buffer initialized with zero's. Once I have the mutable packet, I can just use the setters provided by the library.

**Example:**
```
let mut ethernet_buffer: Vec<u8> = (0..42).map(|_| 0x00).collect::<Vec<u8>>();

let mut ethernet_packet = MutableEthernetPacket::new(&mut ethernet_buffer).unwrap();
ethernet_packet.set_destination(target_mac);
ethernet_packet.set_source(source_mac);
ethernet_packet.set_ethertype(EtherTypes::Arp);
```

I did the same for arp packet using MutableArpPacket. Then this packet should be set as the payload of the above created mutable ethernet packet and the 
ethernet packet is transmitted. This worked perfectly !!

### Kick someone out of the network
Source ip - Gateway ip
Source mac - Your mac address
Target ip - IP of the host you want to kick out.
Target mac - Target mac address

To find the target mac, `arp -a` (Displays the arp table). You should find an entry for the corresponding target ip. If it is not available, then ping 
the target ip first and then check the arp table. Also we have to send an ARP reply packet.

And there it is, we have it working. You can check out the project [here](https://github.com/Dineshs91/send-arp)