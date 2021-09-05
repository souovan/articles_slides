# Install a ubuntu raspberry distro on raspberry

I usually write the image with the image application.

# To login on your ubuntu-raspberry use:

```
user: ubuntu
password: ubuntu
```

# Enable network configurations

To enable network connections it's necessary to edit the file:

```
sudo vi /etc/netplan/50-cloud-init.yaml
```

and add the following to the end of the file:

```
wifis:
    wlan0:
        dhcp4: true
        optional: true
        access-points:
            "Your_WiFi_SSID_name":
                password: "Your_WiFi_password"
```

at  the end your file may look like this:

```
...
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
    wifis:
        wlan0:
            dhcp4: true
            optional: true
            access-points:
                "Your_WiFi_SSID_name":
                    password: "Your_WiFi_password"
```
