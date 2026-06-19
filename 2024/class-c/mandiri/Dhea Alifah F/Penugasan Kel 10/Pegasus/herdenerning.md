# Hardening Server Linux

## 1. Login ke Server melalui SSH

```bash
ssh namauser@ipserver
```

Contoh:

```bash
ssh rakha@192.168.2.145
```

## 2. Aktifkan Layanan SSH

```bash
sudo systemctl start sshd
```

Jika SSH tidak dapat diakses karena firewall:

```bash
sudo systemctl stop firewalld
```

## 3. Periksa Konfigurasi Firewall

```bash
sudo firewall-cmd --list-all-zones
```

## 4. Hapus Service yang Tidak Diperlukan

### Zona DMZ

```bash
sudo firewall-cmd --zone=dmz --remove-service=ssh --permanent
```

### Zona External

```bash
sudo firewall-cmd --zone=external --remove-service=ssh --permanent
```

### Zona Home

```bash
sudo firewall-cmd --zone=home --remove-service=dhcpv6-client --permanent
sudo firewall-cmd --zone=home --remove-service=mdns --permanent
sudo firewall-cmd --zone=home --remove-service=samba-client --permanent
sudo firewall-cmd --zone=home --remove-service=ssh --permanent
```

### Zona Internal

```bash
sudo firewall-cmd --zone=internal --remove-service=dhcpv6-client --permanent
sudo firewall-cmd --zone=internal --remove-service=mdns --permanent
sudo firewall-cmd --zone=internal --remove-service=samba-client --permanent
sudo firewall-cmd --zone=internal --remove-service=ssh --permanent
```

### Zona Public

```bash
sudo firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
```

### Zona Work

```bash
sudo firewall-cmd --zone=work --remove-service=dhcpv6-client --permanent
sudo firewall-cmd --zone=work --remove-service=ssh --permanent
```

## 5. Reload Firewall

```bash
sudo firewall-cmd --reload
```

## 6. Verifikasi Modul Kernel

### cramfs

```bash
sudo lsmod | grep cramfs
```

### freevxfs

```bash
sudo lsmod | grep freevxfs
```

### hfsplus

```bash
sudo lsmod | grep hfsplus
```

### jffs2

```bash
sudo lsmod | grep jffs2
```

### overlayfs

```bash
sudo lsmod | grep overlayfs
```

### squashfs

```bash
sudo lsmod | grep squashfs
```

### udf

```bash
sudo lsmod | grep udf
```

### usb-storage

```bash
sudo lsmod | grep usb-storage
```

Jika tidak ada output, berarti modul tidak aktif.

## 7. Nonaktifkan Modul USB Storage

Buat atau edit file konfigurasi:

```bash
sudo nvim /etc/modprobe.d/disable-module.conf
```

Isi file:

```conf
install usb-storage /bin/false
blacklist usb-storage
```

## 8. Regenerasi Initramfs

```bash
sudo mkinitcpio -P
```

## 9. Reboot Sistem

```bash
sudo reboot
```

## 10. Verifikasi Setelah Reboot

```bash
sudo lsmod | grep usb-storage
```

Jika tidak ada output, maka modul USB Storage berhasil dinonaktifkan.
