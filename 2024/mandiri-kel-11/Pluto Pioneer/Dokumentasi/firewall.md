# Firewall
## Merekam Asciinema
```
asciinema rec nama_file.cast
```
## Connect Jaringan
```
iwctl
```
**Cek jaringan terdekat ada apa aja**
```
station wlan0 get-networks
```
**Masukin nama jaringannya**
```
station wlan0 connect (nama jaringan)
```
```
masukin pass
```
```
exit
```
## Periksa jaringan
```
ping 1.1.1.1
```
## Mengecek status
```
systemctl status firewalld   
```
## firewall
```
firewall-cmd --list-all-zone
```
```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} –permanent
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent  
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} –permanent
firewall -cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --zone=external --remove-service=ssh –permanent
firewall-cmd --zone=dmz --remove-service=ssh –permanent
```
```
firewall-cmd --zone=drop --list-all
firewall-cmd --zone=block --list-all   
```

