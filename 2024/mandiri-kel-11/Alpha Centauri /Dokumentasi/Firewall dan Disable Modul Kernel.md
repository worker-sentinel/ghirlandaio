### Hardening Firewall dan Disable Module Kernel

---

### Masuk ke root

```
sudo su
```

### Verifikasi Status Firewalld
```
systemctl status firewalld
```

### Identifikasi Zona Firewall
```
firewall-cmd --list-all-zone
```

### Menghapus  Layanan yang Tidak Digunakan


### Zona Work
```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
```

### Zona Public

```
firewall-cmd --zone=public --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

### Zona Home

```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

### Zona External

```
firewall-cmd --zone=external --remove-service=ssh --permanent
```

### Zona DMZ

```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
```

### Reload

```
firewall-cmd --reload
```

### Menampilkan Seluruh Zona

```
firewall-cmd --list-all-zone
```

### Mengecek Zona Tertentu

``` 
firewall-cmd --zone=public --list-all
```


### Melihat struktur partisi

```
lsblk
```

### Menonaktifkan Modul Kernel yang Tidak Diperlukan

```
lsmod | grep crampfs
lsmod | grep freevxfs
lsmod | grep jffs2
lsmod | grep hfs
lsmod | grep jfsplus
lsmod | grep hfsplus
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep usb-storage
lsmod | grep usb-bluetooth
```

Edit Konfigurasi Modprobe

```
nvim /etc/modprobe.d/01-custom.conf
```

### Isi Konfigurasinya

```
install bluetooth /bin/false
blacklist bluetooth

install usb-storage /bin/false
blacklist usb-storage
```


### Build Ulang Image Kernel


```
mkinitcpio -P
```

### exit
