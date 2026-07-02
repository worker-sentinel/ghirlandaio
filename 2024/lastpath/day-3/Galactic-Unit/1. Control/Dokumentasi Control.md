### Step 1: Install KVM packages 

> Menginstal semua paket yang diperlukan untuk menjalankan KVM:
```
sudo pacman -Syy

sudo reboot 

sudo pacman -S archlinux-keyring

sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 openbsd-netcat
```
> Instal juga packages _ebtables_ dan _iptables_:
```
sudo pacman -S ebtables iptables
```


### Step 2: Install libguestfs di Arch Linux

> libguestfs adalah sekumpulan alat yang digunakan untuk mengakses dan mengubah citra disk mesin virtual (VM). 
```
sudo pacman -S libguestfs
```

### Step 3: Start KVM libvirt service

> Setelah instalasi selesai, jalankan dan aktifkan layanan libvirtd agar berjalan saat sistem dinyalakan:
```
sudo systemctl enable libvirtd.service

sudo systemctl start libvirtd.service
```
> Untuk mengecek status apakah sudah aktif atau belum, gunakan:
```
systemctl status libvirtd.service
```


### Step 4: Enable normal user account untuk dapat menggunakan KVM

> Konfigurasikan KVM

> Buka file _/etc/libvirt/libvirtd.conf_ untuk mengedit.
```
sudo pacman -S vim

sudo vim /etc/libvirt/libvirtd.conf
```
> Hapus # pada command _#unix_sock_group = "libvirt"_, seperti berikut:
```
unix_sock_group = "libvirt"
```

 > Hapus # pada command _#unix_sock_rw_perms = "0770"_, seperti berikut:

 ```
 unix_sock_rw_perms = "0770"
 ```

 > Menambahkan user account ke libvirt group.
 ```
 sudo usermod -a -G libvirt galactic

newgrp libvirt
```
> Restart libvirt daemon.
```
sudo systemctl restart libvirtd.service
```


### Step 5: Enable Nested Virtualization 

> memungkinkan VM menjalankan VM di dalamnya. Enable Nested virtualization untuk kvm_intel / kvm_amd dengan mengaktifkan kernel module.
```
### AMD Processor ###

sudo modprobe -r kvm_amd

sudo modprobe kvm_amd nested=1
```
> Jalankan Konfigurasi
```
echo "options kvm-amd nested=1" | sudo tee /etc/modprobe.d/kvm-amd.conf
```
> Pastikan bahwa opsi “Nested Virtualization” telah diatur ke “Ya”:
```
systool -m kvm_amd -v | grep nested
cat /sys/module/kvm_amd/parameters/nested 
```
### Step 6: KVM berhasil di install 
