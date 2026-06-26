# Konfigurasi Firewall
Mengecek status firewall
```bash
systemctl status firewalld
```
```bash
firewall-cmd --info-zone=drop
firewall-cmd --info-zone=block
firewall-cmd --info-zone=public
firewall-cmd --info-zone=external
firewall-cmd --info-zone=internal
firewall-cmd --info-zone=dmz
firewall-cmd --info-zone=work
firewall-cmd --info-zone= home
firewall-cmd --info-zone= trusted
```
Hapus yang tidak digunakan
```bash
firewall-cmd --info-zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload

firewall-cmd --info-zone=external --remove-service=ssh --permanent
firewall-cmd --reload

firewall-cmd --info-zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload

firewall-cmd --info-zone=dmz --remove-service=ssh --permanent
firewall-cmd --reload

firewall-cmd --info-zone=work --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --reload

firewall-cmd --info-zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```
# Disable Module Kernel
```bash
lsmod | grep cramfs
lsmod | grep freevxfs
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep jffs2
lsmod | grep overlayfs
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep usb-storage
lsmod | grep afs
lsmod | grep ceph
lsmod | grep cifs
lsmod | grep exfat 
lsmod | grep ext 
lsmod | grep fat 
lsmod | grep fscache 
lsmod | grep fuse 
lsmod | grep gfs2 
lsmod | grep nfs_common 
lsmod | grep nfsd 
lsmod | grep smbfs_common 
```
Selain fat, jika muncul maka silahkan di disable<br>
Jika modul ditemukan, buat konfigurasi
```bash
nvim /etc/modprobe.d/disable-module.conf
```
Masukkan
```bash
install usb-storage /bin/false
blacklist usb-storage
```
Perbarui initramfs
```bash
mkinitcpio -P
```
