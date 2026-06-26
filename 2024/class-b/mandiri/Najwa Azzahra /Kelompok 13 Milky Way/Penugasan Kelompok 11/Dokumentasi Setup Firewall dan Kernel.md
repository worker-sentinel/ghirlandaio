# Dokumentasi Set Up Firewall


## Login sebagai root
```
sudo su
```

## Memastikan Firewalld berjalan
```
systemctl status firewalld
```
```
```
## Konfigurasi zona work
```
firewalld-cmd –info-zone=work
```
```
firewalld-cmd –permanent –zone=work –remove-service={dhcpv6-client,ssh}
``` 
firewalld-cmd –reload
```
```
Firewalld-cmd –info-zone=work
```
```

## Konfigurasi zona dmz
```
firewalld-cmd –info-zone=dmz
```
```
firewalld-cmd –permanent –zone=dmz –remove-service=ssh
```
```
```

## konfigurasi zona external
```
firewalld-cmd –info-zone=external
```
```
firewalld-cmd –permanent –zone=external –remove-service=ssh
```
```
```

## Konfigurasi zona internal
```
firewalld-cmd –info-zone=internal
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=internal 
```
```
firewall-cmd --info-zone=home  
```
```
firewall-cmd --info-zone=trusted
```
```
firewall-cmd --info-zone=drop  
```
```
firewall-cmd --info-zone=block 
```
```
firewall-cmd --reload 
```
```
firewall-cmd --info-zone=public 
```
```
firewall-cmd --permanent --zone=public --remove-service=dhcpv6-client
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=public
```
```
firewall-cmd --info-zone=dmz
```
```
firewall-cmd --info-zone=external  
```
```
firewall-cmd --info-zone=home
```
```
firewall-cmd --info-zone=work  
```
```
firewall-cmd --info-zone=trusted
```
```
firewall-cmd --info-zone=drop
```
```
firewall-cmd --info-zone=block
```
```
```

## Verifikasi kernel
```
lsmod | grep cramfs
```
```
lsmod | grep freefxs
```
```
lsmod | grep hfs
```
```
lsmod | grep hfsplus
```
```
lsmod | grep jffs2
```
```
lsmod | grep squashfs
```
```
lsmod | grep udf
```
```
lsmod | grep firewire-core
```
```
lsmod | grep usb-storage
```
```
lsmod | grep Bluetooth
```
```
```


## Hardening kernel
```
nvim /etc/modprobe.d/hardenings.conf
```
```
Diisi:    

install usb-storage /bin/false
```
```                                                                                                                                                blacklist usb-storage                                                                                                                                                     install bluetooth /bin/false                                                                                                                                              blacklist bluetooth 
exit
```                                                                                                                                                      


     


	



			

