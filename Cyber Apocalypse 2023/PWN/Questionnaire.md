Description: 
It's time to learn some things about binaries and basic c. Connect to a remote server and answer some questions to get the flag.


nc 178.62.9.10 31825

- upon connecting a questionnaire is loaded and prompting to answer all the questions correctly regarding the respective file for this challenge

```
file test
checksec --file=test
```

Q: Is this a '32-bit' or '64-bit' ELF?
A: 64-bit

Q: What's the linking of the binary?
A: dynamic

Q: Is the binary 'stripped' or 'not stripped'?
A: not stripped

Q: Which protections are enabled (Canary, NX, PIE, Fortify)?
A: NX

```
subl test.c
```

Q: What is the name of the custom function that gets called inside 'main()'?
A: vuln()

Q: What is the size of the 'buffer'?
A: 0x20 or 32

Q: Which custom function is never called?
A: gg()

```
gdb ./test
p gg (returns memory address of function)
```

Q: What is the name of the standard function that could trigger a Buffer Overflow?
A: fgets()

Q: Insert 30, then 39, then 40 'A's in the program and see the output. After how many bytes does a Segmentation Fault occur?
A: 40

Q: What is the address of 'gg()' in hex?
A: 0x401176

RETRIEVED FLAG
