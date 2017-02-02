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
	* installed apache2, postgresql, libapache2-mod-wsgi, git, python-setuptools, python-dev, build-essential, python-pip, Flask-SQLAlchemy, sqlite3, libsqlite3-dev, httplib2, google-api-python-client, Flask, itsdangerous, click, Werkzeug, Jinja2, MarkupSafe
* A list of any third-party resources you made use of to complete this project:
	* www.stackoverflow.com
	* http://askubuntu.com/questions/410244/a-command-to-list-all-users-and-how-to-add-delete-modify-users
	* https://help.ubuntu.com/community/AutomaticSecurityUpdates
	* https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
	* https://wiki.ubuntu.com/UncomplicatedFirewall
	* https://help.github.com/articles/set-up-git/#platform-linux
	* https://discussions.udacity.com/t/oauth-provider-callback-uris/20460
	* http://httpd.apache.org/docs/2.2/en/vhosts/name-based.html
