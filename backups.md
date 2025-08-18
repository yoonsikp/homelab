# Backups
## user crontab
```
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
0 3 * * 0 /home/yoonsik/sand_vms_send.sh
0 1 * * 3 /home/yoonsik/rust_backups_send.sh
```

## rust_backups_send.sh
```
#!/usr/bin/env bash
set -e
cd /home/yoonsik/
DATE=$(date +"%y-%m-%d-%s")
LASTBACKUP=$(cat .last_rust_backups)
echo Last Backup on $LASTBACKUP
echo New Backup on $DATE
sudo zfs snapshot rust/backups@$DATE
sudo zfs send -w -i rust/backups@$LASTBACKUP rust/backups@$DATE | pv -L 1536k | ssh root@7399.zfs.rsync.net zfs recv -o readonly=on data1/rust_backups
sudo zfs destroy rust/backups@$LASTBACKUP
echo $DATE > .last_rust_backups

#first run
#sudo zfs send -w rust/backups@$DATE |pv -b -L 1536k| ssh root@7399.zfs.rsync.net zfs recv -o readonly=on data1/rust_backups
```

## sand_vms_send.sh
```
#!/usr/bin/env bash
set -e
cd /home/yoonsik/
DATE=$(date +"%y-%m-%d-%s")
LASTBACKUP=$(cat .last_sand_vms)
echo Last Backup on $LASTBACKUP
echo New Backup on $DATE
sudo zfs snapshot sand/vms@$DATE
sudo zfs send -w -i sand/vms@$LASTBACKUP sand/vms@$DATE | sudo zfs recv -o readonly=on rust/shady_vms
sudo zfs send -w -i sand/vms@$LASTBACKUP sand/vms@$DATE | pv -L 1536k | ssh root@7399.zfs.rsync.net zfs recv -o readonly=on data1/shady_vms
sudo zfs destroy sand/vms@$LASTBACKUP
echo $DATE > .last_sand_vms

#first run
#sudo zfs send -w sand/vms@$DATE | sudo zfs recv rust/shady_vms
#sudo zfs send -w sand/vms@$DATE | ssh root@7399.zfs.rsync.net zfs recv -o readonly=on data1/shady_vms
```

# backups sent to rsync.net every week
