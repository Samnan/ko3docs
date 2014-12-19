---
layout: page
title: "Installing Kohana 3"
date: 2014-12-19 16:09
comments: false
sharing: true
footer: true
---

There are two basic ways to install Kohana 3.

* Download the latest KO3 package from its [website](http://kohanaframework.org/download). Extract it into the web server document root, or any subfolder if you like.
	
* Clone the Kohana 3 application repository from GitHub. In your console/terminal, change directory to webserver document root (or subfolder) and run this command:

		# assuming your webserver document root is /var/www/
		cd /var/www/
		git clone git@github.com:kohana/kohana.git

Wait for the download to complete. After that, change directory to kohana and initialize and download it's dependencies:
		
		cd kohana
		git submodule update --init
	
This will take a little time, once it's done, proceed to [configuring Kohana](/kohana-configuration).