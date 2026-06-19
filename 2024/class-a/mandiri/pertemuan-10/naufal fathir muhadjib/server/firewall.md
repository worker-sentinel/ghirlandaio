## disable module

## konek ke jaringan
````
nvim/etc/main.conf
lalu isi:
[General]
EnableNetworkConfiguration=true
````
````
iwctl
station wlan0 connect
station wlan0 scan
station wlan0 get-networks
lalu exit
````

## disable module kernel
````
nvim /etc/modprobe.d/hardening.conf
lalu isi:
install         cramfs          /bin/false
install         freexfs         /bin/false
install         hfs             /bin/false
install         hfsplus         /bin/false
install         jffs2           /bin/false
install         udf             /bin/false
install         fure-wire-core  /bin/false
install         usb_storage     /bin/false
````
````
mkinitcpio -P
````
````
lsmod | grep cramfs
lsmod | grep freexfs
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep jffs2
lsmod | grep udf
lsmod | grep fure-wire-core
lsmod | grep usb_storage
systemctl enable --now firewalld
````

## setup firewall
````
sudo firewall-cmd --zone=public --add-service=http --permanent
sudo firewall-cmd --zone=public --add-port=2377/tcp --permanent
sudo firewall-cmd --zone=public --add-port=7946/tcp --permanent
sudo firewall-cmd --zone=public --add-port=4789/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=5432/tcp --permanent
sudo firewall-cmd --zone=public --add-port=6379/tcp --permanent
sudo firewall-cmd --reload
exit
````
