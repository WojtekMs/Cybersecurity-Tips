# Securing FTP
- Always remember to log failed logins and authenticity trials.
- Always make sure that FTP software is up-to-date

# TIPS

## Disable standard FTP protocol
Use one of the alternatives:
- FTPS (ftp over SSl/TLS)
- SFTP - this is really not an FTP protocol, it uses a similar interface but it goes through SSH (ftp over SSH file transfer protocol)
(built into linux and UNIX OS, only one port is necessary: 22 (ssh))

## Use strong encryption ciphers
- disable older encryption ciphers (DES, Blowfish)
- use strong cipher (AES, TDES)
- disable older hash algorithms (MD5, SHA-1)
- use strong hash algorithms (MAC algorithms) (SHA-256, SHA-512)

## Use TLS1.1 or TLS1.2
Do not use SSL or TLS1.0 protocols because they are not secure. Use TLS1.1 or TLS1.2. Additionally use Elliptic-Curve Diffie-Hellman algorithm key exchange!

## Change default root user
Do not use root as your admin user. You can use some other user that has elevated privileges!

## Look out for a default banner
SFTP servers: Do not display the version of your software when someone is trying to log in

## Require re-authentication after 15min
If a session is inactive for longer than 15minutes require re-authentication

## Logging
Potential alerts:
- user login fails
- upload/download fails
- when IP addr gets blacklisted because of an attack

# STEPS
- sudo apt-get install vsftpd
- /etc/vsftpd.conf (configuration file for vsftpd)
