## Bagian 1 : INSTALASI OS UNTUK SERVER DAN OPERATOR

Keduanya menggunakan tahapan yang sama, yang berbeda hanyalah operator akan menginstall dekstop juga di tahap akhir. 

### 1. Cek Partisi 

```bash
lsblk
```

### 2. Enkripsi partisi dengan LUKS

```bash
cryptsetup luksFormat --sector-size=4096 /dev/[partisi root]
```

Buka partisi dan beri nama

```bash
cryptsetup luksOpen /dev/[partisi root] [nama]
```

### 3. Buat LVM

Buat physical volume 
```bash
pvcreate /dev/mapper/[nama]
```

Buat volume grup lalu verifikasi apakah vg sudah muncul
```bash
vgcreate [namavg] /dev/mapper/[nama]
vgs
```

Buat Logical Volume untuk root, vars, vlog, vaud, vtmp, home, dan podman 

```bash
lvcreate -L 10G fall -n root  
lvcreate -L 8G fall -n vars
lvcreate -L 1G fall -n vlog
lvcreate -L 1G fall -n vtmp
lvcreate -L 1G fall -n vaud
lvcreate -L 5G fall -n home
lvcreate -L 5G fall -n podman 
```

### 4. Lakukan format 

Format BOOT 
```bash
mkfs.vfat -F32 -n BOOT /dev/[partisiboot]
```

Format sisa logical volume
```bash
mkfs.ext4 /dev/fall/root
mkfs.ext4 /dev/fall/vars
mkfs.ext4 /dev/fall/vtmp
mkfs.ext4 /dev/fall/vaud
mkfs.ext4 /dev/fall/home
mkfs.ext4 /dev/fall/podman
```

### 5. Lakukan mounting

Cek terlebih dahulu dengan `lsblk` sebelum mounting.

1. Mount root terlebih dahulu
```bash
mount /dev/fall/root /mnt
```

2. Mount boot
```bash
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6
```

3. Mount sisa logical volume
```bash 
mount --mkdir -o rw,nodev,nosuid,relatime /dev/fall/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/fall/vtmp /mnt/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/fall/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/fall/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/fall/home /mnt/home
mount --mkdir -o rw,nosuid,noexec,relatime /dev/fall/podman /mnt/var/lib/containers
```
Setelah itu cek dengan `lsblk`. 

### 6. Install package dasar yang dibutuhkan 

```bash
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

### 7. Generate fstab dan copy konfigurasi network

```bash
genfstab -U /mnt > /mnt/etc/fstab 
cp /etc/systemd/network/* /mnt/etc/systemd/network   
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```

Cek apakah sudah berhasil dengan command `cat`
```bash
cat /mnt/etc/fstab 
```

### 8. Konfigurasi di dalam arch-chroot

Masuk ke dalam root
```bash 
arch-chroot /mnt                                                                                                                                      
```

Set localtime
```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime     
```

Set jam 
```bash
hwclock --systohc
```

Konfigurasi
```bash
nvim /etc/locale.gen   
```

Cari `en_US` dan hapus tanda tagar di depan keduanya. Setelah itu keluar `esc` + `:wq`

Generate locale
```bash
locale-gen
```

Masukkan locale ke dalam konfigurasi
```bash
locale > /etc/locale.conf
```

Masuk ke dalam /etc/locale.conf
```bash
nvim /etc/locale.conf
```

Ubah sehingga menjadi seperti berikut : 
```bash 
LANG=en_US.UTF-8                                                                                                                                                          LC_CTYPE="C.UTF-8"                                                                                                                                                        LC_NUMERIC="C.UTF-8"                                                                                                                                                      LC_TIME="C.UTF-8"                                                                                                                                                         LC_COLLATE="C.UTF-8"                                                                                                                                                      LC_MONETARY="C.UTF-8"                                                                                                                                                     LC_MESSAGES=                                                                                                                                                              LC_PAPER="C.UTF-8"                                                                                                                                                        LC_NAME="C.UTF-8"                                                                                                                                                         LC_ADDRESS="C.UTF-8"                                                                                                                                                      LC_TELEPHONE="C.UTF-8"                                                                                                                                                    LC_MEASUREMENT="C.UTF-8"                                                                                                                                                  LC_IDENTIFICATION="C.UTF-8"                                                                                                                                               LC_ALL=en_US.UTF-8     
```

Menambahkan User dan password
```bash
useradd -m starfall
passwd starfall
```

Memberi hak akses ke user sehingga user bisa melakukan sudo su
```bash
echo "starfall ALL=(ALL:ALL) ALL" > /etc/sudoers.d/starfall
```

Buat file directory 
```bash
mkdir /etc/cmdline.d   
```

Membuat file dengan `touch`, memakai tanda kurung kurawal karena kita bikin 2 file langsung.
```bash
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

Masukan konfigurasi ke file boot
```bash
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p7)=star root=/dev/fall/root" > /etc/cmdline.d/01-boot.conf
```

Masukkan file ke conf
```bash
echo "rw" > etc/cmdline.d/02-misc.conf
```

Konfigurasi
```bash
nvim /etc/mkinitcpio.conf
```

Cari hooks yang tidak ada tagarnya lalu tambahkan `sd-encrypt lvm2`. Sehingga menjadi: 
```bash
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-encrypt lvm2 sd-vconsole block filesystems fsck) 
```

prepare boot
etc/mkinitcpio.d/linux-lts.preset

Install bootloader

systemd-boot adalah bootloader ringan yang sudah built-in di systemd:
 ```bash
bootctl --path=/boot install
```
Enable services

manajemen jaringan
```bash
systemctl enable systemd-networkd 
```  
manajemen DNS
```bash
systemctl enable systemd-resolved 
```

WiFi daemon
```bash
systemctl enable iwd       
```
         
firewall
```bash
systemctl enable firewalld         
```

Selesai Instalasi OS

```bash
exit
umount -R /mnt
```

umount -R unmount secara rekursif semua partisi yang ter-mount di bawah /mnt. Setelah ini sistem siap direboot.

