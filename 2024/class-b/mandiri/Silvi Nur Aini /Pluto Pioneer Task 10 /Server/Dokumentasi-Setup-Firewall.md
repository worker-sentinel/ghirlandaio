# Dokumentasi Firewall Admin dan Server
## sambungkan ke jaringan internet
```
nmtui
```
```
ping 1.1.1.1
```
```
sudo su
```
```
systemctl enable firewalld
```
```
systemctl start firewalld
```
```
firewall-cmd --list-all-zone
```
```
firewall-cmd  --info-zone=drop 
```
```
firewall-cmd  --info-zone=block
```
```
firewall-cmd  --info-zone=public
```
```
firewall-cmd  --info-zone=drop
```
```
firewall-cmd  --zone=public –remove-service={dhcpv6-client,ssh}
```
```
firewall-cmd  --reload
```
```
firewall-cmd  --info-zone=public
```
```
firewall-cmd --zone=public --remove-service={dhcpv6-client,ssh} --permanent
```
```
 firewall-cmd  --reload
```
```
firewall-cmd  --info-zone=public
```
```
 firewall-cmd --zone=external --remove-service=ssh --permanent
```
```
firewall-cmd  --reload
```
```
firewall-cmd  --info-zone=external
```
```
firewall-cmd --zone=eksternal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
```
firewall-cmd  --reload
```
```
firewall-cmd  --info-zone=internal
```
```
firewall-cmd  --info-zone=dmz
```
```
 firewall-cmd  --zone=dmz --remove-service=ssh --permanent
```
```
firewall-cmd  --reload
```
```
 firewall-cmd  --info-zone=dmz
```
```
firewall-cmd –zone=work --remove-service={dhcpv6-client,ssh} --permanent
```
```
firewall-cmd  --reload
```
```
firewall-cmd  --info-zone=work
```
```
firewall-cmd  --info-zone=home
```
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
```
firewall-cmd --reload
```
```
firewall-cmd --info-zone=home
```
```
firewall-cmd --info-zone=trusted
```
```
firewall-cmd --info-zone=nm-shared
```
```
firewall-cmd --zone=nm-shared --remove-service={dhcp,dns, ssh} --permanent
```
```
firewall-cmd --reload
```
```
firewall-cmd --info-zone=nm-shared
```
```
firewall-cmd --get-zones
```
```
exit
```
```
exit
```



