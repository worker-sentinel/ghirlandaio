# Arch Linux Install Notes
> Dual boot dengan Windows, encrypted LUKS + LVM, desktop XFCE4

---

## 1. Connect WiFi (iwctl)

```bash
iwctl
station wlan0 get-networks
station wlan0 connect <namawifi>
exit
```

---

## 2. Partisi Disk

Disk layout yang dipakai (`/dev/sda`, sudah ada partisi Windows):

| Partisi | Ukuran | Tipe |
|---------|--------|------|
| sda5 | 2G | **EFI Linux (baru)** |
| sda6 | 47.5G | **Linux filesystem (baru)** |


---

## 3. Setup LUKS Encryption

```bash
cryptsetup luksFormat /dev/sda6
cryptsetup luksOpen /dev/sda6 amelia
```

---

## 4. Setup LVM

```bash
pvcreate /dev/mapper/amelia
vgcreate level /dev/mapper/amelia

lvcreate -L 10G level -n root
lvcreate -L 10G level -n vars
lvcreate -L 1G  level -n vlog
lvcreate -L 1G  level -n vaud
lvcreate -L 1G  level -n vtmp
lvcreate -L 10G level -n home
lvcreate -l100%FREE level -n dock   # sisa semua buat Docker
```

---

## 5. Format Filesystem

```bash
mkfs.vfat -F32 -S 4096 -n BOOT /dev/sda5

mkfs.ext4 /dev/level/root
mkfs.ext4 /dev/level/vars
mkfs.ext4 /dev/level/vlog
mkfs.ext4 /dev/level/vaud
mkfs.ext4 /dev/level/vtmp
mkfs.ext4 /dev/level/home
mkfs.ext4 /dev/level/dock
```

---

## 6. Mount

```bash
mount /dev/level/root /mnt

mount --mkdir -o rw,nodev,nosuid,relatime           /dev/level/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/level/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/level/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/level/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/level/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime    /dev/level/dock /mnt/var/lib/docker

mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/sda5 /mnt/boot
```

---

## 7. Pacstrap (Install Base System)

```bash
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware \
  mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep docker
```

> Kalau gagal karena mirror lambat, ulangi perintah yang sama — paket yang sudah ke-download di-skip.

---

## 8. Chroot ke Sistem Baru

```bash
arch-chroot /mnt
```

### 8a. Locale & Timezone

Edit `/etc/locale.gen`, uncomment `en_US.UTF-8`, lalu:

```bash
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

Set timezone:

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

### 8b. Hostname

```bash
echo "amelia" > /etc/hostname
```

### 8c. mkinitcpio

Edit `/etc/mkinitcpio.conf`, pastikan `HOOKS` mengandung `sd-encrypt` dan `lvm2`:

```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)
```

Edit `/etc/mkinitcpio.d/linux-lts.preset`, ubah path UKI ke:

```
default_uki="/boot/EFI/Linux/arch-linux-lts.efi"
```

Lalu build:

```bash
mkinitcpio -P
```

### 8d. Enable Services

```bash
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
systemctl enable firewalld
```

### 8e. Install Desktop (XFCE4)

```bash
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-jack \
  networkmanager network-manager-applet firefox
```

Ganti iwd ke NetworkManager:

```bash
pacman -R iwd
systemctl enable NetworkManager
systemctl enable sddm
```

### 8f. Keluar dari Chroot

```bash
exit
```

---

## 9. Unmount & Reboot

```bash
umount -R /mnt
reboot
```

---

## Setelah Boot

### Install Apache

```bash
sudo pacman -S apache
nvim /etc/httpd/conf/httpd.conf
nvim /etc/httpd/conf/extra/httpd-vhosts.conf
systemctl enable httpd
systemctl start httpd
```

> Kalau Apache gagal start, cek syntax dulu: `apachectl configtest`

---

## Docker Swarm Setup

### Enable Docker

```bash
systemctl enable docker
systemctl start docker
```

### Buka Port Firewall

```bash
firewall-cmd --zone=public --add-port=2377/tcp --permanent
firewall-cmd --zone=public --add-port=7946/tcp --permanent
firewall-cmd --zone=public --add-port=7946/udp --permanent
firewall-cmd --zone=public --add-port=4789/udp --permanent
firewall-cmd --reload
```

### Init Swarm (di node manager)

```bash
docker swarm init --advertise-addr 192.168.1.47
```

### Join Worker ke Swarm

Jalankan perintah `docker swarm join --token ...` yang muncul setelah init di node worker.


### Label Nodes

```bash
docker node update --label-add role=frontend amelia
docker node update --label-add role=backend  lilis
```

### Verifikasi

```bash
docker node ls
docker node inspect amelia --pretty
docker node inspect lilis  --pretty
```

---

## Clone Aplikasi ke Stack

```bash
pacman -S git
mkdir -p /opt/stacks
cd /opt/stacks
git clone https://github.com/opendocman/opendocman.git
```

