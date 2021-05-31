# Hardening

# Limit roles to single machine
It's best to seperate services to different machines, so that when one is down only one service is dead. If you cannot do that seperate the services to different dockers or at least use chroot.

# Least privilege rule
User accounts & machines should only get the access they need to perform their actions, not any more!

# Disable guest accounts
No other accounts that are definately necessary!

# Restrict non-user accounts from login
When program has an account to perform a function it doesn't have to login! So that even when the attacker gets that account password he won't be able to login to the machine!

# Linux based firewall
When we have a server we have to think about FORWARD!
- iptables -P FORWARD DROP
- iptables -P OUTPUT ACCEPT
- iptables -t nat -P POSTROUTING ACCEPT
- iptables -t nat -P PREROUTING ACCEPT
- iptables -N block (block new packets; new rules)
- iptables -A block -m state --state ESTABLISHED,RELATED -j ACCEPT
- iptables -A block -m state --state NEW -i ! eth0 -j ACCEPT
- iptables -A block -j DROP
- iptables -A INPUT -j block
- iptables -A FORWARD -j block
- iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
- echo 1 > /proc/sys/net/ipv4/ip_forward
- echo 1 > /proc/sys/net/ipv4/tcp_syncookies (helps to avoid syncookies attack)
- echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts (helps to avoid DDOS attack by blocking echo requests)

This blocks everything on the linux server and we have to decide what to let inside.

# Logging
Logs are kept inside /var/log

## Make sure you log privileged commands

## Make sure you log file deletions

## Make sure login & logout events are monitored

# Disable root login
Change root shell to /usr/sbin/nologin. This way only the users in sudo group will have root permissions.

# 
