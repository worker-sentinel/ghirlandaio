# Instalasi OS Server 

## Menghubungkan Wifi
```
iwctl
```
```
device list
```
```
station wlan0 get-network
```
```
station wlan0 scan
```
```
station wlan0 connect "(nama wifinya)"
exit
```
## Memeriksa Partisi 
```
lsblk
```
## Format Luks 
```
cryptsetup luksFormat /dev/nvme0n1p7(partisi root)
```
```
masukkan password
```
```
cryptsetup luksOpen /dev/nvme0n1p7(partisi root) stardust (nama device)
```
## Setup LVM 
```
pvcreate /dev/mapper/stardust (nama device)
```
```
vgcreate system /dev/mapper/(nama device)
```
## Membuat Logical Volume 
```
lvcreate -L 10G system -n root
```
```
lvcreate -L 10G system -n vars
```
```
lvcreate -L 1G system -n vlog
```
```
lvcreate -L 1G system -n vaud
```
```
lvcreate -L 1G system -n vtmp
```
```
lvcreate -L 10G system -n home
```
```
lvcreate -L 5G system -n podman
```
## Memformat Partisi
```
mkfs.vfat -F32 -n BOOT /dev/nvme0n1p6
```
```
mkfs.ext4 /dev/system/root
```
```
mkfs.ext4 /dev/system/vars
```
```
mkfs.ext4 /dev/system/vlog
```
```
mkfs.ext4 /dev/system/vaud
```
```
mkfs.ext4 /dev/system/vtmp
```
```
mkfs.ext4 /dev/system/home
```
```
mkfs.ext4 /dev/system/podman
```
## Cek Partisi apakah sudah terformat atau belum
```
lsblk
```
## Mounting Partisi
```
mount /dev/system/root /mnt
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/system/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vlog /mnt/var/log   
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/podman /mnt/var/lib/containers
```
## Instal Package Arch Linux 
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```
## Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Mengatur Koneksi internet 
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Memformat tmpfs ke tmp
```
echo “tmpfs /tmp tmpfs defaults,rw, nosuid,nodev,noexec,relatime,size=1G 0 0” >> /mnt/etc/fstab
```
## Masuk ke dalam sistem 
```
arch-chroot /mnt
```
## Membuat Hostname 
```
echo stardust > etc/hostname
```
## Mengatur Waktu 
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
## Mengatur Bahasa
```
nvim /etc/locale.gen
```
hapus pagar pada bagian en_US.UTF-8 UTF-8 dan en_US ISO-8859-1  
```
locale-gen
```
```
locale > /etc/locale.conf 
```
```
nvim /etc/locale.conf
```
LANG=C.UTF-8 diganti jadi LANG=en_US.UTF-8 lalu dibagian bawahnya tambahkan LC_ALL=en_US.UTF-8
## Membuat User
```
useradd -m stardust (nama grup)
```
```
passwd (nama password)
```
```
echo "stardust ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/stardust
```
## Mengatur Parameter 
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p7)=stardust root=/dev/system/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
cd /boot                                                                                                                                              
mkdir kernel efi                                                                                                                                   
cd efi                                                                                                                                             
mkdir linux
cd ..                                                                                                                                               
mv vmlinuz-* intel-* kernel                                                                                                                        
ls                                                                                                                                                                                                                                                                                 
rm -fr initramfs-linux-lts.img                                                                                                                     
ls          
ls efi/                                                                                                                                                                                                                                                                                                                
ls kernel/ 
## Mengatur mkinitcpio 
```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
```
nvim /etc/mkinitcpio.d/default.conf 
```
dibagian HOOKS tambahkan sd-vconsole lvm2 sd-encrypt setelah kms keyboard
## Mengedit file konfigurasi
```
nvim /etc/mkinitcpio.d/linux-lts-preset
```
EFI ganti boot
## Menginstall bootloader
```
bootctl --path=/boot install
```
namun karena error yang disebabkan bootnya belum termounting maka disolving terlebih dahulu dengan memounting bootnya lagi yaitu mount /dev/nvme0n1p6 /mnt/boot lalu arch-chroot /mnt lagi dan masukkan lagi bootctl --path=/boot install 
lalu ke step selanjutnya 
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
pada config bagian ALL_kver="/boot/kernel/vmlinuz-linux-lts" isinya diganti menjadi "6.8.35-l-lts" dan begitupun dengan ALL_kerneldest="6.8.35-l-lts" isinya diganti dengan "/boot/kernel/vmlinuz-linux-lts"
setelah itu 
```
mkinitcpio -P
```
## Aktifkan sistem 
```
systemctl enable systemd-resolved
```
```
systemctl enable systemd-networkd
```
```
systemctl enable firewalld
```
setelah itu 
```
exit
```
## Upload Asciinema
```
asciinema upload (namafile).md
```
setelah itu
```
umount -R /mnt
```
```
reboot
```


