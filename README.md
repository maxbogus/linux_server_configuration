# linux_server_configuration
* The IP address and SSH port so your server can be accessed by the reviewer.
	* IP: 52.33.105.217 
	* Port: 2200
* The complete URL to your hosted web application: http://ec2-52-33-105-217.us-west-2.compute.amazonaws.com/
* A summary of software you installed and configuration changes made:
	* Preparation of dev environment:
		* Create new development environment.
		* Download private keys and write down your public IP address.
		* Move the private key file into the folder ~/.ssh:
			* <pre>$ mv ~/Downloads/udacity_key.rsa ~/.ssh/
		* Set file rights (only owner can write and read.):
			* <pre>$ chmod 600 ~/.ssh/udacity_key.rsa
		* SSH into the instance:
			* <pre>$ ssh -i ~/.ssh/udacity_key.rsa root@PUPLIC-IP-ADDRESS
	* Created user grader
		* <pre>$ adduser NEWUSER
	* Gave new user the permission to sudo
		* Open the sudo configuration:
			<pre>* $ visudo
	* Updated list of software and upgraded all local software.
		* <pre> $ sudo apt-get update
		* <pre> $ sudo sudo apt-get upgrade
	* Configurated ssh access:
		* Opened the config file:
			* <pre> $ vim /etc/ssh/sshd_config
		* Made following changes:
			* Change to Port 2200.
			* Change PermitRootLogin from without-password to no.
			* Temporalily change PasswordAuthentication from no to yes.
			* Append UseDNS no.
			* Append AllowUsers NEWUSER.
		* Restarted SSH Service:
			* <pre> $ /etc/init.d/ssh restart
	* Created SSH Keys:
		* Generated a SSH key pair on the local machine:
			* <pre> $ ssh-keygen
		* Installed ssh-copy-id (in my case i had a mac) and copied public key to the server:
			* <pre> $ brew install ssh-copy-id
			* <pre> $ ssh-copy-id username@remote_host -p**_PORTNUMBER_**
		* Logined with the new user:
			* <pre> $ ssh -v grader@PUBLIC-IP-ADDRESS -p2200
		* Opened SSHD config:
			* <pre> $ sudo vim /etc/ssh/sshd_config
			* Change PasswordAuthentication back from yes to no.
	* Setuped ufw: 
		* Turned UFW on with the default set of rules:
			* <pre> $ sudo ufw enable
		* Checked the status of UFW:
			* <pre> $ sudo ufw status verbose
		* Allowed incoming TCP packets on port 2200 (SSH):
			* <pre> $ sudo ufw allow 2200/tcp
		* Allowed incoming TCP packets on port 80 (HTTP):
			* <pre> $ sudo ufw allow 80/tcp
		* Allowed incoming UDP packets on port 123 (NTP):
			* <pre> $ sudo ufw allow 123/udp
	* Configured time zone:
		* <pre> $ sudo dpkg-reconfigure tzdata
		* Then chose 'None of the above', then UTC.
	* Installed and configurated Apache:
		* <pre> $ sudo apt-get install apache2
	* Installed mod_wsgi for serving Python apps from Apache and the helper package python-setuptools:
		* <pre> $ sudo apt-get install python-setuptools libapache2-mod-wsgi
	* Restarted the Apache server for mod_wsgi to load:
		* <pre> $ sudo service apache2 restart
	* Createed an empty Apache config file with the hostname:
		* <pre> $ echo "ServerName HOSTNAME" | sudo tee /etc/apache2/conf-available/catalog.conf
	* Enabled the new config file:
		* <pre> $ sudo a2enconf catalog
	* Installed Git:
		* <pre> $ sudo apt-get install git
	* Installed additional packages that enable Apache to serve Flask applications:
		* <pre> $ sudo apt-get install libapache2-mod-wsgi python-dev
	* Enabled mod_wsgi:
		* <pre> $ sudo a2enmod wsgi
	* Created a Flask app:
		* <pre> $ cd /var/www
		* <pre> $ sudo mkdir catalog
		* <pre> $ cd catalog
		* <pre> $ sudo mkdir catalog
		* <pre> $ cd catalog
		* <pre> $ sudo mkdir static templates
	* Created the file that will contain the flask application logic:
		* <pre> $ sudo nano __init__.py
	* Pasted in the following code:
		from flask import Flask  
		app = Flask(__name__)  
		@app.route("/")  
		def hello():  
			return "Veni vidi vici!!"  
		if __name__ == "__main__":  
			app.run()
	* Installed pip installer:
		* <pre> $ sudo apt-get install python-pip
	* Installed virtualenv:
		* <pre> $ sudo pip install virtualenv
	* Setted virtual environment to name 'venv':
		* <pre> $ sudo virtualenv venv
	* Enabled all permissions for the new virtual environment (no sudo should be used within):
		* <pre> $ sudo chmod -R 777 venv
	* Activated the virtual environment:
		* <pre> $ source venv/bin/activate
	* Installed Flask inside the virtual environment:
		* <pre> $ pip install Flask
	* Ran the app:
		* <pre> $ python __init__.py
	* Deactivated the environment:
		* <pre> $ deactivate
	* Configured and Enable a New Virtual Host#
	* Created a virtual host config file
		* <pre> $ sudo nano /etc/apache2/sites-available/catalog.conf
	* Pasted in the following lines of code:
		  <VirtualHost *:80>
		      ServerName PUBLIC-IP-ADDRESS
		      ServerAdmin admin@PUBLIC-IP-ADDRESS
		      WSGIScriptAlias / /var/www/catalog/catalog.wsgi
		      <Directory /var/www/catalog/catalog/>
			  Order allow,deny
			  Allow from all
		      </Directory>
		      Alias /static /var/www/catalog/catalog/static
		      <Directory /var/www/catalog/catalog/static/>
			  Order allow,deny
			  Allow from all
		      </Directory>
		      ErrorLog ${APACHE_LOG_DIR}/error.log
		      LogLevel warn
		      CustomLog ${APACHE_LOG_DIR}/access.log combined
		  </VirtualHost>
	* Enabled the virtual host:
		* <pre> $ sudo a2ensite catalog
	* Created the .wsgi File and Restart Apache
		* <pre> $ cd /var/www/catalog and $ sudo vim catalog.wsgi
	* Pasted in the following lines of code:
		  #!/usr/bin/python
		  import sys
		  import logging
		  logging.basicConfig(stream=sys.stderr)
		  sys.path.insert(0,"/var/www/catalog/")

		  from catalog import app as application
		  application.secret_key = 'Add your secret key'
	* Restart Apache:
		* <pre> $ sudo service apache2 restart
* A list of any third-party resources you made use of to complete this project:
	* www.stackoverflow.com
	* http://askubuntu.com/questions/410244/a-command-to-list-all-users-and-how-to-add-delete-modify-users
	* https://help.ubuntu.com/community/AutomaticSecurityUpdates
	* https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
	* https://wiki.ubuntu.com/UncomplicatedFirewall
	* https://help.github.com/articles/set-up-git/#platform-linux
	* https://discussions.udacity.com/t/oauth-provider-callback-uris/20460
	* http://httpd.apache.org/docs/2.2/en/vhosts/name-based.html
