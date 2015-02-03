---
layout: page
title: "Creating Routes"
date: 2015-01-11 17:00
comments: false
sharing: true
footer: true
---

As explained previously, Kohana 3 has a powerful route system, and while you can have the 'default' route for most of your puropses, sometimes a specialized route is needed to perform specific operations.

### Default route functionality

The default route included in <code>application/bootstrap.php</code> file allows using any number of controllers and methods within them. Also, there is an additional 'id' parameter that you can provide in the URL and use that id parameter in your code for any purpose.

For example, in our sample application, we have a url like <code>/users</code>. Let's assume we want to add a new url to our application at <code>/users/info</code>, which will take an id after the url and display the user's information.

We will modify the Users controller and add a new method for this purpose.


		class Controller_Users extends Controller_Common {
		
			public function action_info()
			{
				// try to get the id from url parameter
				$user_id = $this->request->param('id');
				
				// show 404 page if user_id is either not passed through the url, or is not a valid numeric id value
				if ( !$user_id || !Valid::digit($user_id) ) {
					throw Http_Exception::factory(404, 'Invalid or missing User ID [:id]', array(':id' => $user_id));
				}
				
				// use the user id
				$user = Model::factory('users')->find_by_id($user_id);
				...
				...

			}
			
		}

As you can see, we use the 'param' method of Request class to get our id parameter. First we validate it to make sure we have a valid numeric id passed, and then process the id further after validation.

### Specialized Routes

While the default route is pretty ok for most purposes, often you will need to create some special purpose routes. For our example, we will try to solve the problem of generating an image thumbnail from a large image source through our application.

Our image urls will be of the following nature:

<code>/images/one-fine-image/1024/768/png</code>

<code>/images/another-image/1024/768/jpg</code>

<code>/images/image_with_underscores/800/600</code>

We can see that our image urls have these specification:

* url must start with 'images/'
* second parameter must be a file name, and can only contain alphanumeric characters, dashes and underscores
* third parameter is the width of the image
* third parameter is the height of the image
* fifth parameter specifies the image file type. Also this an optional parameter, so if you do not specify it, a jpg version of the image will be returned

Now for our purpose, we will first create a new route in <code>application/bootstrap.php</code> before the default route like this:


			Route::set(
				'image-thumb',
				'images/<filename>/<width>/<height>(/<filetype>)',
				array(
					'filename' => '[a-zA-Z0-9_-]+',
					'width'    => '\d+',
					'height'   => '\d+',
					'filetype' => 'jpg|png|gif'
				)
			)
			->defaults(array(
				'controller' => 'Images',
				'action'     => 'index',
				'filetype'   => 'jpg'
			));

Here, 'image-thumb' is a friendly name for our route. The second parameter specifies the url structure, while the third parameter defines the format of each parameter.

The <code>->defaults()</code> method sets the default value for route, and as you can see, we specify the filetype to be 'jpg' if it is not provided.

The code for Image controller will look something like this:


			<?php defined('SYSPATH') or die('No direct script access.');

			class Controller_Images extends Controller {

				public function action_index()
				{
					$filename = $this->request->param('filename');
					$width    = $this->request->param('width');
					$height   = $this->request->param('height');
					$filetype = $this->request->param('filetype');
					
					....
				}

			} // End Images

Kohana takes care of making sure the filename parameter does not contain any invalid characters, and the width and height are valid numbers. The only thing you will need to check is if the file really exists since the filename parameter can be any combination of numbers, characters, dashes and underscores.

<em>There is a <code>filter()</code> method available that you can use in your route definition to even validate the route parameters, before they are passed to your application. For our example, we have ignored that method specifically to keep this example simple enough to understand</em>.



The next section explains [HMVC requests](/application/hmvc) in Kohana 3 in details.




