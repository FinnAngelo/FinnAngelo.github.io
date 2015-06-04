---
layout: post
title: "#004: Delete a massive directory quickly with a batch file"
published: true
---
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
