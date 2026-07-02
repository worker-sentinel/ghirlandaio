# DOKUMENTASI INSTALL KVM
  
> menginstal semua paket yang dibutuhkan untuk menjalankan KVM, untuk Sinkronisasi database paket.
```
sudo pacman -Syy

```
Reboot sistem
```
sudo reboot
```
Perbarui keyring Arch Linux

```

sudo pacman -S archlinux-keyring
```
Perbarui keyring Arch Linux
```


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
> status menunjukkan sedang berjalan
```
$ systemctl status libvirtd.service

 ● libvirtd.service - Daemon virtualisasi 

    Dimuat: dimuat (/usr/lib/systemd/system/libvirtd.service; diaktifkan; pengaturan vendor: dinonaktifkan) 

    Aktif: aktif ( berjalan ) sejak Kam 2019-04-18 20:55:34 EAT; 11 jam yang lalu 

      Dokumen: man:libvirtd(8) 

            https://libvirt.org 

  PID Utama: 646 (libvirtd) 

     Tugas: 26 (batas: 32768) 

    Memori: 74.0M 

    CGroup: /system.slice/libvirtd.service 

            ├─ 646 /usr/bin/libvirtd 

            ├─ 754 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leases> 

            ├─ 755 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leases> 

            ├─ 777 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/docker-machines.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir> 

            ├─ 778 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/docker-machines.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir> 

            ├─25924 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/vagrant-libvirt.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir> 

            ├─25925 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/vagrant-libvirt.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvir> 

            ├─25959 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/fed290.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leasesh> 

            └─25960 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/fed290.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leasesh>
```
> aktifkan akun pengguna biasa untuk menggunakan KVM. Buka file /etc/libvirt/libvirtd.conf untuk diedit
```
sudo pacman -S vim 
sudo vim /etc/libvirt/libvirtd.conf
```
> atur kepemilikan grup soket domain UNIX ke libvirt, (sekitar baris 85 )
```
grup_soket_unix = "libvirt"
```
> atur izin soket UNIX untuk soket baca/tulis (sekitar baris 102 )
```
unix_sock_rw_perms = "0770"
```
> tambahkan akun pengguna Anda ke grup libvirt
```
sudo usermod -a -G libvirt $(whoami) 
newgrp libvirt
```
> mulai ulang daemon libvirt
```
sudo systemctl restart libvirtd.service
```
> aktifkan Virtualisasi Bertingkat (Opsional)
> aktifkan virtualisasi bertingkat kvm_intel / kvm_amddengan mengaktifkan modul kernel seperti yang ditunjukkan
```
### Prosesor Intel ###

 sudo modprobe -r kvm_intel 

sudo modprobe kvm_intel nested=1 



### Prosesor AMD ###

sudo modprobe -r kvm_amd 

sudo modprobe kvm_amd nested=1
```
> untuk membuat konfigurasi ini tetap berlaku, jalankan:
```
echo "options kvm-intel nested=1" | sudo tee /etc/modprobe.d/kvm-intel.conf
```
> konfirmasikan bahwa virtualisasi bertingkat diatur ke Yes:
```
### Prosesor Intel ###

 $ systool -m kvm_intel -v | grep nested 

    nested = "Y" 

    nested_early_check = "N" 

$ cat /sys/module/kvm_intel/parameters/nested 

Y 



### Prosesor AMD ###

 $ systool -m kvm_amd -v | grep nested

     nested = "Y" 

    nested_early_check = "N" 

$ cat /sys/module/kvm_amd/parameters/nested 

Y
```
> menggunakan KVM di Arch Linux / Manjaro
