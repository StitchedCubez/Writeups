Description: 
Pandora's latest mission as part of her reconnaissance training is to infiltrate the Drobots firm that was suspected of engaging in illegal activities. Can you help pandora with this task?

Accessed webpage at http://159.65.81.51:30473
	
  - This is a login page asking for username and password

Attempted SQL injection for admin login
	
  - username: admin' OR 1=1 #
	
  - password: test

Returned error: "invalid credentials" (may be because ' is not the correct character for escaping input)

Tried another attempt at SQL injection for admin login
	
  - username: admin" OR 1=1 #
	
  - password: test

Successfully logged in as admin

RETRIEVED FLAG
