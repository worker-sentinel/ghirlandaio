# Instalasi Nginx

## Masuk sebagai Root

```
sudo su
```

## Mengaktifkan User Namespace

Edit file:

```
nvim /etc/sysctl.d/99-custom.conf
```

Isi:

```
kernel.unprivileged_userns_clone=1
```

## Instalasi Nginx

```
pacman -S nginx
```

## Konfigurasi systemd-networkd

Edit file konfigurasi:

```
nvim /etc/systemd/network/20-ethernet.network
```

Cek file:

```
ls -l /etc/systemd/network/20-ethernet.network
```

```
cat /etc/systemd/network/20-ethernet.network
```

## Mengelola Layanan

Cek status:

```
systemctl status systemd-networkd
```

Aktifkan saat boot:

```
systemctl enable systemd-networkd
```

Jalankan layanan:

```
systemctl start systemd-networkd
```

Pastikan layanan berjalan:

```
systemctl status systemd-networkd
```

## Melihat Contoh Konfigurasi

Daftar file:

```
ls /usr/lib/systemd/network/
```

Lihat contoh konfigurasi:

```
cat /usr/lib/systemd/network/89-ethernet.network.example
```
