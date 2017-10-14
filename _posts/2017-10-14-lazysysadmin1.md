---
layout: post
title: Vulnhub LazySysAdmin 1
---

This pretend to be the walkthrough of LazySysAdmin vulnhub machine. As every penetration test, the first step was to make a ports and service discovery in order to detect misconfigured or vulnerable applications running on the server. As it's shown below, a web application is running on 80/tcp port which seems to be generated using Silex cms generator.

![evidence1]({{ site.baseurl }}/images/lazysysadminI_01.png)

That web application seems to be static-based, so we continue finding the way. If we use a web crawler tool, such as dirb or dirbuster on port 80/tcp, we can find differents directories that revelate the use of Wordpress cms and Phpmyadmin service. The problem was that Wordpress instance seems to be update and wp-scan didn't show us nothing interesting (no core or plugins-based exploits found).

![evidence2]({{ site.baseurl }}/images/lazysysadminI_02.png)

Trying to log into mysql from the attacker machine was impossible due to it's seems to be used a black lists and my IP was banned. So, in this point I was confuse just because I had an static web application running on port 80/tcp, a well-done bastioned Wordpress instance, an "impossible" MySQL service...so, What can I do? Try harder :) . I was deeper in the information-gathering step and I could perform a samba null session which gave me a valid username of the host, "togie" (the name of "togie" appear in some Wordpress pages as well).

![evidence3]({{ site.baseurl }}/images/lazysysadminI_03.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
