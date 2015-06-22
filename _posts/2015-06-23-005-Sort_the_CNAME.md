---
layout: post
title: "#005: Sort the CNAME of http://www.finnangelo.com"
published: false
---
Groan. 

For ages I have been getting these `Page build warning` emails from GitHub, saying that my custom domain <www.finnangelo.com> was set up as an A record.  
It's been bugging me, but really hasn't been a priority. So by following the broken windows theory, lets get this fixed.

----------------------------------------
[Adding a CNAME file to your repository](https://help.github.com/articles/adding-a-cname-file-to-your-repository/)
----------------------------------------

I had this all set up with the `CNAME` file in the `FinnAgelo.github.io` root, but obviously my `Next steps: Configuring DNS settings` was a bit broken

### Gotchas ###

01. In my Zone Management pages with my domain provider, I had added 2 `A` records with the <pages.github.com> IP Addresses
02. I found they belonged to that url with the classic `ping -a 192.30.252.153` on the cmd line, but I could just as easily have looked up https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/ 
03. I deleted them
