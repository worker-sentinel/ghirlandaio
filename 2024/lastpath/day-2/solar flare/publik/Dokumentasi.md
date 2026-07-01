# Dokument si K3s Publik

---
## 1. Inst=ll dan join K3s .gent
```.
curl-sfl https://get.k3s.io |K3S_URL="https://(ip .dress)" K3S_TOKEN="TOKEN" sh -s -.gent
```
> curl(client URL) merupkn utilitas linux yang berfungsi untuk mengirim data melalui berbagai protokol jaringan kemudian -s ialah silent fungsinya untuk menjalankan proses download tanpa menampilkan progress bar, -f ialah fail apabila server mengembalikan error HTTP, -L ialah location ialah untuk mengikuti rediret apabila URL tujuan dialihkan ke alamat lain.Kemudian https://get.k3s.io merupakan alamat website resmi yang menyediakan script otomatis K3s. K3s_URL="https://ip adress" merupakan environment variable yang memberitahu installer alaat kurbernetes API Server. K3S_TOKEN="TOKEN"ialah token autentifikasi.
```
```
## 2. Jalankan Firewalld
```
systemctl start firewalld
```
> Untuk kelole servie seperti jelankan, hentikan, restart ataupun c
## 3. 
