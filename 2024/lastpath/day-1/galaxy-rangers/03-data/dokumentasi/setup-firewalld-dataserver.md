 ### setup firewalld
1. check zone. example in below
```
sudo firewall-cmd --list-all-zone
```
2. allow port. example in below

```
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
```
3. allow service. example in below
```
sudo firewall-cmd --zone=public --add-service=ssh --permanent
```
4. remove service. example in below
```
sudo firewall-cmd --zone=internal --remove-service=ssh --permanent
```
>[Note]
> hapus semua service dan port selain di zone public
```
sudo firewall-cmd --reload
```

### setup hardening kernel
#### example for disale module kernel
for check wireless device
```
lspci -knnd ::0280
```
value
```
02:00.0 Network controller [0280]: Intel Corporation Dual Band Wireless-AC 3168NGW [Stone Peak] [8086:24fb] (rev 10)
	Subsystem: Intel Corporation Device [8086:2110]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
```
> iwlwifi is a module
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
value
```
install usb-storage /bin/false
blacklist usb-storage
install iwlwifi /bin/false
blacklist iwlwifi
```
```
sudo modprobe -r usb-storage
```
```
sudo mkinitcpio -P
```
