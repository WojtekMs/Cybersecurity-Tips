# SMTP
Configuration file: /etc/postfix/main.cf  
This is mail transfer protocol. Lives on port 25. To check your configuration for mistakes do:
- postconf 1>/dev/null

## Hardening
After setting all options remember to restart the service. Additionally watch the log files if there are any mistakes.

### Banner
Set banner to non-informant one
- /etc/postfix/main.cf

### Set message rate limit

### Disable VRFY
VRFY can be used to check if user exists with such username.
- postconf -e disable_vrfy_command=yes

### Disable open relay
Open relay means that server accepts email from all systems and forwards them.  
Set all trusted networks to the variable.
- postconf -e mynetworks="127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128"  
Set for which domains accept mails (mail to gmail.com will be accepted)
- postconf -e relay_domains="gmail.com"

### Configure incoming emails
SMTPD refer to SMTP daemon, which is responsible for incoming email requests.  
Require HELO (spam servers usually do not do this)
- postconf -e smtpd_helo_required=yes  

### SASL authentication
inside /etc/postfix/main.cf  
- smtp_sasl_auth_enable = yes
- smtp_sasl_security_options = noanonymous
- smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd

### Enable authentication
These settings do not allow to send e-mails without logging in first.  
- smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_unauth_destination
- smtpd_sender_restrictions = reject_unknown_sender_domain
