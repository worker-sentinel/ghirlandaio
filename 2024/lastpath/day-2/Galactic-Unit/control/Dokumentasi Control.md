### 1. Mengunduh dan Menjalankan Install Script k3s 
```
sudo curl -sfL https://get.k3s.io | sh -
```
> Membuat service k3s.service, menyiapkan API server, datastore, dan generate token join untuk agent.

### 2. Cek status k3s server
```
sudo sytemctl status k3s.service
```
> Memverifikasi apakah service k3s (mode server) aktif dan berjalan tanpa error.

### 3. Cek IP address
```
ip a
```
> Fungsinya di sini untuk mendapatkan IP node ini, yang nantinya dipakai node agent lain sebagai K3S_URL untuk join ke cluster.

### 4. Cek status firewall
```
sudo systemctl status firewalld
```
> Memastikan firewalld aktif sebelum melakukan konfigurasi port.

### 5. Buka port 6443 di firewall
```
sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
```
> Membuka port 6443/tcp (port API server k3s) secara permanen di zone public, supaya node agent dari luar bisa connect ke control plane ini. Tanpa ini, agent akan gagal konek meskipun k3s server sudah jalan.

### 6. Reload firewall
```
sudo firewall-cmd --reload
```
> Menerapkan perubahan rule firewall dari langkah 5 tanpa perlu restart service firewalld.

### 7. Generate Token
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
> Menampilkan isi token yang digenerate otomatis saat instalasi server. Token ini yang dipakai sebagai K3S_TOKEN saat instalasi node agent tanpa token ini, agent tidak bisa autentikasi ke server.
