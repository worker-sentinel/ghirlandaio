# check interface for ethernet

```
nmcli device status
```

# create admin user connection

```
nmcli connection add type ethernet ifname [interface] con-name "admin-connection" ipv4.method manual ipv4.addresses [ip address for admin]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```


# create operator user connection

```
nmcli connection add type ethernet ifname [interface] con-name "operator-connection" ipv4.method manual ipv4.addresses [ip address for operator]/[CIDR] ipv4.gateway [ip address gateway] ipv4.dns 8.8.8.8
```

# automated login script

```
echo "nmcli connection up admin-connection" >> /home/admin/.bash_profile
```


# automated login script

```
echo "nmcli connection up operator-connection" >> /home/operator/.bash_profile
```


### note
> ip address server dan admin harus dalam satu network contoh:  
> ip server : 192.168.1.12  
> maka  
> ip admin : 192.168.1.13  
> ip operator : 192.168.1.14  
> ip gateway : 192.168.1.1  
> dan notasi CIDR harus sama yakni 24
