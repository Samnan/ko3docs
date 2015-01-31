---
layout: page
title: "Kohana Basics"
date: 2014-12-19 17:00
comments: false
sharing: true
footer: true
---

Before you begin writing code for your application, you need to know some specifics about Kohana 3 so that you can utilize the framework to your advantage. 

* <strong>Routes:</strong> Routes are one of the strongest suite of KO3. Every application must have one route (which is defined in <code>application/bootstrap.php</code> for you). Apart from that, you can create specific routes for different parts of the application. Routes will be dealt in details later on, so for now you can safely continue building your application with just the default route.

* <strong>Transparent Extension:</strong> Another cool feature of KO3 is the ability to allow extending the core functionality with minimum fuss. We will take a look at this later also, when we develop our sample application.

* <strong>HMVC: </strong>HMVC is the ability to use any request in the application internally in a clean manner. Again, we will use this later on to show you how to use Kohana requests internally to your advantage.

### MVC architecture
Just like any other MVC framework, every request in Kohana is handled by a controller, which can utilize models and views to generate the desired output for the web application. A sample controller is provided with the Kohana installation, so you can use it as a starting point.

### Creating your first controller
In the application folder in a default Kohana installation, you will find one controller named Welcome.php. For our application, we want our homepage controller to be called Home, so we make that change by following these steps:

* change the <code>default</code> route right at the end in <code>application/bootstrap.php</code> to use Home controller instead of Welcome controller.

		Route::set('default', '(<controller>(/<action>(/<id>)))')
			->defaults(array(
				'controller' => 'home',  // change from welcome to home
				'action'     => 'index',
		));

 
* Rename Welcome.php to Home.php in the <code>application/classes/Controllers</code> folder.
* Edit Home.php, and change the name of the controller class to Controller_Home, so that the code becomes: 

		<?php defined('SYSPATH') or die('No direct script access.');
		
		class Controller_Home extends Controller {
		
			public function action_index()
			{
				$this->response->body('hello, world!');
			}
		
		} // End Home

You will notice that there is no php end tag at the end of the file, instead just a comment. The end tag is optional which is a php feature, and the comment is a Kohana convention and completely optional, so you remove it if you like. We have left it here and just changed it to match the controller name so that we know it is the end of the controller code.

### Using a View class
As you can see, our homepage just outputs a classic greeting instead of doing something really useful. We are going to change that to utilize a View and output a nice html code for a start. To do that, first create a file called 'home.php' inside <code>application/view</code> folder. Put the following simple HTML code in the home.php file.
	
		<h1>Welcome to Kohana 3</h1>
		<p>This is the homepage</p>
		<p>The view file for the homepage is located at <?php echo $file; ?></p>
		
<em>Please note that the name of the view file need not be the same as controller. You can name it whatever you like.</em>

### Using the view in a controller
Now that we have created the view file, we will change our controller code to use the view as our output. You will also see that we will pass a <code>file</code> parameter to the view, which is then displayed on the page. Change the code inside <code>application/classes/Controller/home.php</code> as below:

	
		class Controller_Home extends Controller {
				
			public function action_index()
			{
				$file = Kohana::find_file('views', 'home');
				$view = View::factory('home', array('file' => $file));
				$this->response->body($view);
			}
				
		} // End Home

<em></em>

In the next section, we will learn how to [create models](/application/models) to use database related functionality in our application.




