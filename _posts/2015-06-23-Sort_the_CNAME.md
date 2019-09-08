---
layout: post
title: "Sort the CNAME of http://www.finnangelo.com"
published: true
---
Groan.

For ages I have been getting these `Page build warning` emails from GitHub, saying that my custom domain [www.finnangelo.com](http://www.finnangelo.com) was set up as an A record.  
It's been bugging me, but really hasn't been a priority. So by following the broken windows theory, lets get this fixed.

----------------------------------------

## [Adding a CNAME file to your repository](https://help.github.com/articles/adding-a-cname-file-to-your-repository/) ##

I had this all set up with the `CNAME` file in the `FinnAgelo.github.io` root, but obviously my `Next steps: Configuring DNS settings` was a bit broken

### Gotchas ###

01. In my Zone Management pages with my domain provider, I had added 2 `A` records with the <pages.github.com> IP Addresses
02. I found they belonged to that url with the classic `ping -a 192.30.252.153` on the cmd line, but I could just as easily have looked up <https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/>
03. I deleted them
04. I tried to add a cname, but my domain provider needs an mx hostname?
05. And I'm still getting the emails - I might have to wait until the propagation is finished...?

Hah! My DNS recoreds and stuff are actually located at Namecheap, not my domain provider...

01. Got to namecheap > Login > Menu > Manage Domains > FreeDNS.Hosted Domains
02. <https://www.namecheap.com/support/knowledgebase/article/settingup_hostrecords>
03. | Hostname | IP Address URL               | Record Type     | MX Pref
    | -------- | ---------------------------- | --------------- |--------
    | @        | <http://www.finnangelo.com.> | URL Redirect    | NA
    | www      | finnangelo.github.io.        | CNAME (alias)   | NA

And it updates _without_ sending me the `A Record` error - fantastic!
