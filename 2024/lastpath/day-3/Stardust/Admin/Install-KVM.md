# Installasi KVM

## Pembaruan Sistem
```
sudo pacman -Syy
```
```
sudo reboot 
```
```
sudo pacman -S archlinux-keyring
```
## Installasi KVM & QEMU
```
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 openbsd-netcat
```
```
sudo pacman -S ebtables iptables
```
```
sudo pacman -S libguestfs
```
## Manajamen Libvirt Service
```
sudo systemctl enable libvirtd.service
```
```
sudo systemctl start libvirtd.service
```
```
systemctl status libvirtd.service
```
## Enable Hak Akses 
```
sudo pacman -S vim
```
```
sudo vim /etc/libvirt/libvirtd.conf
```
hapus hastag 
```
unix_sock_group = "libvirt”
```
hapus hastag
```
unix_sock_rw_perms = "0770”
```
```
sudo usermod -a -G libvirt galactic
```
```
newgrp libvirt
```
```
sudo systemctl restart libvirtd.service
```
## Aktivasi Fitur
```
sudo modprobe -r kvm_amd
```
```
sudo modprobe kvm_amd nested=1
```
```
echo "options kvm-amd nested=1" | sudo tee /etc/modprobe.d/kvm-amd.conf
```
```
systool -m kvm_amd -v | grep nested
cat /sys/module/kvm_amd/parameters/nested 
```
