# PAM
PAM [Privileged Access Management] is a local authentication system. It is used to verify that user is the one that he claims to be. We verify that by checking:
- password
- biometric data
- cryptograhic token / key  

Pam is a modular service, that means that it contains of many modules which have different tasks. For example there are modules that are responsible for system password policy (pam_cracklib) (the minimum number of characters, small or large characters, digits, special signs etc). Another example is a module that is used to block malicious ip addresses (pam_abl)

## Examples
- sudo apt-get install libpam-abl
- sudo apt-get install libpam-cracklib
- manual: man pam_cracklib
- manual: man pam_abl.conf

## Passsword policy
/etc/pam.d/common-password

## Blocking malicious ip addresses
After installing libpam-abl it should log all the network traffic. You can additionally configure it to block all the malicious traffic
