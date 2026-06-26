# Dokumentasi Instalasi Arch Linux dengan Disk Layout CIS dan Enkripsi LUKS ON LVM Kelompok 7 Kelas 4-C

---
## CONNECT WIFI
Sambungkan koneksi wifi seperti langkah berikut.
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/Gambar-gambar/WhatsApp%20Image%202026-05-23%20at%2000.35.03%20(2).jpeg)
```
iwctl
```

Cek wifi.
```
device list
```

Sambungkan wifi ke laptop.
```
station wlan0 connect (nama wifi)
```

```
exit
```

Periksa apakah koneksi telah tersambung.
```
ping 8.8.8.8
```

---
## MEMBUAT PARTISI
Lakukan pengecekan dan pembagian partisi dengan langkah-langkah berikut.

Cek partisi.
```
lsblk
```

Pembagian partisi.
```
cfdisk /dev/(partisi)
```

Minimal partisi yang disesuaikan dengan peyimpanan yang ada.
```
boot = 3G (EFI sistem)
root = 70G (linux filesystem)
```

Cek kembali partisi yang telah dibuat.
```
lsblk
```

---
## Partisi LUKS on LVM dengan Disk Layout CIS
> Merupakan partisi yang di LUKS dan di dalamnya terdapat LVM.
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.32(2).jpeg)

Melakukan setup LVM.
```
pvcreate /dev/(partisi)
```
```
vgcreate (nama grup) /dev/(partisi)
```

**Membuat logical volume**
```
lvcreate -L size (G | M) (nama grup) -n root
```
```
lvcreate -L size (G | M) (nama grup) -n vars
```
```
lvcreate -L size (G | M) (nama grup) -n vlog
```
```
lvcreate -L size (G | M) (nama grup) -n vaud
```
```
lvcreate -L size (G | M) (nama grup) -n vtmp
```
```
lvcreate -L size (G | M) (nama grup) -n home
```
```
lvcreate -l50%FREE (nama grup) -n root (user)
```
> -l50%FREE adalah 50% dari sisa ruang yang akan digunakan.

---
## FORMATTING PARTITION
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.31(1).jpeg)
Format partisi BOOT.
```
mkfs.vfat -F32 -n BOOT /dev/(partisi boot)
```

Format partisi grup logical volume.
```
mkfs.ext4 /dev/(nama grup)/root
```
```
mkfs.ext4 /dev/(nama grup)/vars
```
```
mkfs.ext4 /dev/(nama grup)/vlog
```
```
mkfs.ext4 /dev/(nama grup)/vaud
```
```
mkfs.ext4 /dev/(nama grup)/vtmp
```
```
mkfs.ext4 /dev/(nama grup)/home
```

Melakukan setup LUKS
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.30.jpeg)
```
cryptsetup luksFormat /dev/(nama grup)/(nama)
```

---
## MOUNTING
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.29(1).jpeg)
Mounting partisi grup root.
```
mount /dev/(nama grup)/root /mnt
```

Mounting partisi boot.
```
mount --mkdir -o uid=0,gid=0,dmask=0077,fmask=0077 /dev/(partisi boot) /mnt/boot
```

Mounting partisi grup logical volume.
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vlog /mnt/var/vlog
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/(nama grup)/home /mnt/home
```

---
## Instalasi Packages
> Melakukan instalasi package yang disesuaikan dengan prosesor laptop.
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.29(2).jpeg)
**Intel**
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```

**AMD**
```
pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```

**fstab**
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.28.jpeg)
```
genfstab -U /mnt > /mnt/etc/fstab
```

Melakukan format tmpfs ke tmp
```
echo "/tmpfs /tmp  tmpfs  defaults,nosuid,nodev,noexec,size=1G  0  0" >> /mnt/etc/fstab
```

**chroot**
```
arch-chroot /mnt
```

Membuat hostname
```
nvim /etc/hostname
```

---
## Set Localtime dan Locale
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.28(2).jpeg)
Localtime
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

Locale
Uncomenting `en_US`
```
nvim /etc/locale.gen
```

Generate bahasa yang di uncomenting.
```
locale-gen
```
```
locale > /etc/locale.conf
```

Konfigurasi locale dengan mengisi `LANG=C` dan `LANG=ALL` dengan `en_US.UTF-8`
```
nvim /etc/locale.conf
```

---
### LUKS OPEN

```
cryptsetup luksOpen /dev/(nama grup)/(partisi user) (user)
```

Format partisi user
```
mkfs.ext4 /dev/mapper/(user)
```

Cek kembali partisi

```
lsblk
```
---
## HOME USER DIRECTORY

```
mkdir /home/user
```

```
useradd -d /home/user (nama user)
```

```
chown -R (nama user):(nama user) /home/user
```

```
passwd (nama user)
```

```
echo "(nama user) ALL=(ALL:ALL)" > /etc/sudoers.d/(nama user)
```

**Config volume**
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.24(1).jpeg)
```
nvim /etc/security/pam_mount.conf.xml
```
> Samakan dengan code di bawah.
```
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE pam_mount SYSTEM "pam_mount.conf.xml.dtd">
<!--
	See pam_mount.conf(5) for a description.
-->

<pam_mount>

		<!-- debug should come before everything else,
		since this file is still processed in a single pass
		from top-to-bottom -->

<debug enable="0" />

		<!-- Volume definitions -->


		<!-- pam_mount parameters: General tunables -->

<!--
<luserconf name=".pam_mount.conf.xml" />
-->

<!-- Note that commenting out mntoptions will give you the defaults.
     You will need to explicitly initialize it with the empty string
     to reset the defaults to nothing. -->
<mntoptions allow="nosuid,nodev,loop,encryption,fsck,nonempty,allow_root,allow_other" />
<!--
<mntoptions deny="suid,dev" />
<mntoptions allow="*" />
<mntoptions deny="*" />
-->
<mntoptions require="nosuid,nodev" />

<!-- requires ofl from hxtools to be present -->
<logout wait="0" hup="no" term="no" kill="no" />

<!-- Example entry for a LUKS partition -->
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/proc/[name]" 
    mountpoint="/home/user]" 
/>
		<!-- pam_mount parameters: Volume-related -->

<mkmountpoint enable="1" remove="true" />


</pam_mount>
```

> Harus diperhatikan pada seluruh config
```
<!-- Example entry for a LUKS partition -->
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/proc/[user name]" 
    mountpoint="/home/[user name]" 
/>
```

**Update konfigurasi pam_mount**
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.23(1).jpeg)
```
nvim /etc/pam.d/system-login
```

> Samakan dengan code dibawah.

```
#%PAM-1.0

auth       required   pam_shells.so
auth       requisite  pam_nologin.so
auth       include    system-auth
auth       required   pam_mount.so

account    required   pam_access.so
account    required   pam_nologin.so
account    include    system-auth

password   include    system-auth

session    optional   pam_loginuid.so
session    optional   pam_keyinit.so       force revoke
session    include    system-auth
session    optional   pam_lastlog2.so      silent
session    optional   pam_motd.so
session    optional   pam_mail.so          dir=/var/spool/mail standard quiet
session    optional   pam_umask.so
session    optional  pam_mount.so
-session   optional   pam_systemd.so
session    required   pam_env.so
```

> Untuk memberitahu kepada sistem untuk menggunakan `pam_mount` saat login prosess.

---
## Booster
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.22.jpeg)
```
nvim /etc/booster.yaml
```

add value
```
network:
  dhcp: on
universal: false
modules: -*,ext4,nvme
extra_files: fsck,fsck.ext4
strip: true
enable_lvm: true
```
> Untuk `modules: -*,ext4,nvme` disesuaikan dengan tipe harddisk komputer, jika bertipe `nvme` samakan dan jika `sda atau sdb` tidak perlu tambahkan `nvme`

![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.37.47.jpeg)
```
cd /boot
```

Cek kernel.
```
ls /usr/lib/modules
```
```
booster build --kernel-version <version> /boot/booster-linux-lts-new.img
```
```
rm -fr booster-linux-lts.img
```

---
## Systemd boot
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.37.47(2).jpeg)
```
bootctl --path=/boot install
```
```
nvim /boot/loader/entries/booster.conf
```
```
title    arch with booster
linux    /vmlinuz-linux-lts
initrd   /intel-ucode.img
initrd   /booster-linux-lts-new.img
options  root=/dev/proc/root rw
```
```
nvim /boot/loader/loader.conf
```

add value
```
default  booster.conf
```
```
bootctl --graceful update
```
```
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-jack pipewire-alsa network-manager-applet
```

## Booting
```
exit
```
```
umount -R /mnt
```
```
reboot
```

# Cara Penggunaan Aplikasi  

## KEEPASXC

<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 50 04 AM" src="https://github.com/user-attachments/assets/dfa4e3b9-0dab-4509-bd41-d11c700c94fe" />
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 50 25 AM" src="https://github.com/user-attachments/assets/773c92f6-a17f-4e3b-a2bd-381e2147ff16" />

Masukkan nama database dan password database

<img width="1015" height="1280" alt="WhatsApp Image 2026-06-03 at 6 52 52 AM" src="https://github.com/user-attachments/assets/ef2e9138-ca4c-435f-a0c1-3494c022fb0f" />
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 51 23 AM" src="https://github.com/user-attachments/assets/9162cf0e-38e1-4cd6-a008-976f8842defd" />

Input data login, lalu otomatis data login akan tersimpan di database

---
## SECRETS

<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 54 51 AM" src="https://github.com/user-attachments/assets/698cc991-7b7f-4d9e-89a0-4849d46b58b3" />
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 54 10 AM" src="https://github.com/user-attachments/assets/ae8a08d8-64bc-4ecf-8e90-fabf8c6e5554" />

Buka aplikasi secrets dan akses kembali database berformat .kdbx yang sudah dibuat di aplikasi KeepassXC lalu masukkan password saat 
membuat database di KeepasXC. 

---
## MPV

<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 57 37 AM" src="https://github.com/user-attachments/assets/e913bc76-a978-4d2b-8170-104306570caa" />
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 58 21 AM" src="https://github.com/user-attachments/assets/ca57a4d7-bab1-481e-b269-c9e509c2d3c3" />

```
MPV (link video yang akan ditonton)
```
---
## SUPERFILE

<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 6 59 51 AM" src="https://github.com/user-attachments/assets/b01e4972-3684-4766-948c-8ff05b7fe62e" />
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 7 00 11 AM" src="https://github.com/user-attachments/assets/cee33165-d8d7-4ac4-bb22-3bbedd8d0de9" />

```
spf
```

Lalu buat folder baru

<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 7 01 20 AM" src="https://github.com/user-attachments/assets/e0e3c7ea-2a28-4e78-af5b-2d60000bf61a" />
<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 7 03 18 AM" src="https://github.com/user-attachments/assets/9f4db7a2-294b-4bf9-8906-d1fb640e5cf4" />

```
mkdir (nama folder)
```

> Nama folder tidak boleh menggunakan spasi, agar tidak menjadi folder yang  berbeda.

<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 7 04 07 AM" src="https://github.com/user-attachments/assets/9df82de2-c3b3-4ed3-b393-f6928953c3bb" />

Untuk menghapus folder yang sudah ada.

```
rm -r (nama folder)
```
---

## IP TABLES

<img width="1280" height="720" alt="WhatsApp Image 2026-06-03 at 7 05 00 AM" src="https://github.com/user-attachments/assets/3467e721-f339-4395-b992-c4d8d606340d" />

Untuk masuk ke aplikasi iptables, ketik:

```
sudo iptables -L -n
```
> Digunakan untuk menampilkan daftar rule dan alamat IP dan port dalam bentuk angka.

Untuk menambahkan rule ke dalam iptables, ketik:

```
sudo iptables -A (Chain) -p (protokol) --dport (nomor_port) -j (target)
```

> Chain bisa berupa INPUT, OUTPUT, FORWARD.
> Protokol bisa berupa tcp, udp, icmp.
> Target bisa berupa ACCEPT, DROP, REJECT.

## MPD dan MPC
<img width="1280" height="513" alt="mpd" src="https://github.com/user-attachments/assets/508bccaa-390d-4778-9d0c-67ad02f5c4b5" />

```
mpd
```
```
mpc update 
```
Untuk mengupdate musik dan akan ada tulisan update

```
mpc add namafile.mp3
```
Menambahkan musik dengan mengetik add nama file dengan format mp3 dan file musik ini di pastikan sudah dalam keadaan terdownload dan masuk ke folder tersendiri musik

```
mpc play
```

Untuk memutar (Playing) musik

```
mpc pause
mpc stop
```
Dan jika ingin mengpause musiknya ketik pause dan untuk mengstop musiknya ketik stop


## Open SSH
<img width="1280" height="819" alt="openssh" src="https://github.com/user-attachments/assets/6ede2694-f461-4012-8dc6-54280a89621e" />

OpenSSH ini dapat di lakukkan jika yang ingin diremot harus satu koneksi atau satu wifi agar bisa di remot

```
sudo systemctl start sshd
```

Untuk memulai ssh

```
sudo systemctl status sshd
```

Untuk mengecek apakah ssh nya aktif atau tidak, dilihat dari tulisan bagian yang berwarna hijau jika active (running) itu berarti ssh nya aktif

```
ip address
```

Melihat alamat inet untuk yang ingin diremot

```
ssh user@nomor inet
```

Setelah memasukkan user dan nomor inet masukkan password dari usernya

Dan jika sudah selesai meremot dan ingin melogout

```
exit
```

dan akan ke logout dan akan ada tulisan connecting to (nomor inet) closed tandanya udah ke logout

















