# Set up IP

Colok kabel LAN ke kedua laptop sever.

```
ip a
```

```
nvim /etc/systemd/network/20-ethernet.network
```

Buat IP Address dan IP Gateway baru di bagian Network (sever 1)

Samakan IP Address dan IP Gateway dengan server 1 namun bedakan bagian oktat terakhir di IP Address di bagian Network (server 2)

Contoh:
```
[Network]                                                                                                                                                                 
Address=15.10.10.4/24                                                                                                                                                     
Gateway=10.10.10.1
```

Cek ulang IP
```
ip a
```
Cek bagian enp0 apakah ip telah sama dengan yang dibuat
