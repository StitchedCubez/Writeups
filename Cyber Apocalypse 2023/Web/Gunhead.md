Description: 
During Pandora's training, the Gunhead AI combat robot had been tampered with and was now malfunctioning, causing it to become uncontrollable. With the situation escalating rapidly, Pandora used her hacking skills to infiltrate the managing system of Gunhead and urgently needs to take it down.

Accessed webpage at http://165.232.108.240:31187
	
  - Shows diagnostics of Gunhead robot
	
  - There is an option to open an interactive terminal (only has 3 useable commands)

/storage command returns disk usage of system
/help command shows list of commands
/clear clears out terminal
/ping sends ping to specified IP

Viewed contents of ReconModel.php (inside of Models folder from provided .zip file for challenge)
	
  - shows source code
	
  - ping command is executed on remote system with 'shell_exec()' function

Turned on BurpSuite proxy
Sent /ping command into terminal on webpage 
Sent captured request to 'Repeater' in BurpSuite

![image](https://github.com/StitchedCubez/Writeups/assets/126739122/7ca36a55-ee46-4d33-8d54-d0731c187202)

changed "IP" of ping for command injection
**need to include ; to break out of ping command and execute cat command**

RETRIEVED FLAG
