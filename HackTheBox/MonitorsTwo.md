ENUMERATION:

Nmap scan of all ports:

![image](https://github.com/StitchedCubez/Writeups/assets/126739122/13602667-3bda-45b1-afb8-7909afa9d8cc)

Nmap aggressive scan of open ports:

![image](https://github.com/StitchedCubez/Writeups/assets/126739122/055ea6b2-ef52-4de7-9bd4-c26c55b95b0b)

Browsed to http://10.10.11.211 and found this webpage:

![image](https://github.com/StitchedCubez/Writeups/assets/126739122/5d90c796-8626-4ec7-87f9-68cce2116558)

  - shows that this webpage is running version 1.2.22 of Cacti

INITIAL FOOTHOLD:

After a bit of additional research on this version of Cacti, found that it is vulnerable to CVE-2022-46169 (unauthenticated RCE)
	- REF: https://github.com/FredBrave/CVE-2022-46169-CACTI-1.2.22
	- copied over this POC python script as "exploit.py" and ran it against this webpage
		○ successfully spawned reverse shell as "www-data" user

Ran LinPEAS on this reverse shell instance
	- did not find any users located in /home
	- also did not find any user.txt or root.txt files
	- found that /bin/bash is SUID and can be used for priv esc to root

Eventually found that this reverse shell is not accessing the actual "MonitorsTwo" machine but rather the backend of the webpage

Also noticed that there are multiple copies on "entrypoint.sh" script located throughout this system
	- currently owned by root and has r+x permissions for everyone

Upgraded reverse shell up to root using SUID bash
	- ran "entrypoint.sh" script
	- states that mysql database has been connected and now running

Copied mysql command from "entrypoint.sh" script and modified its function to list contents of "user_auth" table
	- found 3 entries for "user_auth" table 

  ![image](https://github.com/StitchedCubez/Writeups/assets/126739122/1e83f1f0-d238-4423-850a-e09cf97e4af9)

  - marcus seems to be the most likely user that can access the actual machine via SSH
		○ the password is currently saved as a hash in MySQL and needs to be cracked

Cracked marcus' password using Hashcat

![image](https://github.com/StitchedCubez/Writeups/assets/126739122/b2920345-b5ea-4a47-bc30-4b632fe81ca0)

Successfully logged into SSH using marcus' credentials

PRIVILEGE ESCALATION:

Ran LinPEAS
	- found mail located at /var/mail/marcus

  ![image](https://github.com/StitchedCubez/Writeups/assets/126739122/cc1372f0-018b-4a66-a2c3-031dd9a3233c)

  - after reading the email, it shows that this machine has 3 CVE vulnerabilities to be aware of and eventually fix

REF: https://github.com/UncleJ4ck/CVE-2021-41091
	- downloaded this exploit script for CVE-2021-41091
	- ran exp.sh script

Upgraded to root shell

![image](https://github.com/StitchedCubez/Writeups/assets/126739122/829fe52f-84d8-495f-a1ad-105fe9d4f333)


FOUND BOTH FLAGS
