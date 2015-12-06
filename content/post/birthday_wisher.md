+++
Description = ""
Tags = []
date = "2015-12-06T23:34:13+05:30"
title = "Never forget to wish your close one's again."

+++

It was a rainy weekend at my place. I was thinking how to make best use of that time and something crossed
my mind. What if a bot, can send birthday wishes to my friends on Facebook ? It would save me of all the regrets I have
had in the past. I said to myself **challenge accepted**. 
<!--more-->

I have done a similar kind of project in the past at work, where we had to get the gift card balances of 5 gift cards.
I used python urllib and re(regex) to accomplish the task. Hit the url with urllib2 and parse the balances from the
response using regex. But this time, I wanted to try a different approach. I guessed that the previous approach wouldn't
work so well. So, how do I do this ?

I had some experience with [selenium](http://www.seleniumhq.org/) (It is for automating web applications for testing purposes), so I wanted to give it a try. 
Java ? Wait, who am I kidding ? No way. I went with [node.js](https://nodejs.org/en/) (Seemed better, lot better). The thought of opening an IDE, waved me off of java. 
So I bootstrapped a small code in node.js. I had to choose between [protractor](https://angular.github.io/protractor/#/) 
and [webdriverio](http://webdriver.io/). I had used protractor in my [devlog](https://github.com/dineshs91/devlog) project,
but it works only with angular (You can make it work for non-angular projects too, but with some tweaks). So I decided to 
take webdriverio for a spin and it did the job quite well.

Having chosen the framework, it was time to write the code. One weird thing I noticed in facebook is that even the ID 
attribute of an element was not consistent. It kept changing, so you can't use them in selectors. I completed the code
in 10-20 Lines. Selenium invokes a browser, does the actions specified in the code and quits. Now we don't want a browser
show up on the screen, when the bot works right. This is where [phantomjs](http://phantomjs.org/) shines. It is a headless
browser for testing purposes. 

There was one challenge I faced, which was synchronising the 2 steps. step 1 start the selenium standalone server and step 2
execute the webdriverio code. I solved this problem using event emitter. So when selenium server starts a ```start``` event
is emitted and there is a listener, which listens to this event and starts the webdriverio execution. Having solved the main
problem, I did some final refinements and that's it. The code is ready. Rock and roll.

You can find the source [here](https://github.com/Dineshs91/fb-birthday-wisher). And I forgot to mention that, the program 
randomly chooses a wish from an array of wishes in the code.