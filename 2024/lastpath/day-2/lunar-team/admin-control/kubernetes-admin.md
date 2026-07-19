# Dokumentasi Instalasi K3s (Lightweight Kubernetes) di Arch Linux

## 1. Instalasi K3s
```
curl -sfl https://get.k3s.io | sh -
```

Penjelasan:

Command K3s otomatis melakukan
- Deteksi channel stable dan mengunduh binary k3s
- Verifikasi hash binary yang didownload
- Install binary ke /usr/local/bin/k3s
- Membuat symlink kubectl, crictl, ctr ke binary k3s
- Membuat script k3s-killall.sh dan k3s-uninstall.sh
- Membuat environment file & service file systemd (/etc/systemd/system/k3s.service)
- Enable dan start service k3s secara otomatis

Hasil instalasi: K3s v1.36.2+k3s1 terpasang dan service langsung berjalan sebagai control-plane.


## 2. Mengambil Node Token
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
Token ini dipakai untuk mendaftarkan (join) worker node lain ke cluster. Isi token bersifat rahasia — jangan dibagikan ke publik/commit ke repo.


## 3. Cek Konfigurasi Jaringan
```
ip a
```
Digunakan untuk memastikan interface jaringan node:
- wlp3s0 → IP node di LAN (192.168.100.63/24), inilah IP yang dipakai worker untuk konek ke control-plane
- flannel.1 → interface overlay network CNI Flannel (10.42.0.0/32)
- cni0 → bridge CNI untuk pod-pod di node ini (10.42.0.1/24)
- vethxxxx@if2 → virtual ethernet pair tiap pod yang berjalan (masing-masing terhubung ke cni0)

---

## 4.Cek Status Service K3s

```
systemctl status k3s.service
```
Memastikan service k3s.service berstatus active (running). Dari log terlihat komponen bawaan k3s (containerd, traefik sebagai LoadBalancer, dsb) berhasil start dan pod-pod sistem (svclb-traefik, traefik) sudah Running.

## 5.Install Firewalld
Awalnya firewalld belum terpasang:

```
systemctl enable firewalld
```

Failed to enable unit: Unit firewalld.service does not exist


Masuk ke root lalu install:

```
sudo su
pacman -S firewalld
```
> *Catatan troubleshooting:* beberapa mirror pacman sempat gagal (404) saat mengambil paket firewalld dan python-firewall. Ini murni masalah mirror yang belum sinkron, bukan error konfigurasi. Pacman otomatis mencoba mirror lain dalam daftar sampai berhasil download & install.

Paket yang terinstall:
- firewalld — daemon firewall dengan D-Bus interface
- python-firewall — binding Python untuk firewalld
- python-capng — dependency firewalld

## 6.Enable & Start Firewalld
```
systemctl enable firewalld
systemctl start firewalld
```
Firewalld diaktifkan agar otomatis jalan saat boot, lalu langsung distart untuk sesi saat ini.

## 7.Membuka Port Kube-apiserver
```
sudo firewall-cmd --zone=public --remove-port=6433/tcp --permanent
firewall-cmd --zone=public --add-port=6443/tcp --permanent
firewall-cmd --reload
```
Penjelasan flag:
- zone=public → target zona firewall yang aktif
- add-port=6443/tcp → izinkan trafik masuk ke port 6443 protokol TCP
- permanent → perubahan disimpan permanen di config, bukan cuma runtime
- reload → terapkan ulang rules dari config permanen ke runtime
  
## 8.Verifikasi Node Cluster
```
kubectl get node
```
```
NAME        STATUS   ROLES           AGE   VERSION
archlinux   Ready    control-plane   30m   v1.36.2+k3s1
```
Node control-plane berstatus Ready. Perintah ini diulang beberapa kali untuk memastikan node tetap Ready setelah perubahan firewall.

 ## 9. Cek Service k3s-agent (Untuk di laptop Worker)
 ```
systemctl status k3s-agent.service
```
Unit k3s-agent.service could not be found.

Wajar — service k3s-agent hanya ada di *node worker* yang diinstall dengan mode agent (K3S_URL + K3S_TOKEN), bukan di node control-plane ini.

## 10. Worker Node Berhasil Join
```
kubectl get node
```
```
NAME        STATUS   ROLES           AGE     VERSION
archlinux   Ready    control-plane   50m     v1.36.2+k3s1
assyifa     Ready    <none>          3m41s   v1.36.2+k3s1
```

Node worker assyifa berhasil bergabung ke cluster menggunakan node-token dari langkah 2, terhubung ke IP control-plane (192.168.100.63) melalui port 6443 yang sudah dibuka di firewall. Role <none> menandakan node ini adalah worker biasa (bukan control-plane).

