# Mengaktifkan dan mengecek status firewall
```
systemctl status firewalld
systemctl start firewalld
systemctl enable firewalld
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
```
firewall-cmd --reload
```
zona external

```
firewall-cmd --zone=external --remove-service=ssh --permanent
```
```
firewall-cmd --reload
```
zona internal

```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
zona dmz

```
firewall-cmd --zone=dmz –remove-service=ssh –permanent
```
```
firewall-cmd --reload
```
zona work dan home

```
firewall-cmd --info-zone=work –remove-service={dhcpv6-client,ssh} --permanent
```
```
firewall-cmd --info-zone=home --remove-service={dhcpv6-client,samba-client,ssh} --permanent
```
```
firewall-cmd --reload
```
Menyimpan perubahan dan keluar

```
firewall-cmd --reload
```
```
exit
```
