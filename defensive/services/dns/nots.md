# DNS
DNS servers can be either
- recursive (forwards queries to authoritative server)
- authoritative (contains list of IP-domain names)  
Most linux servers use bind9 to run DNS service.  
/etc/bind/named.conf

## Authoritative DNS
- master (DNS records are stored here)
- secondary name server (relies on the master records, as the source of any DNS record it serves)  

When a change occurs on the master it must notify all the secondary name servers. Such servers then request zone transfer in order to update their dns records. Secondary name server may also request a full zone transfer when it starts running. 

Disable recursive dns queries on an authoritative DNS  
/etc/bind/named.conf.options
- recursion no;

### Master DNS
- /etc/bind/named.conf.default-zones  

Inside a zone you can define
- if it is a master (type master;)
- from which DNS servers zone transfer is allowed (allow-transfer {[dns-ip];};)

### Secondary DNS
- /etc/bind/named.conf.default-zones

- if it is a secondary DNS (type slave;)
- what master DNS it has (masters {[master-ip];};)
- from which DNS servers zone transfer is allowed (allow-transfer {"none";};)


## DNS Hardening

### Hide your DNS server version
- /etc/bind/named.conf.options
- version "hidden";
  
### Restrict zone transfers
Zone transfer requests are viable but only from slave domain name servers. In case your DNS is a slave you should forbid transfers
- /etc/bind/named.conf.default-zones
- allow-transfer {"none";};

### Restrict respones
This feature is available in bind >= 9.10.  
To check bind version use: named -v
- /etc/bind/named.conf.options
- rate-limit {responses-per-second 10; };

### Restrict queries
If you know that your DNS server is only supposed to get queries from certain networks (for example only LAN) restrict bind:
- /etc/bind/named.conf.options
- allow-query { 192.168.0.0/24; localhost; };

### Restrict interfaces
If your DNS server runs on multiple interfaces (many ip-addresses) you can restrict that it will listen only on certain interface:
- /etc/bind/name.conf.options
- listen-on {[interface];};

### Restrict recursion
Recursion can lead to DNS poisoning attacks / DNS amplification attacks.
- /etc/bind/name.conf.options
- allow-recursion {localhost;};

### Logging
Logging DNS queries.
- etc/bind/name.conf.options
- logging {channel query.log  { file "/var/log/named/query.log"; severity debug 3; print-time yes;} category queries { query.log; }};

## DNS Service
There are different services running DNS, for example:
- service named start
- service bind9 start

## Chrooting DNS
- https://tldp.org/HOWTO/Chroot-BIND-HOWTO-2.html
