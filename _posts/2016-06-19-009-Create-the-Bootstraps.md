---
layout: post
title: "#009: Create-the-Bootstraps"
published: true
---

---------------------------------------------
Setup `NodeCmd.bat`
---------------------------------------------

<http://stackoverflow.com/questions/60904/how-can-i-open-a-cmd-window-in-a-specific-location>  
<http://superuser.com/questions/106848/batch-file-that-runs-cmd-exe-a-command-and-then-stays-open-at-prompt>  
<http://stackoverflow.com/questions/3827567/how-to-get-the-path-of-the-batch-script-in-windows>  
<http://stackoverflow.com/questions/13704223/how-do-you-use-setlocal-in-a-batch-file>  

Create `NodeCmd.bat` in a folder called `NodeTools`

```bat
@echo off

setlocal
path=%~dp0;%path%
cmd /K "cd %~dp0"

endlocal
```

---------------------------------------------
Download NODE
---------------------------------------------

<http://stackoverflow.com/questions/24246681/how-can-i-run-nodejs-from-usb-drive>  
I got v6 at <http://nodejs.org/dist/>

```bat
REM Node Version
node -v
```

---------------------------------------------
Download NPM and update
---------------------------------------------

<https://docs.npmjs.com/getting-started/installing-node>  
I got the last version at <http://nodejs.org/dist/npm/>

```bat
REM UPDATE NODE
npm install npm
npm -v
```

---------------------------------------------
Install Yeoman
---------------------------------------------

<http://yeoman.io/learning/index.html>

```bat
npm install yo bower grunt-cli
```

---------------------------------------------
Setup dev site
---------------------------------------------

Create a folder called `BootstrapMe` and copy the `NodeCmd.bat` to it

```bat
yo bootstrap
```


