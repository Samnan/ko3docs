---
layout: page
title: "Configuring Kohana installation"
date: 2014-12-19 17:00
comments: false
sharing: true
footer: true
---

You can run your newly installed Kohana 3 application in two ways, either as a subfolder on your webserver, or as a virtual host.

### Running Kohana application inside a subfolder
* Edit <code>application/bootstrap.php</code> file and locate the following code:

		Kohana::init(array(
			'base_url'   => '/kohana/',
		));

If your subfolder name is anything other than kohana, use that folder path in the 'base_url' settings. For example, if you have installed KO3 in
the folder "myapps/testing/kohana3" inside your webserver document root, then change the bootstrap.php code to look like this:
		
		Kohana::init(array(
			'base_url'   => '/myapps/testing/kohana3/',
		));

### Running Kohana application as a virtual host
* Edit <code>application/bootstrap.php</code> file and locate the following code:

		Kohana::init(array(
			'base_url'   => '/kohana/',
		));

Change it to
		
		Kohana::init(array(
			'base_url'   => '/',
		));

### [optional] Enable REST urls
If you would like your application to use REST urls, you will need to do a bit more work.
Edit <code>application/bootstrap.php</code> file again, and change <code>Kohana::init</code> code to look like this:

		Kohana::init(array(
			'base_url'   => '/',
			'index_file' => FALSE
		));	

Rename <code>example.htaccess</code> file in the kohana folder to <code>.htaccess</code> for the REST urls to work.
If you are running your application inside a subfolder (not as a virtual host), then edit <code>.htaccess</code> file and change the line:

		RewriteBase /
		
To:

		RewriteBase /myapps/testing/kohana3/

Proceed to [Running your Kohana 3 application](/first-run) to see if everything worked out fine.
If something does not work, review your code again for typos and incorrect paths, fix them and try again.