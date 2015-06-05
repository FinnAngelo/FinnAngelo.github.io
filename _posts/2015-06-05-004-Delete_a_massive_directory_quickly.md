---
layout: post
title: "#004: Delete a massive directory quickly with a batch file"
published: true
---
<a href="https://www.flickr.com/photos/mell242/46504703" title="Rubbish by mell, on Flickr"><img src="https://c1.staticflickr.com/1/26/46504703_69765383fc_z.jpg?zz=1" width="640" height="480" alt="Rubbish"></a>  
I was at work a couple of years ago, waiting while some directory with zillions of images (or something like that) 
was deleting, and Nick sent me this bat file that makes the delete process a little quicker

-----------------------
Delete_Directory.bat
-----------------------

```bat

del /f/s/q "C:\SomeDirectory" > nul
rmdir /s/q "C:\SomeDirectory"

pause

```

--------------------------------------------
Delete lots of files without the recycle bin
--------------------------------------------

How many people remember this?  
Hold the `shift` key down when you delete the file(s), and kept it down when it prompts `Are you sure?`
