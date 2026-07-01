# Dokumentasi K3s Publik

---
## 1. Install dan join K3s agent
```.
curl-sfl https://get.k3s.io |K3S_URL="https://(ip .dress)" K3S_TOKEN="TOKEN" (menyesuaikan token) sh -s - agent
```
> curl(client URL) merupkn utilitas linux yang berfungsi untuk mengirim data melalui berbagai protokol jaringan kemudian -s ialah silent fungsinya untuk menjalankan proses download tanpa menampilkan progress bar, -f ialah fail apabila server mengembalikan error HTTP, -L ialah location ialah untuk mengikuti rediret apabila URL tujuan dialihkan ke alamat lain.Kemudian https://get.k3s.io merupakan alamat website resmi yang menyediakan script otomatis K3s. K3s_URL="https://ip adress" merupakan environment variable yang memberitahu installer alaat kurbernetes API Server. K3S_TOKEN="TOKEN"ialah token autentifikasi.
```
```
## 2. Jalankan Firewalld
```
systemctl start firewalld
```
> Untuk kelola servie seperti jalankan, hentikan, restart ataupun mengecek status servive
```
```
## 3. Mengecek status firewalld
```
systemctl status firewalld
```
> memanggil utilitas pengelola service
> apabila keluar active (running) maka firewalld berhasil berjalan
```
```
## 4. Restart K3s Agent 
```
systemctl restart k3s-agent.service
```
> menjalankan worker node pada kubernetes
```
```
## 5. Mengecek status K3s agent
```
systemctl status k3s-agent.service
```
> mengecek apakah node sudah aktif dan terhubung ke cluster kubernetes
```
```
## 6. Keluar dari terminal
```
exit
```
