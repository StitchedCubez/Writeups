Description: 
The final stage of your initialization sequence is mastering cutting-edge technology tools that can be life-changing. One of these tools is quipqiup, an automated tool for frequency analysis and breaking substitution ciphers. This is the ultimate challenge, simulating the use of AES encryption to protect a message. Can you break it?

Downloaded crypto_perfect_synchronization.zip file

unzip crypto_perfect_synchronization.zip

```
cat source.py
```

- plaintext message was imported from secret module

- key is randomly generated 16-bytes long

- encrypted using AES-128-ECB and has a 15-byte added salt

- each 16 byte block represents a 1-byte character with the 15-byte added salt

- ciphertext then gets converted to hex and written to output.txt file


Researched more about AES-ECB encryption

- block cipher encryption with set block size
  
- encryption uses same key for each block of plaintext
  
- if 2 blocks include same plaintext, ciphertext will also match

Tried using quipqiup.com to break ciphertext > no luck with this method

Tried converting ciphertext out of hex > not much luck here either

Did more research on methods of breaking/decrypting AES-ECB encryption
	
  - impossible to brute-force all possible keys to decrypt ciphertext
	
  - REF: https://www.youtube.com/watch?v=unn09JYIjOI

```
cat output.txt | xxd -r -p | xxd
```

- lists converted ciphertext into 16 byte blocks (no luck with flag here)
	
- tried inputting each block into quipqiup for flag > did not work 

DID NOT FIND FLAG

====================================================================================================

AFTER PARTY

Reviewed write-up for this challenge

REF: https://github.com/D13David/ctf-writeups/tree/main/cyber_apocalypse23/crypto/perfect_synchronization

Their approach was to assign an alphabetical character/number to each unique 16-byte block of ciphertext

After the ciphertext has been converted to this temp string it is now essentially a substitution cipher

Input this ciphertext into quipqiup.com and found the flag through frequency analysis and substitution cipher decryption

solve.py:

```
with open("output.txt", 'r') as f:
	line = [x.strip() for x in f.readlines()]

alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
seen = {}
sub = 0
temp = ""

for char in lines:
	if char in seen:
		seen[char][0] += 1
	else:
		seen[char] = [1, sub]
		sub += 1

	temp += alphabet[seen[char][1]]

print(temp)
```

FLAG NOW FOUND
