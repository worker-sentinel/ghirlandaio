# kita cek ip terlebih dahulu
```
ip a
```
# kita masuk kedalam directory network
```
sudo nvim /etc/systemd/network/20-ethernet.network
```
lalu isi bagian address dan gateway
```
Address= [isi terserah kalian/24]
Gateway= [samakan dengab ip address diatas, bagian belakang di ganti dengan angka "1"
```

<img width="445" height="217" alt="image" src="https://github.com/user-attachments/assets/b8f92a76-3ff2-4f69-b68f-5590de1d9e4d" />

# setelah itu kita restart network-nya
```
sudo systemctl restart systemd-networkd
```

# lalu cek apakah sudah ke ganti apa tidak
```
ip a
```
