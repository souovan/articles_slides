# Install Samba server

Ubuntu and Debian:
```
sudo apt-get install -y samba
```

## Look if the service is running

```
systemctl status smbd
```

## Create the directory to be shared and change the permissions

```
sudo mkdir -p /mnt/samba-share

sudo chmod 777 /mnt/samba-share
```

## Edit the `/etc/samba/smb.conf` file and add at the bottom of the file:

```
[Samba-Share]
path /mnt/samba-share
read only = no
browsable = yes
```

# Enable samba on firewall and restart service

```
sudo ufw allow samba

service smbd restart
```

> In this example we will use the user `ubuntu` that already exists on the Samba Share server

---

# Mounting Samba share on client linux machine temporarily ( doesn't remain mounted after client reboot )

## Install cifs-utils package

Ubuntu and Debian:
```
sudo apt-get update

sudo apt-get install -y cifs-utils
```

RHEL and Fedora:
```
sudo dnf install -y cifs-utils
```

## Create the local mounting point on client
```
sudo mkdir -p /var/meu-samba-share
```

## Mount the Samba Share
```
sudo mount -t cifs //{sambaserverIP}/{share-name} {local-mounting-point}

# Example sudo mount -t cifs //{192.168.100.13/samba-share /var/meu-samba-share

# Then type the password for the server `ubuntu` that already exists on Samba Share Server
```

---

# Mount the Samba Share permanentily ( will be automatically mounted after client boot )

Edit the `/etc/fstab` file adding it to the bottom:

```
//{sambaserverIP}/{share-name} /{local-mounting-point} cifs username=ubuntu,password=ubuntu 0 0

# Example //192.168.100.13/samba-share /var/meu-samba-share cifs username=ubuntu,password=ubuntu 0 0
```

Now test the mount with:
```
sudo mount -a
```

To be more secure you can change this approach to this:
```
//192.168.100.13/samba-share /var/meu-samba-share cifs credentials=/root/.secret.txt 0 0
```

Where the credentials files contains:
```
cat /root/.secret.txt
username=ubuntu
password=ubuntu
```


