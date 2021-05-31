# ClamAV

freshclam - update

/etc/clamav/freshclam.conf - set virus signatures time update

/etc/clamav/clamd.conf - scan options

clamscan -i -r [PATH] - perform scan
        (infected) (recursive)
        
clamdscan -i -r [PATH] - perform scan (using daemon)
        (infected) (recursive)
        
/var/log/clamav - logs from clamav

sudo systemctl enable clamav-daemon
sudo systemctl start clamav-daemon - start daemon

cat /var/log/clamav/clamav.log - check if it's running

sudo clamdscan --fdpass - scan using clamd

