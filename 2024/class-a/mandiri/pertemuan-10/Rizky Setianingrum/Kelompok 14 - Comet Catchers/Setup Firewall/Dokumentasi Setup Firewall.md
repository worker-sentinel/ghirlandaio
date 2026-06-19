# Dokumentasi Setup Firewall

```
systemctl status firewalld
```

```
firewall-cmd --list-all-zone | grep drop
firewall-cmd --list-all-service 
firewall-cmd --info-zone=drop
firewall-cmd --info-zone=block   
firewall-cmd --info-zone=public 
```

```
firewall-cmd --info-zone=external
firewall-cmd --info-zone=internal   
firewall-cmd --info-zone=trusted 
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload  
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent                                                                                           firewall-cmd --reload
success
```

```
firewall-cmd --zone=dmz --remove-service=ssh --permanent                                                                                                                                  
firewall-cmd --reload                                                                                                                                                                     
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent                                                                                                                 
firewall-cmd --reload                                                                                                                                                                     
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent                                                                                               
firewall-cmd --reload             
```

## Keluar
```
exit
```
