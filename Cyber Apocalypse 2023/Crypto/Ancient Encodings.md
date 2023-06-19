Description:
Your initialization sequence requires loading various programs to gain the necessary knowledge and skills for your journey. Your first task is to learn the ancient encodings used by the aliens in their communication.

Downloaded 'crypto_ancient_encodings.zip'

unzip crypto_ancient_encodings.zip

- automatically created 'crypto_ancient_encodings' directory
  
- contains 2 files: 'output.txt' and 'source.py'
  
output.txt contains a long hex string
		
   - navigated to https://gchq.github.io/CyberChef/ and converted hex to base64
   
new output:
U0ZSQ2V6RnVYM2t3ZFhKZmFqQjFjbTR6ZVY5NU1IVmZkMmt4YkY5elpUTmZkR2d4TlY5bGJtTXdaREZ1WjNOZlpYWXpjbmwzYUdWeU0zMD0=

```
echo 'U0ZSQ2V6RnVYM2t3ZFhKZmFqQjFjbTR6ZVY5NU1IVmZkMmt4YkY5elpUTmZkR2d4TlY5bGJtTXdaREZ1WjNOZlpYWXpjbmwzYUdWeU0zMD0=' | base64 -d
```

new output: SFRCezFuX3kwdXJfajB1cm4zeV95MHVfd2kxbF9zZTNfdGgxNV9lbmMwZDFuZ3NfZXYzcnl3aGVyM30=

- still base64 encoded and need to decode 1 more time

```
echo 'SFRCezFuX3kwdXJfajB1cm4zeV95MHVfd2kxbF9zZTNfdGgxNV9lbmMwZDFuZ3NfZXYzcnl3aGVyM30=' | base64 -d
```

RETRIEVED FLAG
