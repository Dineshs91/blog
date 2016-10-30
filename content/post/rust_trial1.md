+++
Description = ""
Tags = []
date = "2016-10-17T01:50:11+05:30"
title = "Dabbling with Rust"

+++

I liked rust when I tried it last year around May. I also did some contributions to cargo which is the package manager for Rust.

I wanted to do something in rust, something small to do over in the weekend. I finally decided to mimic unix's ls (List files).
So the program will just print the details of the files and directories with the path, size and modified time. Also display the total
block size.

Here is a list of all the external packages I've used.

1. colored
2. chrono

To get the total file bock size, I had to get all the files first using `std::fs`. Then iterate through them to get the block size of
each file and add them up to `file_block_count` variable.

In rust if you iterate through an iterator, then it cannot be reused. So I had to get the files again. Iterate through them and get the
following

1. File path
2. File size
3. File modified at datetime.

Getting file path and size are pretty straight forward. `file_path.display()` gives the path name and `file_path.metadata().unwrap().len()`
gives the size of the file.

With the file modified datetime we have to convert into the required display format. This is where we need to use `chrono`.
I used `Local.timestamp(file_modified as i64, 0);` to get the datetime in the local time zone and `format("%h %d %H:%M")` for formatting the 
datetime.

I used `colored` package to add terminal colors.

So the output looks like this

```
total  128
1   ./.DS_Store      6148    Oct 29 21:46
2   ./.git           510     Oct 30 21:37
3   ./.gitignore     341     Oct 16 14:33
4   ./Cargo.lock     4608    Oct 17 00:43
5   ./Cargo.toml     133     Oct 17 00:43
6   ./LICENSE        35141   Oct 16 14:33
7   ./README.md      151     Oct 16 16:26
8   ./src            136     Oct 16 14:33
9   ./target         102     Oct 16 00:27
```

Source code can be found [here](https://github.com/Dineshs91/rs-ls/). This is a very trivial code to get started with rust.
