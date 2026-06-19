# Dokumentasi Connecting Admin

```
sudo nmcli device status  

sudo nmcli device status 

sudo nmcli connection add type ethernet ifname enp3s0f4u2c2
con-name "admin-connection" ipv4.method manual ipv4.addresses 10.10.10.10/24 ipv
4.gateway 10.10.10.1 ipv4.dns 8.8.8.8   
```

```
enter password
```

```
sudo nmcli connection modify admin-connection connection.autoconnect yes 
```

```
enter password
exit
```
