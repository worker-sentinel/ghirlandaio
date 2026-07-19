## Instalasi Podman dan setup Firewall untuk Node Kubernetes

Bagian ini mencakup instalasi Podman sebagai container runtime dan konfigurasi firewalld agar node dapat terhubung ke cluster Kubernetes (GKE) melalui port API server.

```
sudo su
```
Masuk ke shell dengan hak akses root agar seluruh perintah selanjutnya tidak perlu diawali `sudo`.

```
systemctl status firewalld
```
Memeriksa status service firewalld untuk memastikan firewall aktif sebelum melakukan konfigurasi zona.

```
lsblk
```
Menampilkan daftar block device (disk dan partisi) untuk memverifikasi struktur penyimpanan sebelum instalasi.

```
pacman -S podman
```
Menginstal Podman, container runtime rootless yang digunakan sebagai pengganti Docker untuk menjalankan container SLIMS.

```
systemctl status podman
```
Mengecek status awal service Podman (biasanya masih inactive/tidak berjalan karena baru diinstal).

```
systemctl enable podman
```
Mengaktifkan Podman agar otomatis berjalan setiap kali sistem melakukan boot.

```
systemctl start podman
```
Menjalankan service Podman secara langsung tanpa perlu reboot sistem.

```
systemctl status podman
```
Verifikasi ulang bahwa Podman sudah berstatus active (running) setelah di-enable dan di-start.

```
systemctl status firewalld
```
Verifikasi ulang status firewalld sebelum masuk ke tahap konfigurasi zona.

```
firewall-cmd --list-all-zones
```
Menampilkan seluruh zona firewall beserta service dan port yang aktif di masing-masing zona.

```
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
```
Menghapus service dhcpv6-client dari zona public secara permanen karena tidak dibutuhkan pada node ini.

```
firewall-cmd --zone=public --remove-service=http --permanent
```
Menghapus service http dari zona public secara permanen untuk memperkecil attack surface (mengikuti prinsip hardening).

```
firewall-cmd --zone=public --add-port=6443/tcp --permanent
```
Membuka port 6443/tcp secara permanen, yaitu port default Kubernetes API server yang digunakan node untuk berkomunikasi dengan cluster GKE.

```
firewall-cmd --reload
```
Memuat ulang konfigurasi firewalld agar seluruh perubahan permanen di atas diterapkan tanpa memutus koneksi aktif.

```
firewall-cmd --list-all-zones
```
Verifikasi akhir konfigurasi zona untuk memastikan perubahan (penghapusan dhcpv6-client, http, dan penambahan port 6443) sudah tercatat dengan benar.

```
kubectl get nodes
```
Memeriksa daftar node yang terdaftar di cluster Kubernetes untuk memastikan node ini sudah terhubung dan berstatus Ready.

```
exit
```
Keluar dari shell root, kembali ke user biasa.

```
exit
```
Keluar dari sesi shell/terminal.
