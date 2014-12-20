---
layout: page
title: "Configuring Modules"
date: 2014-12-19 17:00
comments: false
sharing: true
footer: true
---

### What is a Module
A module in a Kohana 3 application is like a package, created for providing a particular functionality. As you would 
guess, Kohana 3 has some of it's own official modules, while there are many others written by other developers which
 you can use in your application. Some examples of a module are:

* <strong>The 'Database' module:</strong> Included with Kohana, this module provides database related functionality 
for your application. The driver based architecture of the module allows for any type of database to be used, 
and the MySQL driver is what we will be using in our application.

* <strong>The 'Image' module:</strong> Also included with Kohana, provides image manipulation functionality like 
resizing, cropping etc.

### Modules vs Plugins
You should not confuse Kohana 3 modules with plugins, for example, as most WordPress developers use 
plugins that extend WordPress functionality. Kohana modules serve as building blocks of your application. 
Unlike plugins which can be enabled/disabled anytime in an application, once you start using a module in your Kohana 3 application, 
disabling it will simply break the site.

### Using Modules in Kohana 3 application
Modules can enabled and disabled in the <code>application/bootstrap.php</code> file. The default bootstrap.php file 
includes all default modules commented out, so it's only a matter of uncommenting few lines to enable those modules. 
The code that does this as follows:

		Kohana::modules(array(
			//'auth'       => MODPATH.'auth',        // Basic authentication
			// 'cache'      => MODPATH.'cache',      // Caching with multiple backends
			// 'codebench'  => MODPATH.'codebench',  // Benchmarking tool
			'database'   => MODPATH.'database',      // Database access
			'image'      => MODPATH.'image',      // Image manipulation
			// 'minion'     => MODPATH.'minion',     // CLI Tasks
			// 'orm'        => MODPATH.'orm',        // Object Relationship Mapping
			// 'unittest'   => MODPATH.'unittest',   // Unit testing
			// 'userguide'  => MODPATH.'userguide',  // User guide and API documentation
			//'email'      => MODPATH . 'email'
		));

As you can see, we have uncommented the 'database' and 'image' module for our test application. We will use the 
modules later in our application and explain their functionality in details.

At this point, if you refresh your browser, you will see no difference in the 'Hello World!' output. That's because 
we have not yet utilized any of module functionality in our application. If the site breaks after enabling one or 
more modules, it simply means the module folder does not exist. Check your Kohana installation again and make sure 
the module is in the right place. The <code>MODPATH</code> constant refers to the default 'modules' folder.

### Module folders
Please note that you need not place a module in the 'module' folder. You can put it anywhere you like, 
just make sure you put the right path in <code>bootstrap.php</code> file for it. For example, 
if you have a module called 'myfirstmod' placed on the root of kohana installation, 
your bootstrap.php file will contain something like this:

		Kohana::modules(array(
			'mymod' => DOCROOT . 'myfirstmod'
		));
		
You will also see that it is not necessary to name your module in the array same as it's folder name. Only thing to 
check for is that all the modules you use in your application have unique name in bootstrap.php code.

It's time to learn a few [Kohana basics](/application/basics), and then we can start writing our own code 
for the application.













