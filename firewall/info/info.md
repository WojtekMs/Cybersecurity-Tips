# Firewall

## Next generation firewall
Is able to inspect the network traffic at the protocol level. For example if the attacker uses SSH on a different port, such firewall can block all the SSH movement even if the attacker uses non-standard port. Whereas stateful inspection firewall blocks only certain port numbers!

## Host firewalls
Iptables is a host firewall! Windows firewall can also block applications; protocols;

## IDS/IPS
- IDS - intrusion detection system (close to anti-virus)
- IPS - intrusion prevention system (additionally can block that traffic using firewall)
  
They can be combined with firewall! When you find out something you can then block it.
- Snort 
  - (was designed as a packet sniffer but now it growed more)
  - you can setup rules about someone connecting to your application, important service

NAT - proxing information (port forwarding - our server to be accessible from the router we would have to set it up)

For example we can install Snort as a package to network firewall (for example pfSense).  Snort is a powerful tool!

