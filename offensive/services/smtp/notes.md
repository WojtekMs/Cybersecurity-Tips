# SMTP
It is possible to find out usernames on the server by simply trying to send emails to them.

## Example
- telnet [ip-addr] 25
- RCPT TO:[username] (this provides response: user exists, user doesn't exist)
- VRFY [username] (the same)
- EXPN [username] (the same)

## Metasploit framework
There are ready brute force usernames scanners for smtp ready. It should provide you with a list of valid usernames for the server. Later on you can try to brute-force password for a certain user.
- msfconsole
- search smtp
- find smtp_enum and use it
- set RHOSTS [target-ip-addr]
- set user file (optional)
- run
  
