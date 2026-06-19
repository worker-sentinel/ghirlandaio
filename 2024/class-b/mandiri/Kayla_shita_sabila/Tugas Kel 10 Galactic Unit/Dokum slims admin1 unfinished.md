
# Instalasi Podman Compose dan Persiapan SLiMS 1

## 1. Instalasi Podman Compose

Install paket `podman-compose`:

```bash
sudo pacman -S podman-compose
```

---

## 2. Membuat Direktori Konfigurasi

Buat direktori konfigurasi dan masuk ke dalamnya:

```bash
mkdir -p ~/.config/galactic
cd ~/.config/galactic
```

---

## 3. Mengunduh Source Docker Compose SLiMS

Unduh source Docker Compose untuk SLiMS dari GitHub:

```bash
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

---

## 4. Instalasi Wget dan Unzip (Jika Belum Terpasang)

Install paket yang diperlukan untuk mengunduh dan mengekstrak file:

```bash
sudo pacman -S wget unzip
```

---

## 5. Mengekstrak File dan Menyiapkan Direktori

Ekstrak file ZIP, ubah nama direktori hasil ekstraksi, lalu masuk ke direktori tersebut:

```bash
unzip master.zip
mv docker-compose-for-slims-master compose
cd compose
```

---

## 6. Konfigurasi Port Non-Privileged

Edit file konfigurasi sysctl:

```bash
sudo nvim /etc/sysctl.d/13-custom.conf
```

Tambahkan konfigurasi berikut:

```conf
net.ipv4.ip_unprivileged_port_start=80
```

Terapkan perubahan konfigurasi:

```bash
sudo sysctl --system
```

---

## 7. Mengganti File Docker Compose

Cadangkan file Docker Compose lama dan gunakan konfigurasi Redis:

```bash
mv docker-compose.yaml docker-compose.yaml.bck
mv docker-compose-redis.yaml docker-compose.yaml
```

---
