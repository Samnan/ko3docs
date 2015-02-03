---
layout: page
title: "Using Layout Templates"
date: 2015-01-10 17:00
comments: false
sharing: true
footer: true
---

Unlike other frameworks out there which provide built-in view parsers and template languages, Kohana 3 has a very simplistic template based controller for a start. It is so simple you can use it to create your own customized layout and template logic. Also, Kohana works neatly with third party templat engines (like smarty) and there are many modules available that allow quick integration of any of these template engines into your Kohana engine.

Since we are more concerned on getting to know Kohana better, we will use Kohana's built-in template controller to create our sample application layout. There are three steps involved in the process:

* Creating a Template Controller
* Creating the view template
* Using the Template Controller


### Creating a Template Controller
As a good practice, we are going to first create a new Controller for our application called Controller_Common. It will be derived from Kohana's Controller_Template, and then we will add a bit of code to make it work for our application.
The code for Controller_Common is as follows:

			<?php defined('SYSPATH') or die('No direct script access.');

			class Controller_Common extends Controller_Template {

				public $template = 'templates/default';

			} // End Common

Surprised? Yes, that is how simple it is to have a template controller in your application. The only explanation required here is for this line:

			public $template = 'templates/default';
			
This line tells our common controller that we will be using the view <code>view/templates/default</code> as our application layout template. The rest is taken care by the template controller. All we need to do now is create out default view template and use it in our controllers.

### Creating the view template
Like any other view, we will create a new file 'default.php' in the folder <code>views/templates</code>. The code for the view is as follows:


			<!DOCTYPE html>
			<html lang="en">
			<head>
				<meta charset="utf-8">
				<title>Sample Kohana 3 Application</title>
				<meta name="author" content="Samnan ur Rehman">
				<meta name="description" content="A sample Kohana 3 application">
				<link href="<?php echo Kohana::$base_url; ?>css/default.css" rel="stylesheet" type="text/css">
			</head>
			<body>
				<?php echo isset($content) ? $content : ''; ?>
			</body>
			</html>

There are two items that require explanation here:

* <strong>Kohana::$base_url: </strong>Adding this before any link makes the link 'proper' by prepending the application's complete url before the link. It is recommended to use it before any css/javascript url in your application, so that there path is always correct.
* <strong>$content: </strong>This is the actual view for each page that will be embedded within your template by the template controller (which is explained below).

### Using the Template Controller
After making that above changes, if you go back and run your application, you will it works without any changes and the browser shows the same output as before. This is because all our controllers are derived from the 'Controller' class.

So, in order to use our layout logic, we first change our home and users controller and make it inherit the Controller_Common class.


			class Controller_Home extends Controller_Common {
			...
			
			
			
			class Controller_Users extends Controller_Common {
			
			...

Now the only thing left is to populate the '$content' variable for our template in the controllers. As you will recall, we previously used <code>$this->response->body()</code> to set our application output. Now that we are using a template controller, we will change that line to look like this:


			// $this->response->body($view);
			$this->template->content = $view;

And we are done. If you go back and refresh the application in browser, and view it's source, you will see the page properly starts and ends with &lt;html&gt; tags and contains other basic html tags, along with the page output.

For all our future work, we will be deriving all our controllers from Controller_Common, and we will use the above code to use our template based output in browser.

<em>You can go ahead and at this point create the 'css' folder along with the 'index.php' file, and put 'default.css' in it with some styles. After that refresh your browser to see the css inddeed loads correctly. If it does not, recheck your code to make sure you follow the guidelines above</em>.


We are now ready to do more complex operations in our application, and before we do that, let's see a few [How-to's and mostly used stuff](/application/how-tos) in Kohana 3.




