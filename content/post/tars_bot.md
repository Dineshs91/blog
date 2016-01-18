+++
Description = ""
Tags = []
date = "2016-01-18T21:26:25+05:30"
title = "Tars my first bot"

+++

In one of my earlier [posts](http://dineshs91.github.io/blog/post/greenkeeper/) I had written about [greenkeeper](http://greenkeeper.io/) the app that automates dependency updates. I will start with a brief explanation about greenkeeper, it will send you a pull request(PR) 
whenever there is a newer version of your depenedencies specified in package.json. This is just one half of the automation right. 
You have to manually merge those PR's. 
<!--more-->

I thought I could automate that part too. This is were [tars](https://github.com/Dineshs91/tars) comes into picture. Yes, the name was
inspired from Interstellar. We all love tars right!. I wrote this bot to help me with my [devlog](https://github.com/Dineshs91/devlog)
project.

It follows a 3 step process. Check, merge and delete. First step, fetch all open PR's and see which one's are opened by GREENKEEPER_BOT. 
We only care about the one's opened by it. Then we check the CI status of the PR, whether the tests are passing 
or failing. In the next step, if the tests are passing, tars merge's the PR. If PR cannot be merged automatically, it creates a comment with 
a message saying *This pull request cannot be merged by me*

After successful merging of PR, it deletes the corresponding branch. Greenkeeper opens a branch with the PR. So after merging,
the branch is no longer needed. Tars removes the branch once the PR is merged succesfully into master branch.

The next part is to, set up the program to run as a job. You can schedule a cron job to make the process completely automatic.
You can use [crontab.guru](http://crontab.guru/) which is a simple editor for cron schedule expressions.

Bots are fascinating right, it's an opportunity for us to just sit back, and watch things happen. Why not write your own bot ?
I am sure it will be fun.