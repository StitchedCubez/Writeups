Description:
As part of your initialization sequence, your team loaded various tools into your system, but you still need to learn how to use them effectively. They have tasked you with the challenge of finding the appropriate tool to open a file containing strange serial signals. Can you rise to the challenge and find the right tool?

Downloaded "hw_timed_transmission" zip file

- Included "Captured_Signals.sal" file

Attempted to google "how to open .sal files"

- Says this file type could be opened in MySQL or SSMS
  
- Downloaded MySQL Workbench from https://dev.mysql.com/downloads/installer/
  
- Tried opening "Captured_Signals.sal" file in MySQL Workbench > error: "unsupported file type"
  
- Also downloaded SSMS from https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16
  
- Tried opening "Captured_Signals.sal" file again > error: "unsupported file type"

Transferred over "Captured_Signals.sal" file over to Kali Linux VM


```
exiftool -a Captured_Signals.sal
```
- Found 6 files stored within this .sal file (digital0-4.bin & meta.json)

- Tried running .bin files in Linux terminal > zsh: exec format error

- Tried to cat each .bin file > nothing helpful there

- Opened meta.json file in FireFox > found additional information about .bin files

- Found "CaptureDevice" = "Logic 8"

- Googled Logic 8 > this is a type of Channel Logic Analyzer used to record and display signals

- Searched for Logic 8 application download > installed Saleae Logic 2 from https://www.saleae.com/downloads/

Opened Logic 2.4.7 > successfully opened Captured_Signals.sal file

- Shows 5 channels each with their own captured signals
	
- Did not immediately find flag once this file was loaded into Logic
	
- Spent a few hours tinkering with the "Analyzers" options in Logic to try finding the flag in the captured data

- Kept on zooming in and out on the signals > eventually the signals formed into the flag

RETRIEVED FLAG
