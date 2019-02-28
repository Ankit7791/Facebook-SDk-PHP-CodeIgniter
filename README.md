# Facebook-SDk-PHP-CodeIgniter
Integrating Facebook Login into PHP CodeIgniter

-----------------Login with Facebook in Codeigniter-----------------

1. First of all go to https://developers.facebook.com, Login with facebook account and create a new App.
2. After the App is created successfully, you will get the App Id and App Secret Key, which we will be using in the library.
3. After creating the app, go to Setting > Basic, insert the App Domains on which you want to use the app, add the Privacy Policy URL, select the category.
4. Select 'Provide services to other businesses' in the Business Use option.
5. Then after, add a platform, select website and enter the Site Url(The Url which you will be accessing to allow user to login wth Facebook).
6. Save the Changes.
7. After that in the left panel, select Facebook login > Settings, enable Client OAuth Login, enable Web OAuth Login, enable Embedded Browser OAuth Login, disable Force Web OAuth Reauthentication.
8. Add the Valid OAuth Redirect URIs(The same URL that you entered in the Site Url in the 7th step.
9. Save the Changes.
10. Change the Status to live in the Status bar.

-----------------Move to the Ci Project Folder-----------------

1. Move the attached files to your project folders as guided.
2. Move facebook.php to your config folder.
3. Move Facebook.php to your libraries folder.
4. Download and install composer from https://getcomposer.org
5. Composer is the recommended way to integrate the Facebook SDK with PHP. So, create a new file composer.json in the root of your project directory(project_name/application/composer.json) and please add and save the following code to file.
	{
    		"require" : {
        		"facebook/graph-sdk" : "~5.0"
    		}
	}
6. Now open command prompt and enter into your project folder.
7. After reaching the project folder, enter the command
	cd application
and press enter
8. Now we need to install the composer to our project.
9. So, enter the command 
	composer install
and press enter. This will install the composer on your project. As soon as the installation finishes, to verify whether it is installed or not go to the project folder > application, here you should see the folder named vendor, which specifies that the composer is installed successfully to your project.
10. Then after, in the config.php file change the $config['composer_autoload'] from FALSE to TRUE.
11. Now, you need to copy the App Id and App Secret Key to the facebook.php file in the config folder.
12. Update these values with the ones showing on your created app, $config['facebook_app_id'] = '****************',$config['facebook_app_secret'] = '**********************************';
13. Set $config['facebook_login_type'] to 'web'.
14. Set $config['facebook_login_redirect_url'] to the Controller name which will be redirected after clicking the login button.
15. Set $config['facebook_logout_redirect_url'] to the Controller name which will be redirected after the logout event.
16. Set $config['facebook_permissions'] to array('email')
17. Set $config['facebook_auth_on_load'] to TRUE
18. Now in the autoload.php file, add 'facebook' to $autoload['libraries'].
19. Create a view > signin.php.
20. <h1>Welcome to facebook</h1>
	<a href="<?php echo $this->facebook->login_url(); ?>">Login with facebook</a>
21. Now creating a Controller to allow the user to login to the project via facebook.
22. controllers > Signin.php
	<?php

	class Signin extends CI_Controller
	{
		public function index()
		{
			// Check if user is logged in
			if ($this->facebook->is_authenticated())
			{
				// User logged in, get user details
				$user = $this->facebook->request('get', '/me?fields=id,first_name,last_name,email,picture');
				if (!isset($user['error']))
				{
					$data['user'] = $user;
				}
				// display view
				$this->load->view('user_data',$data);
			}
			else
			{
				$this->load->view('signin');
			}
		}
		public function logout()
		{
			$this->facebook->logout_url(); //Used for logout process
			$this->facebook->destroy_session(); //Used for destroying the session

		}
	}
	?>
23. Create a view > user_data.php to show the details of the user logged in.
24. <h1>Welcome <?=$user['first_name'].' '.$user['last_name']; ?></h1>
	<p>Email Address: <?= $user['email'];?></p>
	<p>Picture: <img src="<?=$user['picture']['data']['url'];?>"></p>
	<a href="<?= base_url(); ?>Signin/logout">Logout</a>
25. After the login you should now see the data of the user login.
26. Click the logout button to destroy the session and logout the user from project.

