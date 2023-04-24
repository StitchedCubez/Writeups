ENUMERATION:

Nmap:

![image](https://user-images.githubusercontent.com/126739122/234123911-26a88044-ec3e-49f9-a8b6-298592edbbde.png)

Browsed to http://10.10.121.23
	- shows Apache2 Ubuntu Default webpage

Ran gobuster scan:

![image](https://user-images.githubusercontent.com/126739122/234123921-419f5465-16b1-4d3c-b604-e0b0f6d5f43e.png)

Browsed to /blog directory
	- noticed that the webpage is trying to change/resolve as internal.thm
		○ added 10.10.121.23 to /etc/hosts
Found there is a /wordpress directory on this webserver and this is most likely the route for the initial foothold

Ran another gobuster scan on http://internal.thm/wordpress:

![image](https://user-images.githubusercontent.com/126739122/234123931-584e7adf-fd03-4b61-9cad-841c4f58db28.png)

Browsed to /wp-admin directory and this is the WordPress login page
	- noticed that when browsing to http://internal.thm/wordpress/wp-admin the URL changes to http://internal.thm/blog/wp-login.php
	- seems to be redirecting back to the /blog directory
	
Ran wpscan tool on http://internal.thm/blog to enumerate any users:

![image](https://user-images.githubusercontent.com/126739122/234123939-3c2cb915-3353-4af0-a717-983f17f19817.png)

Identified "admin" user account

Attempting to brute force this login as "admin" using Hydra:
	- tried using both "best1050" and "top10000" password files from seclists

Unable to brute force password using Hydra

INITIAL FOOTHOLD:

Found there is a way to do a brute force password attack through wpscan:

![image](https://user-images.githubusercontent.com/126739122/234123954-8838d1c8-9844-42c3-b907-48863db0f719.png)

Successfully logged into WordPress as "admin"

REF: https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/wordpress#panel-rce
	- opened Theme Editor tool
	- updated contents of "archive.php" theme to instead be php reverse shell

Browsed to http://internal.thm/blog/wp-content/themes/twentyseventeen/archive.php
Spawned a reverse shell

PRIVILEGE ESCALATION:

Now have access to machine as 'www-data' user
transferred over LinPEAS from local machine to reverse shell

Ran LinPEAS:

![image](https://user-images.githubusercontent.com/126739122/234123971-a29b2e3e-c14b-488b-9aa9-369a174107d8.png)

Found a file located in /opt
	- 'wp-save.txt' file contains user creds (aubreanna:bubb13guM!@#123)

Switched user accounts to 'aubreanna' 

FOUND USER FLAG

There is also a 'jenkins.txt' file located in /home/aubreanna
	- says that there is an internal Jenkins service running on 127.0.0.2:8080

Tunneled this service to localhost to access this Jenkins service
	- ssh -L 4444:localhost:8080 aubreanna@10.10.121.23

Browsed to http://localhost:4444
	- standard Jenkins login web page
	- no additional or other user credentials found by LinPEAS that could be used for this login

Ran Hydra to attempt brute-forcing this "admin" login for this Jenkins login:

![image](https://user-images.githubusercontent.com/126739122/234123986-7b333850-7388-4f29-b16e-006c821abb87.png)

Found password for "admin" user account
Successfully logged into Jenkins webpage as "admin"

REF: https://blog.pentesteracademy.com/abusing-jenkins-groovy-script-console-to-get-shell-98b951fa64a6
	- Clicked on "Manage Jenkins" > "Script Console"
	- Uploaded groovy script for reverse shell

Spawned a reverse shell on "JENKINS" docker container

Ran LinPEAS:

![image](https://user-images.githubusercontent.com/126739122/234123996-951364d0-5c8a-4ab8-9626-8e516c2bd7ca.png)

Opened /opt/note.txt file
	- found saved credentials (root:tr0ub13guM!@#123)

Tried switching user accounts to root > error:  'permission denied'
	- realized these creds for "root" are for the target machine and not the docker container

Connected back to 10.10.121.23 via SSH
	- ssh root@10.10.121.23

FOUND ROOT FLAG
