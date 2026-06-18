### network
```
nvim /etc/iwd/main.conf
```
masukan
```
[General]
EnableNetworkConfiguration=true
```
### disable modul
```
nvim /etc/modprobe.d/hardening.conf
```
isi
```
install    cramfs           /bin/false


install    freexfs          /bin/false


install    hfs              /bin/false


install    hfsplus          /bin/false


install    jffs2            /bin/false


install    udf              /bin/false


install    fire-wire-core   /bin/false


install    usb_storage      /bin/false
```

ketik

```
mkinitcpio -P
```

cek lisk module

```
lsmod
```
```
lsmod | grep namamodule
```
### setup firewalld
```
systemctl enable --now firewalld
```
```
sudo firewall-cmd --zone=public --add-service=http --permanent
```
```
sudo firewall-cmd --zone=public --add-port=2377/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=2377/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=4789/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=5432/tcp --permanent
```
```
sudo firewall-cmd --zone=public --add-port=6379/tcp --permanent
```
```
sudo firewall-cmd --reload
```

liat port yang sudah di enable
```
sudo firewall-cmd --reload
```




