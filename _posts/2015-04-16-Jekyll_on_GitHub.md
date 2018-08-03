---
layout: post
title: "Setup Jekyll on GitHub"
published: true
---

<a href="https://en.wikipedia.org/wiki/Robert_Louis_Stevenson" title="Robert Lewis Stevenson - Writer of cool stuff and all round nice guy"><img src="{{ IMAGES_PATH }}/Robert_Louis_StevensonJune_1885.jpg" alt="Robert-Louis-StevensonN"></a>

If you look at the broken history displayed on my twitter feed, I have posted a number of blog posts on wordpress... and ghost... and yeah.

I have been looking for a blog tool that keeps history and uses markdown and is simple to use, transparent with what I have done and deleted and... it's just [Github pages](https://pages.github.com/), ok?

------------------
Setup GitHub pages
------------------

<https://pages.github.com/>

Use this to setup a static site on github:

1. Log into GitHub and setup a repository called `<Your github user name>.github.io` ie for me this was `FinnAngelo.github.io`
2. As per usual, I put in an MIT License but I left out an ignore file.
3. Add an `index.html` with `hello world!`
4. Browse to <http://finnangelo.github.io>
5. Done!  

### Gotchas ###

1. I initially set the repository name to just `finnangelo` which obviously didn't work. Fortunately it's 
	easy to rename a repository in GitHub, under settings

------------------------
Setup Jekyll onto GitHub
------------------------

I forked the `jekyll-now` project from 
<https://github.com/barryclark/jekyll-now>

1. I used TortoiseGit to do the forking -> merging thing, which is very easy.  
	Its' mostly like <https://help.github.com/articles/merging-an-upstream-repository-into-your-fork/>, but using TortoiseGit to do the work
	* All the commits etc go to the `master` trunk, because this is a user/organisation site 
	* I had to merge the `index.html` that I set up for `hello world` which wasn't hard, but I'm still not sure about the MIT License. Do I just append my name onto @BarryClark or do I setup a separate license? Or what?
		* It looks like I leave it EXACTLY as is 
			ie no change as per the instructions in it. Doh! 
		* I guess this is a lesson learned for my very first fork!
	* Unfortunately it kept the history for barryclark on the fork which I guess 
		is a feature. 
2. I deleted the Git History using the approach from <http://stackoverflow.com/questions/9683279/make-the-current-commit-the-only-initial-commit-in-a-git-repository> 
	* Note that this is all done in the git-bash terminal on my windows Virtual box	

```bash

# Step 1: remove all history
rm -rf .git

# Step 2: reconstruct the Git repo with only the current content

git init
git add .
git commit -m "Initial commit"

# Step 3: push to GitHub.

git remote add origin <github-uri>
git push -u --force origin master

```

---------------------------------------------------------------------------
Setup <http://www.finnangelo.com> to point to <http://finnangelo.github.io>
---------------------------------------------------------------------------

<https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/>

### Setup a CNAME ###

<https://help.github.com/articles/adding-a-cname-file-to-your-repository/>

### Gotchas ###

1. I haven't set up the `cname` properly at all - every time I update this github repostitory I get an email 
	pointing this out. It's kind of embarrassing that I haven't fixed this, but its nice that they show 
	they care.

---------------------------------
Setup Jekyll with GitHub Settings
---------------------------------

Bit nervous about this...

---
Fin
---

I am glad I could use the picture of Robert for the head of this article - apparently he was the nicest guy that ever lived in Scotland.

As a bonus, I just loved this image... it totally didn't work for the header, but it's just a great picture:  
<a href="https://www.flickr.com/photos/128224075@N02/15653170045" title="Jeckyll &amp; Hyde by Killa Tequilla, on Flickr" style="float:left; margin-right:3em;"><img src="images/002_Lego_Jekyll.jpg" width="300" alt="Jeckyll &amp; Hyde"></a>

