# Install NFS Server


Ubuntu and Debian:
```
sudo apt-get update

sudo apt-get install -y nfs-kernel-server
```

RHEL and Fedora:
```
dnf update

dnf install -y nfs-kernel-server
```

# Create root NFS Directory

```
sudo mkdir -p /mnt/nfs-share
```

## Set permissions (in this example any user on the client can access and modify content on the shared folder)

```
sudo chown nobody:nogroup /mnt/nfs-share

sudo chmod 777 /mnt/nfs-share
```

# Define access for NFS Clients

Edit the `/etc/exports` file:

| Enable acess to a single client | /mnt/nfs-share {clientIP} (rw,sync,no_subtree_check) |
| --- | --- |
| Enable access to several clients | /mnt/nfs-share {clientIP-1} (rw,sync,no_subtree_check) {clientIP-2} (...) |
| Enable access to an entire subnet | /mnt/nfs-share {subnetIP}/{subnetMask} (rw, sync, no_subtree_check)


# Make NFS Share available to Clients

```
sudo exportfs -a

sudo systemctl restart nfs-kernel-server
```
---

# Setting up NFS on Client and mounting the NFS Share

## Installing NFS Client Packages

Ubuntu and Debian:
```
sudo apt-get update

sudo apt-get install -y nfs-common
```

RHEL and Fedora:
```
dnf install -y nfs-utils
```

>You can check what folders are available at the NFS Server:
>```
>showmount -e {nfsserverIP}
>```


# Mounting the NFS File Share Temporarily ( doesn't remain mounted after client reboot )

1. Create a local directory where the NFS Share will be mounted:

```
sudo mkdir -p /var/my-nfs-share
```

2. Mount the file share, there is no output if the command is successful
```
sudo mount -t nfs {nfsserverIP}:{folder path on server} /var/my-nfs-share

# example sudo mount -t nfs 192.168.100.13:/nfs-share /var/my-nfs-share
```

3. To verify if the NFS Share is mounted successfully, run:

```
mount

# or

df -h
```

---
# Mounting NFS Share Permanentily (will be automatically mounted after client reboot)

1. Create a local directory:

```
sudo mkdir -p /var/my-automaticmount-nfs-share
```

2. Edit the `/etc/fstab` file and insert on the last line (**be careful when editing this file**):
```
{nfsserverIP}:{folder path on server} /var/my-automaticmounut-nfs-share nfs defaults 0 0
```

3. Now test the mount with:
```
sudo mount -a
```
