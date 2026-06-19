## disable kernel
````
nvim /etc/modprobe.d/disable-module.conf
isi dengan:
install usb_storage /bin/false
blacklist usb-storage
install ext /bin/false
blacklist ext
````
