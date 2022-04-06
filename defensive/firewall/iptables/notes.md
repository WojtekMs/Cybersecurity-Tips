# Firewall
Firewall is a module of linux kernel. It is called netfilter!  
Iptables is a program used to configure netfilter.

# Usage
The best way to think of how to use iptables is thinking about the ISO/OSI model:
- physical layer
- data link layer [interface]
- network layer [ip address]
- transport layer [protocol TCP/UDP]
- session / presentation / application [service port]

Try to organize your iptables commands in this order, it will be easier to remember.  

- iptables -A [chain] -s [source ip address] -d [destination ip address] -p [protocl tcp udp] --dport [service port] -j [action ACCEPT DROP LOG REJECT]
- iptables -P [chain] [default policy ACCEPT DROP LOG REJECT]
- iptables -A [chain] -m [module] -j [action]

# Examples
- iptables -P INPUT DROP [by default all input movement will be dropped]
- iptables -P OUTPUT DROP [by default all output movement will be dropped]
- iptables -A INPUT -p tcp --dport 22 -j ACCEPT [accepts ssh connections]
- iptables -A OUTPUT -m state --state=ESTABLISHED,RELATED -j ACCEPT [accepts output connections that are response to some network movement - only packets that were established before are sent out]
- iptables -A INPUT -m state --state=ESTABLISHED,RELATED -j ACCEPT [accepts input connections that are response to some other network movement]
- iptables -A INPUT -p ICMP -j ACCEPT [accepts ICMP protocol movement - which is ping movement]
- iptables -A OUTPUT -p udp --dport 53 -j ACCEPT [accepts dns output movement - DNS works on both udp and tcp! so make sure to add another rule!]
- iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT [accepts http output movement]
- iptables -F INPUT [clears all rules defined for the INPUT chain]
- iptables -L [lists all the rules defined in the firewall]
- iptables -L -v [lists all the rules and shows the number of packets the rule serviced]
- iptables -L -v --line-numbers [shows the rules with index of each rule]

# Notes
1. All configuration done on iptables is runtime configuration. This means that after the computer will be rebooted or the iptables program will be reloaded, the configuration will dissapear. In order to save the configuration you should use iptables-save > /etc/sysconfig/iptables

2. All the rules specified in the firewall are in specific order. This order is important! If packet is resolved by a certain rule no more rules are going to be checked (the same pattern as exception matching in Python/C++). You have remember about the order of the rules, it is essential in achieving good results!!!

3. There is also --sport, it is the source port and it does matter in the OUTPUT chain if we use the source port. It can be used to allow output movement that is a response to an already established connection, for example on port 22 (SSH service).
