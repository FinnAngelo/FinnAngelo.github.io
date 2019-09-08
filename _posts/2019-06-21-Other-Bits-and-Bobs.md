---
layout: post
title: "Other Cute Bits-and-Bobs"
published: true
---

More goodness - but not powershell...

This will obviously update as I find more 

----------------------------------------

+ [Taskbar directory](#Taskbar-directory)
+ [Chrome incognito shortcut](#Chrome-incognito-shortcut)
+ [Credits](#Credits)    

----------------------------------------
<a name="Taskbar-directory"></a>
## Taskbar directory ##

So where do all the shortcuts on the taskbar live?

`C:\Users\jfinnangelo\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar`

----------------------------------------
<a name="Chrome-incognito-shortcut"></a>
## Chrome incognito shortcut ##

How do I open chrome as incognito from a shortcut?

Clone a shortcut to chrome, and change the `target` field to this:  
`"C:\Program Files (x86)\Google\Chrome\Application\chrome_proxy.exe" --profile-directory=Default -incognito https://mail.google.com https://www.onenote.com`

Note that it opens **two** web pages in separate tabs - harrah!  
I keep the shortcut in the Taskbar directory
    
----------------------------------------
<a name="Credits"></a>
## Credits ##

_Cheers!_
