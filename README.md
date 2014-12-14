## watch activity

watch -n 1 zfs list -t all 

watch -n 1 iostat -Nhm

## install on (ubuntu) linux

sudo add-apt-repository ppa:zfs-native/stable

sudo apt-get update

sudo apt-get install ubuntu-zfs

## create new zfs drive with mirror

### add to pool
zpool create home -o ashift=12 mirror /dev/sdb /dev/sdc

### enable compression

zfs set compression=lz4 home

### create partition

zfs create tank/fish

## create smb share

zfs get sharesmb

smbpasswd yourUser

zfs set sharesmb=on tank/fish

zfs get -r sharesmb tank

## show state

### get common stats

zfs get all tank/fish | awk '$3 ~ /[GEKMx]$/'

### show partition details

zfs list -t all

### show mountpoint details

zpool list 

### show detailed stats

zpool status

## snapshots

### create snapshot

zfs snapshot tank/fish@backup

### send snapshot

zfs send tank/fish@backup | zfs receive tank2/fish


## destroy

### destroy snapshot

zfs destroy tank/fish@1

### destroy partition

zfs destroy tank/fish

### destroy mount point

zfs destroy tank

## rename

zfs rename tank/fish2 tank/fish

## check ZFS

### start check

zpool scrub tank

### show progress

zpool status

### stop check

zpool scrub -s tank

## auto-snapshot

### turn on

zfs com.sun:auto-snapshot=true tank/fish

### turn off

zfs com.sun:auto-snapshot=false tank/fish

## dedup

### get current state

zfs get dedup

### enable dedup

zfs set dedup=on usb/backup

### disable dedup

zfs set dedup=off usb/backup

## zfs on usb

### mount

zfs import usb/partition

### unmount

zfs export usb/partition
