---
layout: post
title: "Updating Jekyll in GitHub Pages"
categories: Jekyll 
published: true
---

I was getting security warnings from my github pages commits from the dependabot about a kramdown dependency.  
I thought it would be easy to fix, but nah, it was easier to just reinstall everything.

I don't have time to learn the ins-and-outs of Ruby and Jekyll, so here is my dirty fix

----------------------------------------

## TL;DR

+ [Setup a Jekyll Sandbox](#Setup-a-Jekyll-Sandbox)
+ [Setup GIT with Chocolatey](#Setup-GIT-with-Chocolatey)
+ [Install Ruby Dev tools and Jekyll](#Install-Ruby-Dev-tools-and-Jekyll)
+ [Clone and backup your website](#Clone-and-backup-your-website)
+ [Make a new jekyll website](#Make-a-new-jekyll-website)
+ [Restore your blogs and stuff](#Restore-your-blogs-and-stuff)
+ [Credits](#Credits)

----------------------------------------

## Setup a Jekyll Sandbox

I do this in a sandbox on Windows 10 because I dont really want Ruby on my day-to-day pc when I only use it every 18 months or so...

Make sure you have Win10 and Sandboxes enabled

Make a file called `SandBox.wsb` with this content:

```xml
<Configuration>
  <MappedFolders>
    <MappedFolder>
      <HostFolder>C:\Users\Jon\Desktop\GIT\Sandbox-Jekyll</HostFolder>
      <ReadOnly>false</ReadOnly>
    </MappedFolder>
  </MappedFolders>
</Configuration>
```

----------------------------------------

## Setup GIT with Chocolatey

In the sandbox, run this in the Powershell ISE as steps

```powershell
Set-ExecutionPolicy Unrestricted
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Then install Git

```powershell
choco install notepadplusplus -y
choco install git -y
choco install tortoisegit -y
```

----------------------------------------

## Install Ruby Dev tools and Jekyll

- <https://jekyllrb.com/docs/installation/windows/>
- <https://rubyinstaller.org/>

01. Install Ruby from the installer - I used `rubyinstaller-devkit-3.0.1-1-x64.exe`
02. Run `ridk install` to setup MSYS2 and dev toolchain 
	- This is option 3 on the cmd window installer
03. Install Jekyll with `gem install jekyll bundler` in a new cmd window
	- Check with `jekyll -v`

----------------------------------------

## Clone and backup your website

- <https://stackoverflow.com/questions/52344102/upgrading-jekyll-and-dependencies-for-github-pages>

01. Clone to sandbox 
	- I use TortoiseGit
02. Delete everything except:
	- `.git` folder!!
	- `_posts` folder
	- `CNAME` file
	- `_config.yml`
	- `FinnAngelo.github.io.code-workspace` file
	- `about.markdown` and `index.markdown` if you have changed them
03. Move everything except the `.git` folder out

----------------------------------------

## Make a new jekyll website

<https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll>

01. Navigate the cmd window to the website folder
02. Make a new jekyll website with `jekyll new .`
03. Replace the  `gem "jekyll"`with `gem "github-pages"` in the gemfile as per instructions on link
	- Dont forget the version bit
04. Run `bundle update`

----------------------------------------

## Restore your blogs and stuff

01. Put everything back except the `_config.yml`
02. Put all the missing bits back into the `_config.yml` from the backup
03. Commit
	- Check renames
04. Push

----------------------------------------

## Credits ##

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
