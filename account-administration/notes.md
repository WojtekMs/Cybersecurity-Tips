# Account administration

## Check what groups user belongs to
- group [user]
- id [user]

## Check what groups are available on the system
- cat /etc/group

## Check what users are available on the system
- cat /etc/passwd

## Check user UID
- id -u username

## Add user
- sudo useradd -m -d [home directory] -G [groups]  -s [shell] [username] 
- sudo useradd -m -d /ftp -G ftp -s /usr/sbin/nologin

## Modify existing user
### Add user to another group
- sudo usermod -aG [group] [user]

### Change home directory
This also moves the content of the original home directory to the new home directory
- sudo usermod -m -d [new home dir] [user]

### Change user login
- sudo usermod -l [new login] [old login]

### Lock user password
- sudo usermod -L/--lock [user]

### Change user login shell
- sudo usermod -s [shell] [user]
- sudo usermod -s /usr/sbin/nologin root

### Unlock user password
- sudo usermod -U [user]


## Delete user
- sudo userdel [user]
