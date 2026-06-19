# Setup Firewalld
## Memeriksa Status Firewalld
```
systemctl status firewalld
```

## Menjalankan dan Mengaktifkan Firewalld
```
systemctl start firewalld
```
```
systemctl enable firewalld
```
```    
systemctl start firewalld
```
```
systemctl status firewalld
```

## Menampilkan Informasi Zona Firewalld
```
firewall -cmd --info-zone=drop
```
```
firewall -cmd --info-zone=block
```
```
firewall -cmd --info-zone=public
```

## Konfigurasi Zona Public
```
firewall-cmd --zone=public --remove-service=dhcpv6-client –permanent
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=public
```

## Konfigurasi Zona External 
```
firewall-cmd --info-zone=external
```
```
firewall-cmd --zone=external --remove-service=ssh –permanent
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=external
```

## Konfigurasi Zona Internal
```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} –permanent
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=internal
```

## Konfigurasi Zona DMZ
```
firewall-cmd --info-zone=dmz
```
```
firewall-cmd --zone=dmz --remove-service=ssh –permanent
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=dmz
```

## Konfigurasi Zona Work
```
firewall-cmd --info-zone=work
```
```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} –permanent
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=work
```

## Konfigurasi Zona Home
```
firewall-cmd --info-zone=home
```
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} –permanent
```
```
firewall-cmd –reload
```
```
firewall-cmd --info-zone=home
```

## Konfigurasi Zona Trusted
```
firewall-cmd --info-zone=trusted
```

## Selesai
```
exit
```
