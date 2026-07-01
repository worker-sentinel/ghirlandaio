# Join K3s Agent — kubernet internal
(join node ke cluster K3s)

## 1. Menghubungkan Node Agent ke Cluster K3s
```
curl -sfL https://get.k3s.io | K3S_URL="https://(ip adress)" K3S_TOKEN="menyesuaikan token" sh -s - agent
```
output:
```
[INFO] Using v1.36.2+k3s1 as release

[INFO] Installing k3s to /usr/local/bin/k3s

[INFO] Creating /usr/local/bin/kubectl symlink to k3s

[INFO] Creating /usr/local/bin/crictl symlink to k3s

[INFO] systemd: Creating service file /etc/systemd/system/k3s-agent.service

[INFO] systemd: Enabling k3s-agent unit Created symlink '/etc/systemd/system/multi-user.target.wants/k3s-agent.service'
```
Penjelasan:
> Command ini digunakan untuk memasukkan node baru ke dalam cluster K3s sebagai agent atau worker

> K3S_URL berfungsi untuk mengarahkan node ke server utama (control plane), sedangkan K3S_TOKEN digunakan sebagai kunci supaya node bisa diterima dan bergabung ke cluster

> Bagian sh -s - agent menjalankan proses instalasi K3s sebagai agent. Setelah berhasil, sistem otomatis membuat file K3s, membuat shortcut kubectl dan crictl, serta membuat service k3s-agent agar berjalan otomatis

## 2. Memastikan Service K3s Agent Berjalan (restart)
```
systemctl restart k3s-agent.service
```
cek status service nya:
```
systemctl status k3s-agent.service
```
Output:
```
k3s-agent.service - Lightweight Kubernetes 
   Active: active (running) 
... 
time="2026-07-01T11:06:01Z" level=info msg="k3s agent is up and running" 
Started Lightweight Kubernetes
```
penjelasan:
> Setelah node berhasil masuk ke cluster, service K3s agent direstart untuk memastikan semua konfigurasi udah berjalan dengan baik
> Kalau statusnya sudah menunjukkan active (running), berarti service K3s agent sudah aktif
> Pesan k3s agent is up and running menandakan node internal sudah berhasil terhubung dan resmi menjadi bagian dari cluster sebagai worker/agent

## 3. Menutup Koneksi Server
exit
