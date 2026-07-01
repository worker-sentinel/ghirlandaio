# Cek IP addres node 
untuk mengecek ip addres
Ketik;
```
ip a
```
> Sebelum Install K3s kita harus mengetahui dulu, karena itu yang dipakai node buat komunikasi ke node lain di cluster.
# Untuk menyamakan waktu
```
sudo timedatectl set-ntp true
```
> untuk mengaktifkan sinkronisasi waktu otomatis. Wajib buat K3s, karena beda jam antar node bisa bikin TLS/cert  itu error.
# aktifkan sudo
```
sudo curl -sfL https://get.k3s.io | sh -s server
```
> menjalankan command dengan hak akses root/administrator karena command yang dijalankan butuh izin yang tidak dimiliki user sbiasa
# cek sttatus k3s
```
sudo systemctl status k3s.service
```
> untuk memastikan k3s sudah nyala apa belum, cek dulu apakah K3s sudah jalan bener setelah diinstall. Kalau hasilnya active (running)
 = berhasil
# Sudo su
```
sudo su
```
> untuk masuk jadi user root sepenuhnya, jadi command-command berikutnya nggak perlu ketik sudo lagi
# Masuk ke folder data internal k3s server
```
cd /var/lib/rancher/k3s/server/
```
> untuk masuk ke folder data internal K3s server, supaya kamu bisa akses file-file penting cluster
# sudo systemctl status firewalld
```
sudo systemctl status firewalld
```
> cek apakah firewall (firewalld) sedang aktif di server ini
# Started firewalld - dynamic
```
Started firewalld - dynamic firewall daemon
```
> berfungsi mengatur lalu lintas jaringan
# sudo firewall-cmd 
```
sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
```
> untuk membuka port 6443/TCP pada firewall secara permanen, lalu menerapkan perubahan tersebut
# untuk memuat ulang
```
sudo firewall-cmd --reload
```
> untuk memuat ulang (reload) konfigurasi firewalld tanpa menghentikan layanan firewall
# untuk menampilkan daftar node dalam cluster Kubernetes beserta statusnya.
```
sudo kubectl get node
```
>  mengecek apakah node Kubernetes sudah berhasil bergabung ke cluster dan melihat statusnya
# EXIT
