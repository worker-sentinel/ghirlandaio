# Install OS untuk Admin

## Kelompok 11 Pluto Pioneer
1. Silvi Nur Aini
2. Fatma Ramadhani
3. Salfa Firyal Hasanah
4. Muhammad Fauzan Azhiimi
5. Aditya Pangruwating Dhiyu
6. Ahmad Hafiz Baihaqi
## Merekam Asciinema
```
asciinema rec nama_file.cast
```
## Connect Jaringan
```
iwctl
```
**Cek jaringan terdekat ada apa aja**
```
station wlan0 get-networks
```
**Masukin nama jaringannya**
```
station wlan0 connect (nama jaringan)
```
```
masukin pass
```
```
exit
```
## Periksa jaringan
```
ping 1.1.1.1
```
## Bagi partisi
```
cfdisk /dev/(partisi)
```
**Untuk membentuk layout yang mau di install**
```
boot = 4G (EFI System)
root = 45.5G (Linux Filesystem)
```
lsblk
```
## Cryptsetup dan Format Luks
```
cryptsetup luksFormat /dev/(partisi root)
```
```
Ketik YES
```
Masukin pass
```
```
Verify pass
```
```
cryptsetup luksOpen /dev/(partisi root) (nama device)
```
## Setup LVM
```
pvcreate /dev/mapper/(nama device)
```
```
vgcreate (nama grup) /dev/mapper/(nama device)
```
## Membuat Logical Volume
```
lvcreate -L 15G (nama grup) -n root
```
```
lvcreate -L 10G (nama grup) -n vars
```
```
lvcreate -L 1G (nama grup) -n vlog
```
```
lvcreate -L 512M (nama grup) -n vaud
```
```
lvcreate -L 1G (nama grup) -n vtmp
```
```
lvcreate -l50%FREE (nama grup) -n home
```
```
lvcreate -l100%FREE (nama grup) -n podman
```
## Cek Partisi
```
lsblk
```
## Formating
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
```
mkfs.ext4 /dev/(nama grup)/podman
```
```
mkfs.vfat -F32 /dev/(partisi boot)
```
## Mounting
```
mount /dev/(nama grup)/root /mnt
```
```
mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/(partisi boot) /mnt/boot
```
```
mount –mkdir -o rw,nosuid,nodev,relatime /dev/(nama grup)/vars /mnt/var
```
```
mount –mkdir -o rw,nosuid,nodev,noexec,relatime /dev/(nama grup)/vlog /mnt/var/log
```
```
mount –mkdir -o rw,nosuid,nodev,noexec,relatime /dev/(nama grup)/vaud /mnt/var/log/audit
```
```
mount –mkdir -o rw,nosuid,nodev,noexec,relatime /dev/(nama grup)/vtmp /mnt/var/tmp
```
```
mount –mkdir -o rw,nosuid,nodev,relatime /dev/(nama grup)/home /mnt/home
```
```
mount –mkdir -o rw,nosuid,nodev,relatime /dev/(nama grup)/podman /mnt/var/lib/containers
```
## Install package
**ini intel ya, klo km amd sung Ganti amd aj**
```
pacstrap –K /mnt linux-lts linux-lts-headers base pacman sudo firewalld openssh podman neovim git grep linux-firmware networkmanager intel-ucode mkinitcpio lvm2 curl which iwd
```
```
Proceed with installation? Y
```
## Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Menyalin jaringan konfigurasi
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Ngeformat tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
## Chroot
```
arch-chroot /mnt
```
## Hostname 
```
echo “nama hostname”  > /etc/hostname
```
## menganukan waktu dan Bahasa
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock –systohc
```
```
nvim /etc/locale.gen
```
>Klik I untuk mencari en_US trs enter. Klik I untuk menghapus tagar pada en_US.UTF-8 dan en_US ISO8859-1, biar aktif trs klik esc. 
```
:wq
```
```
locale-gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
```
>LANG=C.UTF-8 ganti menjadi LANG=en_US.UTF-8 lalu dibagian LC_ALL= tambahkan en_US.UTF-8
```
## Membuat user
```
useradd -m (nama user)
```
```
passwd (nama user)
```
```
echo "(nama user) ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/(nama user)
```
## Kernel parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/(partisi root))=(nama luks) root=/dev/(nama grup)/root" > /etc/cmdline.d/satu-boot.conf
```
## Untuk melihat isi file
```
cat /etc/cmdline.d/satu-boot.conf
```
## Melihat partisi disk dengan opsi name dan UUID
```
lsblk -o name,UUID
```
## Mengedit konfigurasi mkinitcpio.conf
```
nvim /etc/mkinitcpio.conf
```
>klik / untuk mencari HOOKS. Klik I untuk menambahkan sd-encrypt lvm2 setelah sd-vconsole. Klik esc
```
:wq
```
## Mengedit konfigurasi mkinitcpio.d/linux-lts.preset
>klik I untuk menghapus tagar pertama ALL_config, ALL_kerneldest. Lalu menambahkan tagar pada default_image. Trs menghapus tagar pada default_uki. Trs di situ edit kata katanya jadi kaya gini pokonya = “/boot/EFI/Linux/arch-linux-lts.efi” kemudian klik esc
```
:wq
```
## Tampilkan folder
```
ls
```
## Masuk ke boot
```
cd /boot
```
## Install bootctl
```
bootctl –path=/boot install
```
*(kalo laptop lenovo bootctl nya di luar dari arch-chroot)*
## Aktifkan sebagai berikut
```
systemctl enable system-networkd
```
```
systemctl enable system-resolved
```
```
systemctl enable NetworkdManager
```
## Keluar dari arch-chroot
```
exit
```
## Melepaskan label mountpoint
```
umount -R /mnt
```
## Stoph rekaman
```
Ctrl + d
```
Atau
```
exit
```
## Upload asciinema
```
asciinema upload (nama file).cast
```
*nnti enter 2x trs ada link yg pertama itu di foto atau sung aj di ketik di hp trs dicek dulu itu bisa kebuka apa ngga soalnya biasanya ga bisa kebuka karna typo*
## Selesai
```
reboot
```
