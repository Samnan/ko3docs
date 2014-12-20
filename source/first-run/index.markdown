---
layout: page
title: "Running your Kohana Application"
date: 2014-12-19 17:00
comments: false
sharing: true
footer: true
---

### Testing the application
* Navigating to <code>http://localhost/kohana/</code> in your browser should show 'Hello world!'.

* If you have configured Kohana 3 application on a virtual host, then the virtual host url should show the same 
results as above. For example, if your virtual host is configured as <code>http://kohana3.localhost</code>, 
then running the url in browser should show 'Hello World!'.

### Troubleshooting
If you do not get the usual 'Hello World!' from the Kohana application output, then you should check and make sure:

* You have set the correct 'base_url' in <code>application/bootstrap.php</code> file.
* You have set the correct REWRITE_BASE configuration directive in <code>.htaccess</code> file (if you are using the 
.htacess file for clean urls).


We are now ready to [configure modules](/configuration/modules) for our application before we start working on the 
rest of the application code.