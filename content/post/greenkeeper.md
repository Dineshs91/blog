+++
Description = "Greenkeeper, meet the dependency genie"
Tags = []
date = "2015-10-09T18:50:37+05:30"
title = "Greenkeeper, meet the dependency genie"

+++

Meet [greenkeeper](http://greenkeeper.io/), the app that automates dependency updates and notifies you of build failures 
so you can spend your development time actually making things.

<!--more-->

So, what exactly is greenkeeper ?

If you have worked on any node.js project, then you probably know how painful it is, to manage all the dependencies.
This is the problem that greenkeeper is trying to address. 

**Getting started**

Setting up greenkeeper is very simple. Just a few commands and clicks, and you are good to go.

```$ npm install -g greenkeeper```

```$ greenkeeper login```

![login](/images/greenkeeper/login.png) 

This will open a browser and asks you to log in to GitHub. By the way, that's a cool logo.

![github](/images/greenkeeper/github.png)

Once you authorize the application, you will get the successful message in terminal.

![done](/images/greenkeeper/done.png)

```$ greenkeeper enable```

This will read all your repositories and enables the applications which are supported by it.

That's it. It's easy right. Just 3 steps and we are all set.

**Let the magic begin**

After you have done the setup, sit back and wait for the PR from greenkeeper.

The PR looks like this.

![pull request](/images/greenkeeper/pull_request.png)

You can review the PR and take the necessary action. The awesome thing is if you are using travis-ci, then
the PR is run on it, so you can know if it breaks anything. Cool right. 

It really helps developers in managing their dependencies. This is a must if you are developing node.js apps.
They have both free and paid plans. Pay a visit to their [site](http://greenkeeper.io/) to know more.