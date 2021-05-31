# Snort
It is based on rules which you can create on your own.  
Go inside /etc/snort/snort.conf file.  
- set variables (home net or other)
- include your rules (if you added any) (include $RULE_PATH/custom.rules)
(sed -i "s/include \$RULE\_PATH/#include \$RULE\_PATH/" /etc/snort/snort.conf - Comment out all rulesets)

# Rule structure
[Actions] [Protocol] [Source IP ADDR] [Source PortNo] [DirectionOperator] [Destination IP] [Destination Port] [rule options]

# Actions
- alert (generate alert with time & msg)
- log (log packet)
- pass
- activate (alert and run another rule)
- drop (block and log packet)
- reject (block, log, and send info back)
- sdrop (block but dont log it)

# Destination operator
- -> 
- <.>

# Example rules
alert tcp any any -> $HOME_NET 21 (msg: "FTP connection attemp"; sid: 1000001; rev:1;) - alert on any ftp connection attempt

alert tcp any any -> 192.168.1.4 22 (msg: "SSH connection attemp"; sid: 1000001; rev:1;) - alert on specyfic ip ssh connection attempt

# Using
- snort -T -c /etc/snort/snort.conf (test)
- snort -A console -q -c /etc/snort/snort.conf (start & log to console)
- by default logging goes to /var/log/snort
- snort -D -c /etc/snort/snort.conf -l /var/log/snort (start as daemon, use a config file, log to a file)
- 
