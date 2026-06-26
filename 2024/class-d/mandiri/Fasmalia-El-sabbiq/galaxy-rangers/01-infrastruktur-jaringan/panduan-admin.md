# Setup IP Admin

##  Check interface for ethernet
```
nmcli device status
```

## Check Interface
```
ip a
```

## Create admin user connection
```
nmcli connection add type ethernet ifname [interface] con-name "admin-connection" ipv4.method manual ipv4.addresses [ip address for admin]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```
```
nmcli connection add type ethernet ifname enp3s0f3u1 con-name "admin-connection" ipv4.method manual ipv4.addresses 18.81.8.88/24 ipv4.gateway 18.81.8.1 ipv4.dns 8.8.8.8
```

## Automated login script
```
echo "nmcli connection up admin-connection" >> /home/admin/.bash_profile
```
```
echo "nmcli connection up admin-connection" >> /home/galaxy/.bash_profile  
```
## Keluar
```
exit
```


