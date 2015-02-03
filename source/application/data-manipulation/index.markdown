---
layout: page
title: "Common Database Operations"
date: 2015-01-11 17:00
comments: false
sharing: true
footer: true
---

As explained previously, the DB class in Kohana provides fast database operations without any overhead involved, and is ideal for keeping your code portable, as well as easier to read. Below we will show common database operations performed using the DB class.

### Select Queries

By default, the result of a db select operation is a multidimensional array.

			$users = DB::select()->from('users')->execute();
			// $users = array of users, each user record is an array itself


<strong>Selecting records as object array</strong>

			$users = DB::select()->as_object()->from('users')->execute();
			// $users = array of user objects
		
<em>The <code>as_object()</code> method call is not necessary right after select. You can use it anytime before calling <code>execute()</code></em>.

<strong>Selecting a particular list of fields from user table</strong>

			$users = DB::select('id', 'email')->as_object()->from('users')->execute();

<strong>Selecting a field with an alias</strong>

			$users = DB::select('id', array('fullname', 'displayname'))
			
				->as_object()->from('users')->execute();
			// each user object contains 'id' and 'displayname' property

<strong>Using the infamous NOW() expression in select queries</strong>

			$users = DB::select('*', array(DB::expr('NOW()'), 'current_time'))
				->as_object()->from('users')->execute();
			// each user object now also contains 'current_time' property

<em><code>DB::expr()</code> allows using raw database expressions in queries which you do not want to be quoted automatically. Any expression you use with tis method should be escaped/quoted by yourself if it contains a user provided data variable.</em>

<strong>Joins</strong>

			$users = DB::select('users.*', 'addresses.city', 'addresses.zip')
				->as_object()->from('users')
				->join('addresses', 'left')
				->on('addresses.user_id', '=', 'users.id')
				->execute();

<em>We are using a left join in our example as not all users may have an address. Skip the second parameter to use a regular join</em>.


<strong>Using Where clause</strong>

			$active_users = DB::select('users.*', 'addresses.city', 'addresses.zip')
				->as_object()->from('users')
				->join('addresses', 'left')
				->on('addresses.user_id', '=', 'users.id')
				->where('users.status', '=', 1)
				->execute();

<strong>A simple but complex query example</strong>

			$active_users = DB::select('users.*', 'addresses.city', 'addresses.zip', DB::expr('NOW() as current_time'))
				->as_object()
				->from('users')
				->join('addresses', 'left')
				->on('addresses.user_id', '=', 'users.id')
				->where('users.status', '=', 1)
				->and_where_open()
					->where('users.fullname', 'IS NOT', NULL)
					->or_where('users.email', 'IS NOT', NULL)
				->and_where_close()
				->execute();

Other method follow the same pattern, e.g. <code>group_by()</code>, <code>having()</code> etc.

### Insert Queries

			$result = DB::insert('users', array('email', 'fullname', 'status'))
					->values( array('john@mail.com', 'John Connor', 1) )
					->execute();

<em>The second parameter to <code>insert()</code> is optional, and if not provided means you are providing values for all fields of a table.</em>

<em>The return value of <code>insert</code> operation is an array containing the result of the operation, and an auto_increment id value if the insert operation was successful</em>.

### Update Queries

			$user_id = 1;
			$user_data = array('email' => 'sarah@mail.com', 'fullname' => 'Sarah Connors');
			DB::update( 'users' )
				->set( $user_data )
				->where( 'id', '=', $user_id )
				->execute();

<em>The return value of <code>update</code> is an array containing the result of the operation, and the number of rows affected</em>.

For more information, consult the Kohana documentation for details of any API for the database module.

Let's move onto [creating layout template](/application/layouts) for our application to make it look good in a browser and specifying additional information for the site.




