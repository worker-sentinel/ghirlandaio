### Menghubungkan WiFi 

Masuk ke utilitas WiFi:
```
iwctl
```

Melihat perangkat WiFi:
```
device list 
```

Melihat jaringan yang tersedia:
```
station wlan0 get-networks
```
Menghubungkan WiFi:
```
station wlan0 connect [nama wifi]
```
Koneksikan wifi di 3 regional 

kita periksa terlebih dahulu ip a-nya, fungsinya untuk menampilkan konfigurasi antarmuka jaringan (network interface) beserta IP Address yang berhasil didapatkan oleh laptop setelah terhubung ke WiFi
```
ip a
```
Setelah terkoneksi, pada wifi. selanjutnya melakukan ssh regional internal di laptop control dengan command, 
```
ssh internal@[ipnyainternal]
```
Melakukan Secure Shell (SSH) untuk masuk dan mengendalikan laptop regional internal dari laptop kontrol secara remote (jarak jauh) melalui jalur yang terenkripsi (aman). Setelah berhasil ssh kita buka terminal baru pada laptop control, dengan menajalankan commad berikut
```
Sudo cat /var/lib/rancher/k3s/server/node-token
```
fungsi command tersebut adalah untuk mengambil kode rahasia (token authentication) yang berada di laptop kontrol (K3s Server). Token ini berfungsi sebagai "kunci gerbang" atau bukti verifikasi keamanan agar node baru diizinkan bergabung ke klaster. Setelah menjalankan command tersebut, akan mengeluarkan token. setelah token keluar kita akan menjalankan command
```
curl -sfL https://get.k3s.io | sudo sh -s - agent --server https://[ip internal]:6443 --token [Token]
```
Perintah utama ini digunakan untuk menginstal K3s Agent sekaligus menghubungkannya ke pusat. 

Setelah itu tunggu hingga berhasil.

```
curl -sfL https://get.k3s.io | sudo sh -s - agent --server https://[ip data]:6443 --token [Token]
```
Perintah utama ini digunakan untuk menginstal K3s Agent sekaligus menghubungkannya ke pusat. 
