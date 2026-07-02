# HOW TO INSTALL UBUNTU

```
ls
```
>Desktop  Downloads  dow-vm.cast
```
cd Downloads/
```
```
ls
```
>ubuntu-24.04-server-cloudimg-amd64.img
```
mv ubuntu-24.04-server-cloudimg-amd64.img ubuntu.img
```
```
ls
```
>ubuntu.img
```
mv ubuntu.img /var/lib/libvirt/images/
```
```
ls
```
>cd /var/lib/libvirt/images/
```
ls
```
>ubuntu.img
```
sudo pacman -S guestfs-tools
```
```
sudo virt-customize -a ubuntu.img --root-password password: (rahasia)
```
