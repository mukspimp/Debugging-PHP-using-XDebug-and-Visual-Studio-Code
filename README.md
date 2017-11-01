# Debugging-PHP-using-Xdebug-and-Visual-Studio-Code
Debugging PHP using Xdebug and Visual Studio Code

Are you suffering from PHP debugging pain using var_dump(); echo “foo”; die();  :-(

Let’s move to VS Code using PHP xDebug and have funny debugging.. :-)

1) Visual Studio Code Installation  (source - https://code.visualstudio.com/docs/setup/linux) 	

a) Download latest version https://go.microsoft.com/fwlink/?LinkID=760868

b) Installing the .deb package will automatically install the apt repository and signing key to enable auto-updating using the regular system mechanism.

	sudo dpkg -i <file>.deb
	sudo apt-get install -f # Install dependencies
	
   OR 
   The repository and key can also be installed manually with the following script:
	
	curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
	sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
	sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
	
c) Then update the package cache and install the package using:

	sudo apt-get update 
	sudo apt-get install code # or code-insiders
	
d) And then run command: $ code  in terminal and your VC Code window open..
	
	code
	
2) Search the following VS Code Extensions and Install: 

	PHP Debug  - PHP Debug Adapter for Visual Studio Code
	
	PHP IntelliSense (optional)
	
	ESLint - VS Code ESLint extension (optional)

3) After installing extensions, we overwrite User Settings in VS Code for that:
	Open User Settings in VS Code: press F1 and 
	search “user” and click on Preferences: Open User Settings option in user settings add: 
	
		{
		"php.validate.executablePath": "/usr/bin/php"
		}

4) Installing and configuring Xdebug extension for PHP Using Xdebug installation wizard  

In terminal enter $php -i which gives you phpinfo in terminal
	
	php -i
	
Copy terminal php info.
Open https://xdebug.org/wizard.php and pest in the box and click on Analyse My phpinfo()  button, you get the instruction for installing as below: (source - https://xdebug.org/wizard.php) 

These are instructions for PHP7.1  
 (You may get different version xdebug or commands depending on your PHP version, my is PHP7.1)

a. Download xdebug-2.5.4.tgz

b. Unpack the downloaded file with 
	
	tar -xvzf xdebug-2.5.4.tgz

c. Run: 
	
	cd xdebug-2.5.4

d. Run: 
	
	phpize 

   Note - If you don’t have phpize here is the command for installing: 
   
	For 7.0 - 

		sudo apt install php7.0-dev
		sudo service apache2 restart

	For 7.1 - 

		sudo apt install php7.1-dev
		sudo service apache2 restart

e. As part of 'phpsize' its output it should show:

f. Configuring for:
	Zend Module Api No:      20160303
	Zend Extension Api No:   320160303


g. Run following commands:

		./configure
		make
		cp modules/xdebug.so /usr/lib/php/20160303
	
h. Update file '/etc/php/7.1/cli/php.ini' and change the line "Zend_extension = /usr/lib/php/20160303/xdebug.so"
	 
	
5) Update '/etc/php/7.1/apache2/php.ini' and add the lines
	[XDebug]
	xdebug.remote_enable = 1
	xdebug.remote_autostart = 1
	xdebug.remote_host = localhost


6) Configuring xDebug with PHP7 
	
	i) A simple apt-get command:  
			
		sudo apt-get install php-xdebug 
	
	ii) You should check your PHP modules list and see xdebug in there now:   
		
		php -m 

	iii) You will also need to restart Apache2: 
		
		sudo service apache2 restart

	iv) We enable stack trace manually open xdebug.ini file : 
		
		vim /etc/php/7.0/mods-available/xdebug.ini
	
	 v) And add line in xdebug ini file: 
	 	
		xdebug.show_error_trace = 1

7) Visual Studio Code debugger setting 
     In VS Code go to debug icon and open launch.json by clicking debug setting icon
     Add the below configuration in launch.json file and save the file
     
		{
		   "version": "0.2.0",   
		   "configurations": [
		     {
		       "name": "Listen for XDebug",
		       "type": "php",
		       "request": "launch",
		       "port": 9000,
		       "log": true
		     },
		     {
		       "name": "Launch",
		       "request": "launch",
		       "type": "php",
		       "program": "${file}",
		       "cwd": "${workspaceRoot}",
		       "externalConsole": false
		     }
		   ]
		 }
 
8) You have Done it…. Now start debugging.... :-) 

Note - If you stuck somewhere please check PHP version and you are adding xdebug configuration in same version php.ini
