---
layout: page
title: "Creating Models"
date: 2015-01-10 17:00
comments: false
sharing: true
footer: true
---

Before we create our first model, you should remember that there are three ways to encapsulate database functionality in your Kohana application. They are described below:

*<strong>Models: </strong>Models provide a simplistic approach to query databases and perform other data manipulation operations in your application. Models are also the fastest performance wise, since they do not add any overhead to your database code, but you need to write relatively more code inside models as opposed to other methods.

*<strong>ORM: </strong>The official ORM module provided with Kohana provides functionality similar to other object relational mapping libraries that you will find for PHP. Since using ORM is the simplest approach, and there are tutorials in the Kohana documentation for utilizing ORM module, we will stick to using plain models so we can understand the database module in Kohana 3 in detail. 

* <strong>Third Party Modules: </strong> Like the official ORM module, there are many other ORM and database wrappers available for Kohana 3 on the web. You can try out any of them to see if you like a particular module instead of using the Kohana provided database modules. 

### Database configuration
Before we begin creating our first model, we must setup our database access by specifying the database configuration for our application. To do this, first copy the file <code>modules/database/config/database.php</code> to your <code>application/config</code> folder. After that, edit the file you just put in your <code>application/config</code> folder, and change the settings to match for your database server.

### Creating your first Model
All models in Kohana follow the same file naming convention like controllers, and go inside <code>application/classes/Model</code> folder. For our application, we will create our model file called 'Users.php'. The code for the file is as follows:


		<?php defined('SYSPATH') or die('No direct script access.');

		class Model_Users extends Model {

			public function find_all()
			{
				return DB::select()
					->as_object()
					->from("users")
					->execute();
			}

		} // End Model_Users

<em>You can view Kohana documentation on the details of using the DB class. Also note that DB and Database are two separate classes. 'DB' class provides portable use of database, while the 'Database' class is the underlying wrapper for basic database connection.</em>

The structure for the 'users' and addresses table we use in our sample application is as below:
	
			CREATE TABLE `users` (
				`id` int(5) UNSIGNED NOT NULL AUTO_INCREMENT,
				`email` varchar(50) NOT NULL,
				`fullname` varchar(25),
				`status` int(1) UNSIGNED DEFAULT 1 NOT NULL,
				PRIMARY KEY (`id`),
				UNIQUE KEY(`email`)
			);
			
			
			CREATE TABLE `addresses` (
				`id` int(5) UNSIGNED NOT NULL AUTO_INCREMENT,
				`user_id` int(5) NOT NULL,
				`city` varchar(50),
				`zip` varchar(10),
				PRIMARY KEY (`id`)
			);
			
<em>You should add a few records in the tables after you create them, so that you can see the output when we use data from the model in our views.</em>

Now that we have the model created, let's use it in our controller. Create a new controller called 'Users' in the <code>classes/Controller</code> folder. The filename would be 'Users.php' and the code inside it as follows:

		<?php defined('SYSPATH') or die('No direct script access.');

		class Controller_Users extends Controller {

			public function action_index()
			{
				$users = Model::factory('users')->find_all();
				$view = View::factory('users/list', array('users' => $users));
				$this->response->body($view);
			}

		} // End Users

You can see that our view name in the controller is 'users/list', instead of users. This simply indicates that 'users' is a subfolder in views and it contains the view file called 'list.php'.

The code for 'list.php' is as follows (make sure the file is inside the folder <code>application/views/users</code>):

		<h1>User List</h1>
		<table>
			<thead>
				<tr><th>ID</th><th>Email</th><th>Full Name</th><th>Status</th></tr>
			</thead>
			<tbody>
		<?php foreach($users as $user): ?>
			<tr>
				<td><?php echo $user->id; ?></td>
				<td><?php echo HTML::chars($user->email); ?></td>
				<td><?php echo HTML::chars($user->fullname); ?></td>
				<td><?php echo ($user->status == 1) ? 'Active' : 'Inactive'; ?></td>
			</tr>
			</tr>
		<?php endforeach; ?>
			</tbody>
		</table>

<code>HTML::chars()</code> is a utility function in Kohana to escape characters properly in html before display. It is necesary to use it (or any other function you like) to avoid security problems with user data in your application.
You will also note that each user record is an object, since we specifically used <code>DB::as_object()</code> method in our controller to fetch user records in object form.

In the next section, we explain some [mostly used database operations](/application/data-manipulation) to show how to perform data manipulation using the DB class, before we use a template controller to make our application output look good in a browser.




