# Deployment & Binding Kubernetes

## Connect Wifi
sebelum menghubungkan internet pastikan terlebih dahulu status network kalian menggunakan command
```
systemctl status [aplikasi network]
```
command `systemctl` berfungsi untuk mengelola suatu system
command `status` berfungsi untuk memberi perintah kepada system

Jika status network kalian disable, maka kalian harus enable terlebih dahulu menggunakan command
```
systemctl enable [aplikasi network]
systemctl start [aplikasi network]
```
command `systemctl enabel` berfungsi untuk mengaktifkan aplikasi dan memberikan perintah untuk menjalankannya secara otomatis setelah melakukan booting
command `systemctl start` berfungsi untuk menjalankan secara langsung aplikasi yang ingin dijalankan

Kemudian setelah diaktifkan maka silahkan cek kembali, menggunakan command `systemctl status [aplikasi]`
Jika aplikasi sudah dipastikan berjalan, maka silahkan menghubungkan network kalian menggunakan command

```
iwctl
```
masukkan command `iwctl` untuk masuk ke package iwd.
jika kalian menggunakan `NetworkManager` maka gunakan command
```
nmtui
```
karna disini saya menggunakan package iwd maka saya akan berfokus kepada connect network menggunakan iwd

Kemudian cek driver wifi kalian menggunakan command
```
device list
```

Lalu setelah kalian mengetahui device wifi kalian silahkan masukkan command
```
station [device wifi] get-networks
```
untuk mendapatkan wifi yang tersedia 
Setelah itu masukkan command
```
station [device wifi] connect [nama wifi]
```
{catatan} jika nama wifi kalian lebih dari satu kata maka gunakan tanda petik 2 `("[nama wifi]")` 
kemudian ketik
```
exit
```
untuk keluar dari package iwd, lalu ketik 
```
ping 8.8.8.8
````
untuk menguji apakah komputer kalian apakah sudah terhubung ke internet atau belum, jika sudah maka klik `ctrl + c` pada komputer kalian untuk menstop 
uji ping.

## Samakan waktu

Gunakan command
```
timedatectl
```
Buat cek waktu di komputer kalian apakah sudah jalan atau belum

Kemudian

```
timedatectl set-ntp true
```

Untuk mensinkronkan waktu secara otomatis

```
timedatectl set-timezone Asia/Jakarta
```

Untuk mengubah zona waktu menjadi Asia/Jakarta (WIB)


## Deployment Kubernetes Engine
sebelum kita melakukan deployment, disarankan untuk kalian menginstall package `curl` terlebih dahulu menggunakan command
```
pacman -Q curl
```
command `pacman -Q` berfungsi untuk komputer kalian mencek terlebih dahulu apakah package curl sudah ada di komputer kalian atau belum,
jika sudah maka komputer tidak akan menginstall lagi package tersebut dan menunjukkan versi package `curl` kalian, dan jika belum
komputer akan otomatis akan menginstall package `curl` kalian.

lalu jalankan 
```
curl -sfL https://get.k3s.io | sh -
```
untuk menginstall sekaligus mendeploy kubernetes engine kalian
```
cat /var/lib/rancher/k3s/server/node-token
````
