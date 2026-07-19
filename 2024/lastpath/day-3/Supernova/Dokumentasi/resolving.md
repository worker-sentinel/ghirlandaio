###### install kvm qemu and virt 
```sh
pacman -S libvirt qemu-desktop openbsd-netcat dmidecode 
```
install manager 
```sh
pacman -S virt manager
```
lihat status virt 
```sh
systemctl status libvirt
```

enable libvirt 
```sh
systemctl enable --now libvirt 
```

mengizinkan akses ke user 
```sh
usermod -aG wheel supernova
```
###### installl Qcow ubuntu 
here link ; 
```sh
https://cloud-images.ubuntu.com/releases/noble/release/
```
