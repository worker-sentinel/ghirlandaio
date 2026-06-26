# Mengaktifkan dan mengecek status firewall
```
systemctl status firewalld
systemctl start firewalld
systemctl enable firewalld
```
## Cek status firewalld
Pastikan statusnya aktif dan enable
```
systemctl status firewalld
```

# Memeriksa konfigurasi bawaan
```
firewall-cmd --info-zone=drop
firewall-cmd --info-zone=block
firewall-cmd --info-zone=public
firewall-cmd --info-zone=external
firewall-cmd --info-zone=internal
firewall-cmd --info-zone=dmz
firewall-cmd --info-zone=work
firewall-cmd --info-zone=home
firewall-cmd --info-zone=trusted
```
# Mengunci dan menghapus layanan
zona public
```
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
```

zona external
```
firewall-cmd --zone=external --remove-service=ssh --permanent
```

zona internal
```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

zona dmz
```
firewall-cmd --zone=dmz –remove-service=ssh –permanent
```

zona work
```
firewall-cmd --info-zone=work –remove-service={dhcpv6-client,ssh} --permanent
```

zona home
```
firewall-cmd --info-zone=home --remove-service={dhcpv6-client,samba-client,ssh} --permanent
```

zona operator
```
firewall-cmd --zone=operator --remove-service=ssh --permanent
```

zona nm-shared
```
firewall-cmd --zone=nm-shared --remove-service={dhcp,dns,ssh} --permanent
```

zona admin
```
firewall-cmd --zone=admin --remove-service=ssh --permanent
```

```
firewall-cmd --reload
```
## Melihat List Zona
```
firewall-cmd --list-all-zone
```

```
exit
```
> [!NOTE]
> Semua services pada bagian zone dihapus. Pengecualian untuk zona public, sisakan ssh.Gunakan {} apabila ada lebih dari satu dibagian services untuk tiap zona.
