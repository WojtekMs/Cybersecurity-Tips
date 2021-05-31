# WAF
- It is aware of user session
- It is aware  of application insides
- It is designed to protect against the OWASP top 10 list (xss; insecure deserialization; code injection)
- credential protection, pbd, ip intelligence (ip tracking) (very sophisticated defense techniques)
- protocol support (http, https, ftp, smtp (check emails, attachments, viruses, rate limit), dns)
- port scanning; anonymous request; command line limits
- excessive logins

## Apache2 Mod Security
It started as an Apache2 module, but now it is almost stand-alone firewall. It inspects the requests that are sent to your server in real-time and then checks the predefined rules to decide if the request is malicious or not. 
- XSS, SQL injection (mod security will block it)  

We will be using OWASP ModSecurity Core Rule Set. 

## Installing
- sudo apt install libapache2-mod-security2
- a2enmod headers
- systemctl restart apache2

## Rules
/usr/share/modsecurity-crs

We can use the OWASP ModeSecurity Core Rules Set by cloning the github repository:
- git clone https://github.com/coreruleset/coreruleset  

Rename the setup configuration file
- mv crs-setup.conf.example crs-setup.conf  

/etc/modsecurity  
Rename the configuration file 
- mv modsecurity.conf-recommended modsecurity.conf

## Active blocking
Modify configuration file  
/etc/modsecurity/modsecurity.conf
- SecRuleEngine On

## Enable Mod Security in Apache
/etc/apache2/apache2.conf
- \<IfModule security2_module\>
  -  Include /usr/share/modsecurity-crs/crs-setup.conf
  -  Include /usr/share/modsecurity-crs/rules/*.conf 
- \<\IfModule\>  

/etc/apache2/sites-enabled/*.conf (all files that are responsible for different servers / websites)  
Inside VirtualHost
- SecRuleEngine On
- \<IfModule security2_module\>
  -  Include /usr/share/modsecurity-crs/crs-setup.conf
  -  Include /usr/share/modsecurity-crs/rules/*.conf 
- \<\IfModule\>

## Restart apache
- systemctl restart apache2
