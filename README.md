# lern-windows-share-linux

### linux mount share from windows 

## IN PROCESS LOGIN WITH USER ROOT

### Debian

```bash
apt install cifs-utils -y
```

### Centos

```bash
yum install cifs-utils -y
```

## CREATE FOLDER MOUNT POINT

```bash
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
```bash
chown root: /etc/win-credentials
chmod 600 /etc/win-credentials
```

## CREATE USER

```bash
adduser adminbemis
passwd adminbemis 
usermod -a -G wheel adminbemis
su - adminbemis 
```

## SHOW CURRENT UID AND GID SWITCH TO USER `adminbemis`

```bash
id -u 

id -g 
```

## TEST MOUNT 

### MOUNT BASIC 

```bash
mount -t cifs -o credentials=/etc/win-credentials //172.16.1.130/test-share1 /mnt/win_share
```

### MOUNT WITH PERMISSION

```bash
mount -t cifs -o uid=1004,gid=1004,credentials=/etc/win-credentials,iocharset=utf8,vers=3.0,file_mode=0775,dir_mode=0775  //172.16.1.130/test-share1 /mnt/win_share
```

## CHECK AFTER MOUNT

```bash
df -h
```

## MOUNT ON BOOT `/etc/fstab`

```bash
nano /etc/fstab

//172.16.1.130/test-share1  /mnt/win_share  cifs  uid=1004,gid=1004,credentials=/etc/win-credentials,iocharset=utf8,vers=3.0,file_mode=0775,dir_mode=0775,noperm 0 0
```

## CHECK MOUNT AND SHOW PERMISSION

```bash
ls -la /mnt/win_share
```

### and

```bash
df -h /mnt/win_share
```

## UNMOUNT 

```bash
sudo umount -l MOUNT_POINT

sudo umount -l /mnt/win_share
```

## log error

tail -f /var/log/kern.log

```
Apr 28 12:23:21 k8s-server kernel: [117899.620111] CIFS: VFS: \\nas-server.example.com Dialect not supported by server. Consider  specifying vers=1.0 or vers=2.0 on mount for accessing older servers
```

UBUNTU 22.04 change => vers=2.0

CENTOS 7 change => vers=3.0


