# Install OS Server
asciinema rec installosserver.cast

## 1. Hubungkan ke jaringan
```
iwctl
```
```
station wlan0 get-networks
station wlan0 connect "Nama wifi"
Masukin password wifi
exit
ping 8.8.8.8
ctrl+c
```
## 2. Membagi partisi
```
lsblk
cfdisk /dev/nvme0n1
4G EFI system
44.8 Linux filesystem
write: yes
quit
```
## 3. Cryptsetup dan format luks
```
cryptsetup luksFormat /dev/partisi root
```
ketik "YES"
```
Masukin password untuk root
```
## 4. Buka partisi luks
```
cryptsetup luksOpen /dev/partisi root (nama device)
```
Masukin password
## 5. Volume group
```
vgcreate (nama volume group) /dev/mapper/nama device
```
Hasil
```
vgcreate abur /dev/mapper/fathur
```
## 6. Logical volume
```
lvcreate -L 10G abur -n root
lvcreate -L 8G abur -n vars
lvcreate -L 1G abur -n vlog
lvcreate -L 1G abur -n vaud
lvcreate -L 1G abur -n vtmp
lvcreate -L 8G abur -n podman
lvcreate -L 10 abur -n home
```
## 7. Format partisi
```
mkfs.ext4 /dev/abur/root
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p5 
mkfs.ext4 /dev/abur/vars
mkfs.ext4 /dev/abur/vlog
mkfs.ext4 /dev/abur/vaud
mkfs.ext4 /dev/abur/vtmp
mkfs.ext4 /dev/abur/podman
mkfs.ext4 /dev/abur/home
```
## 8. Mounting
```
mount /dev/abur/root /mnt
mount –mkdir -o uid=0.gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p5 /mnt/boot
mount –mkdir -o rw,nosuid,nodev,relatime /dev/abur/vars /mnt/vars
mount –mkdir -o rw,nosuid,nodev,noexec,relatime /dev/abur/vlog /mnt/var/log
mount –mkdir -o rw,nosuid,nodev,noexec,relatime /dev/abur/vaud /mnt/var/log/audit
mount –mkdir -o rw,nosuid,nodev,noexec,relatime /dev/abur/vtmp /mnt/var/tmp
mount –mkdir -o rw,nosuid,nodev,relatime /dev/abur/podman /mnt/var/lib/containers
mount –mkdir -o rw,nosuid,relatime /dev/abur/home /mnt/home
```
Harus berurutan yyaw
## 9. Install pacstrap yang dibutuhkan
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose podman-desktop
```
Proses gagal karena jaringan terputus sehingga harus kembali mengatifkan jaringan derngan cara berikut
```
iwctl
device list
station wlan0 connect [nama jaringan, lalu masukan pass jaringan]
exit
ping 8.8.8.8 
systemsctl restart iwd
systemsctl enable iwd
systemsctl restart iwd
systemctl start iwd
```
Mengulang proses instalasi dengan command yang sama:
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman podman-compose podman-desktop
## 10. Generate fstab
```
genfstab -U /mnt/fstab
cat /etc/fstab
lsblk
```
Ada kesalahan
```
umount /mnt/home
mount –mkdir -o rw,nosuid,nodev,relatime, /dev/abur/home /mnt/home
lsblk
genfstab -U /mnt /etc/fstab
pacstrab /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo culr neovin iwd firewalld pacman podman podman-compose podman desktop
genfstab -U /mnt /etc/fstab
 lsblk
```
## 11. Masuk ke system arch root
```
arch-root /mnt
```
## 12. Hostname 
```
nvim /etc/hostname
[klik I untuk insert, kemudian insert di baris paling atas]
Laptopfathur 
[lalu :wq dan enter]
cat /etc/hostname
```
## 13.	Sinkronasi waktu (Timezone) 
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
## 14.	Atur Bahasa 
```
nvim /etc/locale.gen
```
[Cari baris #en_US.UTF-8 UTF-8 dan #en_US ISO-8859-1. Lalu hilangkan tanda pagar atau hastag di depannya kemudian enter]
```
locale-gen
locale > etc/locale.conf
nvim /etc/locale.conf
```
[Ganti baris paling atas dan bawah menjadi sebagai berikut lalu wq kemudian enter untuk menyimpan]
LANG=en_US.UTF-8 			[Baris paling atas]
LC_ALL=en_US.UTF-8		[Baris paling bawah]
mkdir -p /home/user
## 15.	Buat user 
```
user add -d /home/user fatur
chown -R fatur:fatur /home/user
passwd fatur
```
[masukkan password yang akan dibuat]
## Sudo
```
echo “fatur ALL=(ALL:ALL) ALL” >> /etc/sudoers.d/fatur
cat /etc/sudoers.d/fatur
```
## 16. Atur parameter 
```
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,-6-nisc.conf}
echo “rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p6)=fatur root=/dev/abur/root” > etc/cmdline.d/01-boot.conf
```
## 17. Mkinitcpio 
```
nvim /etc/mkintcpio.d/default.conf
nvim /etc/cmdline.conf/06-nisc.conf
```
[Masukkan di baris paling atas lalu wq kemudian enter untuk menyimpan]
```
rw
nvim /etc/cmdline.d/06-nisc.conf
```
[Ulangi langkah sebelumnya]
```
mv /etc/mkinitcpio.donf /etc/mkinitcpio.d/default.conf
mv /etc/mkiinitcpio.d/default.conf
```
[Cari baris yang tidak terdapat hastag di depannya diawali oleh kata HOOKS. Kemudian tambahkan berikut setelah vconsole]
block sd-encrypt filesystem fcsk)
cd /bootnf” 811, 3361B written
```
ls
mkdir efi kernel
cd efi/
mkdir linux
ls
rm -fr initramfs-linux-lts.ing
ls
ls efi/
ls kernel/
nvim /etc/mkinitcpio.d/linux-lts.preset
```
[Untuk baris ke 3 hingga ke 5 diawali dengan ALL dan juga baris ke 10 yang diawali #default. Pada baris ke 10 tsb h=perhatikan barus ke 12. Tampilannya seperti ini jika belum maka ubah lah]
## 19.	Install bootctl
```
bootctl –path=/boot install
cat /etc/cmdline.d/01-boot.conf
ls efi/linux
bootctl status
```
```
mkinitcpio -P
bootctl status
```
## 20. Keluar dan Selesaiii yuhuu
```
exit
umount -R /mnt
```
ctrl+d untuk menghentikan rekaman
```
asciinema upload installosserver.cast
```
```
reboot
```

