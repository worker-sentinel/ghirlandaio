## 1. Access to Memory (AtoM) 2.10 - Installation on Ubuntu

### Update sistem

```bash
sudo apt update && sudo apt upgrade
```

### Instalasi Nginx

```bash
sudo apt install nginx
```

### Instalasi MariaDB

```bash
sudo apt install mariadb-server
```

### Instalasi PHP dan modul yang dibutuhkan

```bash
sudo apt install php-fpm php-cli php-mysql php-xml php-curl php-mbstring php-zip php-intl php-bcmath
```

### Mengamankan instalasi MariaDB

```bash
sudo mysql_secure_installation
```

### Me-restart layanan Nginx

```bash
sudo systemctl restart nginx
```

### Me-restart PHP-FPM

```bash
sudo systemctl restart php8.x-fpm
```

### Mengaktifkan Nginx saat boot

```bash
sudo systemctl enable nginx
```

### Mengaktifkan MariaDB saat boot

```bash
sudo systemctl enable mariadb
```

### Membersihkan cache AtoM

```bash
php symfony cc
```

---

## 2. Instalasi KVM, QEMU, dan Virt-Manager pada Arch Linux

### Sinkronisasi database paket

```bash
sudo pacman -Syy
```

### Update Arch Linux Keyring

```bash
sudo pacman -S archlinux-keyring
```

### Instalasi QEMU, Virt-Manager, dan paket pendukung

```bash
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```

### Instalasi ebtables dan iptables

```bash
sudo pacman -S ebtables iptables
```

### Instalasi libguestfs

```bash
sudo pacman -S libguestfs
```

### Mengaktifkan libvirtd saat boot

```bash
sudo systemctl enable libvirtd.service
```

### Menjalankan libvirtd

```bash
sudo systemctl start libvirtd.service
```

### Mengecek status libvirtd

```bash
sudo systemctl status libvirtd.service
```

### Me-restart libvirtd

```bash
sudo systemctl restart libvirtd.service
```

### Mengaktifkan Nested Virtualization (Intel)

```bash
sudo modprobe kvm_intel nested=1
```

### Mengaktifkan Nested Virtualization (AMD)

```bash
sudo modprobe kvm_amd nested=1
```

### Memeriksa Nested Virtualization (Intel)

```bash
cat /sys/module/kvm_intel/parameters/nested
```

### Memeriksa Nested Virtualization (AMD)

```bash
cat /sys/module/kvm_amd/parameters/nested
```

---

## 3. Ubuntu Cloud Images (Ubuntu Noble 24.04)

### Menambahkan Ubuntu Cloud Images ke LXD

```bash
lxc remote add --protocol simplestreams ubuntu https://cloud-images.ubuntu.com/releases/
```

### Membuat container Ubuntu Noble

```bash
lxc launch ubuntu:noble
```

### Contoh image Ubuntu Noble

```text
noble-server-cloudimg-amd64.img
```
