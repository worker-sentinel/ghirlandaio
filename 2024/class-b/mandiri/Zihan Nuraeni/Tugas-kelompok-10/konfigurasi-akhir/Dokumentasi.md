

## Bagian 5 : Konfigurasi Akhir Firewall

Kami menggunakan command di bawah, sebagai berikut :

```exit
systemctl enable firewalld
system start firewalld
firewall-cmd –list-all-zone
firewall-cmd –info-zone=public
firewall-cmd --zone=public –remove-service=dhcpv6-client –permanent
firewall-cmd –reload
firewall-cmd –info-zone=trusted
firewall-cmd –info-zone=home
firewall-cmd –zone=home –remove-service=dhcpv6-client –permanent
firewall-cmd –reload
firewall-cmd –info-zone=home
firewall-cmd –info-zone=work
firewall-cmd –zone=work –remove-service=dhcpv6-client –permanent
firewall-cmd –reload
firewall-cmd –info-zone=trusted
firewall-cmd –info-zone=internal
firewall-cmd –zone=internal –remove-service=dhcpv6-client –permanent
firewall-cmd –reload
firewall-cmd –info-zone=trusted
firewall-cmd –info-zone=dmz
firewall-cmd –info-zone=external
firewall-cmd –info-zone=block
firewall-cmd –info-zone=drop
firewall-cmd –reload
firewall-cmd –list-all-zone
firewall-cmd –zone=internal –remove-service={mdns,samba-client} –permanent
firewall-cmd –reload
exit
```












