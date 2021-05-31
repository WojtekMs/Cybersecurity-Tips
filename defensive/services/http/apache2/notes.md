# Apache2

## Update your software!
- sudo apt update
- sudo apt-get install apache2

## Use an unprivileged account
Make sure apache2 is using an account without any permissions. 
- cat /etc/passwd
- check if there is a user for www
- you can add a user for www
- sudo useradd -s [shell] -d [absolute-home-path] [username] 
- sudo useradd -s /usr/sbin/nologin -d /var/www/ www-data
- sudo adduser (more friendly interface)

## Directory listing
Make sure that you cannot list any directories you should not list.

## Signature
Do not display server signature on the website (server version, ip-addr, port)

# Configuration
/etc/apache2/apache2.conf

Go down to Directory tags.  
- Directory [directory]
  -  AllowOverride All [we override all options that have been set]
  -  Options -Indexes [do not list directories]
  -  ServerSignature off [no signature]
- Directory  

After doing configuration settings you have to restart the service!  

You can also create a configuration file in a directory used by the web server which is called
- .htaccess (you can define for example Options -Indexes)  

Warning!: .htaccess file should not belong to root, but it should belong to www-data  

## Create users
- cd /etc/apache2
- sudo htpasswd -c /etc/apache2/.htpasswd [user]

## Add authentication options in .htaccess
- inside your http server, go to .htaccess file
- add options
  -  AuthType Basic
  -  AuthName "Administrator access only"
  -  AuthUserFile /etc/apache2/.htpasswd
  -  Require valid-user 
