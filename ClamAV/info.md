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


/usr/local/clamav/script/clamscan_daily
----
TMP_LOG=/var/log/clamav/clam.daily

#! /bin/bash
if [[ ! -e ${TMP_LOG} ]]; then
    mkdir -p /var/log/clamav
    touch /var/log/clamav/file.txt
fi
clamscan -r / --quiet --infected --log=${TMP_LOG}

crontab -e
*/15 * * * * /usr/local/clamav/script/clamscan_hourly

chmod +x /usr/local/clamav/script/clamscan_daily
