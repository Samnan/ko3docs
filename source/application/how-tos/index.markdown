---
layout: page
title: "Mostly Used Stuff"
date: 2015-01-10 17:00
comments: false
sharing: true
footer: true
---

### Redirecting to a new URL

			// anywhere, but preferrably inside a controller
			HTTP::redirect('/somehwere-else/');
			
### Check if the user has requested the page through ajax

			// inside a controller
			if ($this->request->is_ajax()) { // do some stuff }
			
			// anywhere else
			if (Request::initial()->is_ajax()) { // do some stuff }

### Make the content download instead of sending to the browser

			$data = 'some data generated or fetched from somewhere';
			$this->response->body($data);
			$this->response->send_file(TRUE, 'data.txt');  // data.txt will be downloaded by browser, containing contents of the $data variable
			
			$filepath = DOCROOT . 'downloads/myfiles/data.zip';  // just an example
			$this->response->send_file($filepath, 'data.zip');   // data.zip will be downloaded by browser (you can skip the second parameter also)
			
			// delete the file after sending to the browser automatically
			$this->response->send_file($filepath, 'something.zip', array('delete' => TRUE));

<em>Ideally you will almost always use download functionality inside your controller. Will be really strange to be using it elsewhere!</em>.

### Getting configuration variables

			// get the name of default database our application uses
			$db_name = Kohana::$config->load('database.default.connection.database');
			
			// get the default database character set being used
			$db_charset = Kohana::$config->load('database.default.charset');
			
			// get the whole database configuration
			$db_info = Kohana::$config->load('database');

### Logging errors and messages

			Kohana::$log->add(Log::ERROR, 'something wrong happened');
			
			Kohana::$log->add(Log::INFO, 'user registration completed');
			
			Kohana::$log->add(Log::INFO, 'user :user logged in at :time', array(
				':user' => $user->login_id,
				':time' => date("Y-m-d H:i:s")
			));

			Kohana::$log->add(Log::DEBUG, 'debug information for some reason');

<em>You can control whether the log statements in your code actually go into the log or get ignored. See Kohana documentation of <code>Log</code> class to find out how to do that</em>.

### Using Session variables

			$session = Session::instance();  // session type depends on the config you set (e.g. native PHP sessions, stored inside db etc)
			
			$session->set('user_id', $user->id);
			
			$user_id = $session->get('user_id');
			
			$session->delete('user_id');
			
### Exceptions

			// throw a 404 exception for a missing downloadable file
			if (!file_exists($download_file)) {
				throw new Http_Exception_404('Download file invalid: :file',
					array(':file' => $download_file)
				);
				// another way to do the same
				throw Http_Exception::factory(404, 'Download file invalid: :file',
					array(':file' => $download_file)
				);
			}
			

In the next section, we will be [creating additional routes](/application/routes) to see the power of Kohana 3 routes and their use in a real application.

