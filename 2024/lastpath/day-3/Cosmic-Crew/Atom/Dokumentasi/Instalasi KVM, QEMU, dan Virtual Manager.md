# Mengaktifkan Virtualization di BIOS

## Langkah-langkah

Nyalakan laptop atau perangkat, lalu masuk ke mode BIOS.

Pilih menu Security.

Pilih menu Virtualization.

Ubah opsi menjadi Enabled.

Simpan perubahan dan keluar (Save & Exit).

## Verifikasi di Arch Linux

Cek status virtualization pada CPU.

```
lscpu | grep -i virtualization
```

Jika muncul output `VT-x` (Intel) atau `AMD-V` (AMD), virtualization sudah aktif dan siap dipakai untuk KVM/QEMU.

___

# Instalasi KVM, QEMU, dan Virt Manager di Arch Linux

## Update Sistem dan Keyring

Sinkronisasi database paket.

```
sudo pacman -Syy
```

Reboot sistem.

```
sudo reboot
```

Perbarui keyring Arch Linux.

```
sudo pacman -S archlinux-keyring
```

## Instalasi Paket KVM

Instal QEMU, Virt Manager, dan tools jaringan pendukung.

```
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```

Instal ebtables dan iptables untuk firewall jaringan virtual.

```
sudo pacman -S ebtables iptables
```

## Instalasi libguestfs

Instal libguestfs untuk mengelola disk image VM.

```
sudo pacman -S libguestfs
```

## Menjalankan Layanan libvirtd

Aktifkan libvirtd saat boot.

```
sudo systemctl enable libvirtd.service
```

Jalankan libvirtd.

```
sudo systemctl start libvirtd.service
```

Cek status layanan.

```
systemctl status libvirtd.service
```

## Mengizinkan User Biasa Mengelola KVM

Edit file konfigurasi libvirtd.

```
sudo vim /etc/libvirt/libvirtd.conf
```

Set grup soket ke libvirt (baris ~85).

```
unix_sock_group = "libvirt"
```

Set izin akses soket (baris ~102).

```
unix_sock_rw_perms = "0770"
```

Tambahkan user ke grup libvirt.

```
sudo usermod -a -G libvirt $(whoami)
```

Muat ulang grup tanpa logout.

```
newgrp libvirt
```

Restart libvirtd.

```
sudo systemctl restart libvirtd.service
```

## Nested Virtualization (Opsional)

Unload modul kvm_intel (Intel).

```
sudo modprobe -r kvm_intel
```

Muat ulang dengan nested aktif.

```
sudo modprobe kvm_intel nested=1
```

Unload modul kvm_amd (AMD).

```
sudo modprobe -r kvm_amd
```

Muat ulang dengan nested aktif.

```
sudo modprobe kvm_amd nested=1
```

Simpan konfigurasi nested permanen (Intel).

```
echo "options kvm-intel nested=1" | sudo tee /etc/modprobe.d/kvm-intel.conf
```

Cek status nested (Intel).

```
cat /sys/module/kvm_intel/parameters/nested
```

Cek status nested (AMD).

```
cat /sys/module/kvm_amd/parameters/nested
```

Output `Y` berarti nested virtualization sudah aktif.
