+++
Description = ""
Tags = []
date = "2016-12-17T14:14:54+05:30"
title = "Docker automatic image build with raspberry pi"

+++

It was more than a week, that I had a raspberry pi and did nothing with it (Other than the initial setup). I was looking to do
something with it. Then I decided to write a bot that will build docker images and push it to docker registry.
<!--more-->

The design goal was simple.

1. Check if any new tags were created (In github)
2. Notify about the same.
3. Build docker images and push them to docker registry (Dockerhub)
4. Notify when the images are pushed to dockerhub.

For most of the above steps, I used python's subprocess module and for notification I used twilio.

The first step is a bit challenging, while all the others are pretty straightforward.

Why is the first one challenging ? Well how to know if a new tag was pushed, github hooks ? I didn't want to use that solution, because
it needs a server to do this. So I had to think of something else. 

After a while of thinking, I came up with this. (Thanks to stackoverflow !!)

- `git fetch --tags` 
- `git rev-list --tags --max-count=1`
- `git describe --tags rev_list`

Summarizing the steps above, fetch all the tags and get the latest tag from the list of tags. These commands we execute from shell.
I used subprocess to do it from python. By this we have the latest tag with us.

Next we need to know the tag our bot processed recently so that we only process the new tags, that were not processed.
For this, I write the processed tag to a file at `~/.docker-bot`. Now all I have to do is check the file to see if the latest tag
was processed. 

Notification is very simple. I used twilio python library. For docker build images and push also is very straightforward. Just docker
build and docker push will do. 

For scheduling I am using cron, which is very reliable and just works. 
This is my cron expression

```
*/5 * * * * . $HOME/.profile; cd ~/Documents/developer/accounts; python ~/Documents/developer/docker-bot/main.py >> /var/log/docker-bot.log 2>&1
```

Some final thoughts. The reason I had to this is Docker hub supports automated builds, but it doesn't support passing in some env variables or anything.
So basically it is just docker build without any build arguments. 