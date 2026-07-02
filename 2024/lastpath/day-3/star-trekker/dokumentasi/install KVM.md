# DOKUMENTASI INSTALL KVM
  
> menginstal semua paket yang dibutuhkan untuk menjalankan KVM
```
sudo pacman -Syy 

sudo reboot 

sudo pacman -S archlinux-keyring 

sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```
> instal juga paket ebtables dan iptables
```
sudo pacman -S ebtables iptables
```
> instal libguestfs di Arch Linux / Manjaro. libguestfs adalah seperangkat alat yang digunakan untuk mengakses dan memodifikasi citra disk mesin virtual (VM).
```
sudo pacman -S libguestfs
```
> mulai layanan KVM libvirt. Setelah instalasi selesai, jalankan dan aktifkan layanan libvirtd agar berjalan saat boot
```
sudo systemctl enable libvirtd.service 

sudo systemctl start libvirtd.service
```
