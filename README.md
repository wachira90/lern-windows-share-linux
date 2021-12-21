# lern-windows-share-linux

### linux mount share from windows 

## IN PROCESS LOGIN WITH USER ROOT

### Debian

```
apt install cifs-utils -y
```

### Centos

```
yum install cifs-utils -y
```

## CREATE FOLDER MOUNT POINT

```
mkdir /mnt/win_share
```

## CREATE CREDENTIAL

```
nano /etc/win-credentials

username=adminbemis
password=****
domain=EXAMPLE
```

## PERMISSION CREDENTIAL
```
chown root: /etc/win-credentials
chmod 600 /etc/win-credentials
```

## CREATE USER
```
adduser adminbemis
passwd adminbemis 
usermod -a -G wheel adminbemis
su - adminbemis 
```

## SHOW CURRENT UID AND GID 
```
id -u 

id -g 
```

## TEST MOUNT 

### MOUNT BASIC 

```
mount -t cifs -o credentials=/etc/win-credentials //172.16.1.130/test-share1 /mnt/win_share
```

### MOUNT WITH PERMISSION

```
mount -t cifs -o uid=1004,gid=1004,credentials=/etc/win-credentials,iocharset=utf8,vers=3.0,file_mode=0775,dir_mode=0775  //172.16.1.130/test-share1 /mnt/win_share
```

## CHECK AFTER MOUNT

```
df -h
```

## MOUNT ON BOOT

```
nano /etc/fstab

//172.16.1.130/test-share1  /mnt/win_share  cifs  uid=1003,gid=1003,credentials=/etc/win-credentials,iocharset=utf8,vers=3.0,file_mode=0775,dir_mode=0775,noperm 0 0

//172.16.1.130/test-share1  /mnt/win_share  cifs  uid=1004,gid=1004,credentials=/etc/win-credentials,iocharset=utf8,vers=3.0,file_mode=0775,dir_mode=0775,noperm 0 0
```

## CHECK MOUNT AND SHOW PERMISSION

```
ls -la /mnt/win_share
```

## UNMOUNT 

```
sudo umount -l MOUNT_POINT

sudo umount -l /mnt/win_share
```
