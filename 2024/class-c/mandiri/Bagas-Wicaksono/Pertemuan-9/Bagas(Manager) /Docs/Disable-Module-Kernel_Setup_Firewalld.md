# masuk mode root

```
sudo su
```

masukan password root

# masukan coammand ini untuk konfigurasi

```
nvim /etc/iwd/main.conf 
```

lalu masukan konfigurasi berikut:

```
[General]
EnableNetworkConfiguration=true   
```

setelah itu write dan quit menggunakan :wq

# masukan command ini untuk konfigurasi disable module kernel 

```
nvim /etc/modprobe.d/hardening.conf     
```

lalu masukan konfigurasi berikut:

```
install         cramfs          /bin/false                                                                                                          
install         freexfs         /bin/false                                                                                                          
install         hfs             /bin/false                                                                                                       
install         hfsplus         /bin/false                                                                                                          
install         jffs2           /bin/false                                                                                                          
install         udf             /bin/false                                                                                                          
install         fire-wire-core  /bin/false                                                                                                          
install         usb_storage     /bin/false       

```

lalu write dan quit menggunakan :wq

# tetapkan konfigurasi yang dibuat

```
mkinitcpio -P
```
untuk membuat ulang konfigurasi yang baru dirubah

# cek list module yang sudah dibuat

```
lsmod
```
untuk melihat list module kernel yang ada

# cek apakah firewalld sudah aktif dengan command

```
systemctl status firewalld
```
untuk melihat apakah firewalld sudah berfungsi atau belum, kalau belum bisa enable dan start menggunakan command

# enable firewalld

```
systemctl enable firewalld
```

# mulai firewalld

```
systemctl start firewalld
```
lalu masukan konfigurasi:

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent 

firewall-cmd --zone=public --add-port=3306/tcp --permanent 

firewall-cmd --zone=public --add-port=2377/tcp --permanent

firewall-cmd --zone=public --add-port=7946/tcp --permanent  

firewall-cmd --zone=public --add-port=7946/udp --permanent    

irewall-cmd --zone=public --add-port=4789/udp --permanent   
```

# reload firewall untuk memuat konfigurasi yang baru

```
firewall-cmd --reload  
```

# cek list firewall
```
firewall-cmd --list-all  
```

untuk cek port yang sudah di konfigurasi
