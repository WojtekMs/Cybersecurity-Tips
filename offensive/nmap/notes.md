# Nmap
This is the main tool when performing penetration tests. Scanning is crucial, so mastering this tool will give you a lot of benefit.
If you want to see the progress of the scanning use arrow up while the nmap is running. 

# Scan set of hosts
- nmap [network-prefix].[first-host-addr]-[last-host-addr]
- nmap 192.168.0.1-255

# Scan set of ports
- nmap -P [first port]-[last port] [network]
- nmap -P 1-65535 192.168.0.0/24


# Scan fast
This option scans only the first 100 ports.
- nmap -F [network]
- nmap -F 192.168.0.0/24

# Scan services & versions
Only the open ports show the version of the service. The closed ports or filtered by firewall do not show versions.
- nmap -sV [ip-addr] (shows services opened on ports together with the services versions)

# Scan full options
This option scans OS, Services & versions, does traceroute and uses scripts. This also prints host public keys for SSH, additionally it will show the supported methods for http server, server header and server title. Additionally it shows the network route to the chosen host.
- nmap -A [ip-addr]

# Some hosts look offline
This option skips host discovery and treats like all hosts in the network are online. In case the machine is blocking pings or does other stealth actions, nmap might not discover the machine at a regular scan. If you pass the this option, the host will be discovered anyway. However this takes more time. 
- nmap -Pn [ip-addr]
- nmap -Pn 192.168.0.0/24

# TCP Scan

## -sT
This option performs 3-way handshake. Thanks to this option you can gather more information about the host you are scanning. However this will certainly expose you as the 'attacker' since you create a full connection with the host.
- nmap -sT [ip-addr]

## -sS
This option just performs the first step in the 3-way handshake. This prevents the target from detecting your scans. However this still can be discovered by the IPS system. This takes more time but afterall gives almost the same information.
- nmap -sS [ip-addr]

## -sU
This option performs scans with UDP protocol. It can give you a different result!
- nmap -sU [ip-addr]


# Stealth scanning

## -sA
This option can be used to bypass some of the firewall rules. Some routers have firewall rules that only allow to receive SYN packets (the beggining of the 3-way-handshake) from hosts in LAN. This way, when we try to send SYN from outer network it will get rejected, but sending ACK packet which indicates a response to already sent SYN packet will get through the firewall.
- nmap -sA [ip-addr]

## --source-port / -g
Some firewalls allow to receive network traffic only from certain source ports. We can specify a source port with which we perform scans. You still have to find out which port is able to be sent through.
- nmap --source-port [port] [ip-addr]
- nmap -g [port] [ip-addr]

## --data-length
Some firewalls block network traffic with certain packets size. Nmap by default sends packets with a fixed size, and some firewalls block it. You can change it, so that nmap sends packets with randomized size.
- nmap --data-length [size] [ip-addr]

## --spoof-mac
You can hide your MAC address when performing nmap scanning. You have to give some fake MAC address to the command. If firewall blocks certan MAC addresses you can disguise your MAC.
- nmap --spoof-mac [new-mac] [ip-addr]
- nmap --spoof-mac 11:22:33:44:55:66 192.168.0.0/24

### check your MAC addr
- macchanger --show [interface]
- macchanger --show eth0

# Nmap scripts
There are ready out-of-the-box scripts available on kali machine.
- /usr/share/nmap/scripts

You can download additional scripts from GitHub repositories. 

## SSH scripts
You can pass the name of the script from the folder with scripts. 
- nmap --script=ssh-brute.nse [ip-addr]

It is possible to change the password & username list with additional CLI options
- nmap -p 22 --script ssh-brute --script-args userdb=[dictionary] passdb=[dictionary] ssh-brute.timeout=4s [target]

You can also change this in the script file itself.

## External scripts
You can use 
- vulscan repository
- nmap-vulners repository

After downloading, go to folder which contains both repositories and use command
- nmap -sV --script vulscan/vulscan.nse,nmap-vulners/vulners.nse [ip-addr-target]

### Vulnerabilities
If the host has vulnerabilities on certain ports, the script will show them. Each vulnerability has a score which shows how easy it is to use them [10.0 very easy, 1.0 very hard].

### Exploiting vulnerabilities  
Next step is trying to exploit a vulnerability, so we should search in the google for the vulnerability & exploits. There are available some source codes that exploit some vulnerabilities, which you can download, compile and use. Of course you would have to change some ip-addresses, port number etc. Additionally you can use metasploit framework.

# Show opened ports
- nmap -sT [IP]

# Show vulnarabilities
- nmap --script vulln [IP]



