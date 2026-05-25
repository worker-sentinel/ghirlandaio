# Dokumentasi Instalasi Arch Linux dengan Disk Layout CIS Kelompok 7 Kelas 4-C

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
## CHECKING PARTISI
Lakukan pengecekan dan pembagian partisi dengan langkah-langkah berikut.
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.32.jpeg)
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
root = 30G (linux filesystem)
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
pvcreate /dev/(partisi root)
```
```
vgcreate (nama grup) /dev/(partisi root)
```

**Membuat logical volume**
```
lvcreate -L size (G | M) (nama grup) -n root
```
```
lvcreate -L size (G | M) (nama grup) -n vars
```
```
lvcreate -L size (G | M) (nama grup) -n vtmp
```
```
lvcreate -L size (G | M) (nama grup) -n vlog
```
```
lvcreate -L size (G | M) (nama grup) -n vaud
```
```
lvcreate -L size (G | M) (nama grup) -n root
```
```
lvcreate -L size (G | M) (nama grup) -n home
```
```
lvcreate -l50%FREE (nama grup) -n root (nama)
```
> -l50%FREE adalah 50% dari sisa ruang yang akan digunakan.

**Formatting**
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
mkfs.ext4 /dev/(nama grup)/vtmp
```
```
mkfs.ext4 /dev/(nama grup)/vlog
```
```
mkfs.ext4 /dev/(nama grup)/vaud
```
```
mkfs.ext4 /dev/(nama grup)/home
```
```
mkfs.ext4 /dev/(nama grup)/(nama)
```

Melakukan setup LUKS
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.30.jpeg)
```
cryptsetup luksFormat /dev/(nama grup)/(nama)
```
```
cryptsetup luksOpen /dev/(nama grup)/(nama) (nama device)
```
```
mkfs.ext4 /dev/mapper/(nama device)
```

**Melakukan mounting**
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
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/(nama grup)/home /mnt/home
```

---
## Instalasi Packages
Melakukan instalasi package yang disesuaikan dengan prosesor laptop.
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
### Pam_mount
**Menambahkan User**
![alt text](https://raw.githubusercontent.com/Rafly-87/Studying/refs/heads/main/arch-xfce/WhatsApp%20Image%202026-05-25%20at%2008.38.24.jpeg)
```
mkdir /home/user
```
```
useradd -d  /home/user (nama user)
passwd (nama user)
```
```
chown -R (nama user):(nama user) /home/user
```
```
passwd
```
> Kata sandi harus sama dengan LUKS untuk partisi ini.
```
echo '(nama user) ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none
```
```
sudo mount -o rw,nodev,nosuid,relatime /dev/mapper/(nama device) /home/(nama)
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
modules: -*,ext4
extra_files: fsck,fsck.ext4
strip: true
enable_lvm: true
```

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










