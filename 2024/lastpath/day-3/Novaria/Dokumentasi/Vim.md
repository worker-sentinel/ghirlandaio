## HOW TO INSTALL VIM

```
sudo pacman -Syy
```
```
sudo reboot 
```
```
sudo pacman -S archlinux-keyring
```
```
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```
```
sudo pacman -S ebtables iptables
```
```
sudo pacman -S libguestfs
```
```
sudo systemctl enable libvirtd.service
```
```
sudo systemctl start libvirtd.service
```
```
Jul 02 19:10:39 laptopfathur systemd[1]: Starting libvirt legacy monolithic daemon...
Jul 02 19:10:40 laptopfathur systemd[1]: Started libvirt legacy monolithic daemon.
Jul 02 19:10:40 laptopfathur libvirtd[10871]: libvirt version: 12.5.0
Jul 02 19:10:40 laptopfathur libvirtd[10871]: hostname: laptopfathur, uid: 0
```
```
Jul 02 19:10:40 laptopfathur libvirtd[10871]: Unable to find 'dnsmasq' binary in $PATH: No such file or directory
Jul 02 19:10:40 laptopfathur libvirtd[10871]: Cannot find 'dmidecode' in path: No such file or directory
Jul 02 19:10:40 laptopfathur libvirtd[10871]: Cannot find 'dmidecode' in path: No such file or directory
Jul 02 19:12:40 laptopfathur systemd[1]: libvirtd.service: Deactivated successfully.
```
## (yang cannot ini perlu di install) (yang di dalam tanda kutip)
