+++
Description = ""
Tags = []
date = "2015-09-24T09:14:08+05:30"
title = "Add a custom titlebar in nw.js apps"

+++

In this post, I am going to show you how to create a custom titlebar for your nw.js app. If you haven't heard of nw.js, check it out 
[here](https://github.com/nwjs/nw.js). It enables a new way of writing desktop applications with all web technologies.
<!--more-->
Let's answer a simple question. **Why do we need a custom titlebar ?** Because it's better than the default one and it looks
the **same across different platforms**. Default titlebar looks different, on different operating systems.

**Devlog with custom titlebar**
![custom titlebar](/images/custom_titlebar/custom titlebar.png) 
**And with default one.**
![default titlebar](/images/custom_titlebar/default titlebar.png) 

Custom titlebar looks cool right ?

**Let's get started.**

1.  The first thing we do is, we disable the default titlebar. 

        "window": {
          "toolbar": false,
          "frame": false
        }
    

    To do that, make the **frame** and **toolbar** name false in package.json. This will remove the frame, and now the window becomes 
    frameless. 
    
2.  After the first step, we will notice that the window is frameless now and we will not be able to drag the
    application anymore.
    
    To make the whole window draggable, add style `-webkit-app-region: drag` to body tag.
    
        <body style="-webkit-app-region: drag">
            ...
        </body>
        
    
    Or we can make a particular region draggable, by adding style `-webkit-app-region: drag` to that element.
    
        <div class="titlebar" style="-webkit-app-region: drag">
            ...
        </div>
        
    
    Now, our application is draggable.
    
3.  Next thing, we add the window buttons (close, minimize and maximize)

    For buttons, I've used semantic-ui [labels](http://semantic-ui.com/elements/label.html#circular) for devlog.
    
    **OS X style buttons**
    ![Alt text](/images/custom_titlebar/window buttons.png) 
    
    Though it's not perfect, it does the job well. 
    
That's it, we have successfully created a custom titlebar for our nw.js application. Go ahead and create awesome titlebars for your apps.