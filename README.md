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
	* created user grader
		* <pre>$ adduser NEWUSER
	* Give new user the permission to sudo
		* Open the sudo configuration:
			<pre>* $ visudo
	* updated list of software and upgraded all local software.
		* <pre> $ sudo apt-get update
		* <pre> $ sudo sudo apt-get upgrade
	* configurated ssh access:
		* Open the config file:
			* <pre> $ vim /etc/ssh/sshd_config
		* And made following changes:
			* Change to Port 2200.
			* Change PermitRootLogin from without-password to no.
			* Temporalily change PasswordAuthentication from no to yes.
			* Append UseDNS no.
			* Append AllowUsers NEWUSER.
		* Restart SSH Service:
			* <pre> $ /etc/init.d/ssh restart
	* Created SSH Keys:
		* Generated a SSH key pair on the local machine:
			* <pre> $ ssh-keygen
		* Installed ssh-copy-id (in my case i had a mac) and copied public key to the server:
			* <pre> $ brew install ssh-copy-id
			* <pre> $ ssh-copy-id username@remote_host -p**_PORTNUMBER_**
		* Login with the new user:
			* <pre> $ ssh -v grader@PUBLIC-IP-ADDRESS -p2200
		* Open SSHD config:
			* <pre> $ sudo vim /etc/ssh/sshd_config
			* Change PasswordAuthentication back from yes to no.
	* Setup ufw: 
		* Turn UFW on with the default set of rules:
			* <pre> $ sudo ufw enable
		* Check the status of UFW:
			* <pre> $ sudo ufw status verbose
		* Allow incoming TCP packets on port 2200 (SSH):
			* <pre> $ sudo ufw allow 2200/tcp
		* Allow incoming TCP packets on port 80 (HTTP):
			* <pre> $ sudo ufw allow 80/tcp
		* Allow incoming UDP packets on port 123 (NTP):
			* <pre> $ sudo ufw allow 123/udp
	* Configure time zone:
		* <pre> $ sudo dpkg-reconfigure tzdata
		* Then chose 'None of the above', then UTC.
	* Installed and configurated Apache:
		* <pre> $ sudo apt-get install apache2
	* Install mod_wsgi for serving Python apps from Apache and the helper package python-setuptools:
		* <pre> $ sudo apt-get install python-setuptools libapache2-mod-wsgi
	* Restart the Apache server for mod_wsgi to load:
		* <pre> $ sudo service apache2 restart
	* Create an empty Apache config file with the hostname:
		* <pre> $ echo "ServerName HOSTNAME" | sudo tee /etc/apache2/conf-available/catalog.conf
	* Enable the new config file:
		* <pre> $ sudo a2enconf catalog
* A list of any third-party resources you made use of to complete this project:
	* www.stackoverflow.com
	* http://askubuntu.com/questions/410244/a-command-to-list-all-users-and-how-to-add-delete-modify-users
	* https://help.ubuntu.com/community/AutomaticSecurityUpdates
	* https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
	* https://wiki.ubuntu.com/UncomplicatedFirewall
	* https://help.github.com/articles/set-up-git/#platform-linux
	* https://discussions.udacity.com/t/oauth-provider-callback-uris/20460
	* http://httpd.apache.org/docs/2.2/en/vhosts/name-based.html
