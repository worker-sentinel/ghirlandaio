### Instalasi OS Server

1. Sudo Cryptsetup luksFormat /dev/nvme0n1p5

YES

Masukin passwd

2.     Cryptsetup luksOpen /dev/nvme0n1p5 novaria

Masukin passwd

3.     Pvcreate /dev/mapper/Novaria

4.     Vgcreate kel4 /dev/mapper/Novaria

5.     Lvcreate -L 8G kel4 -n root

Mkfs.ext4 -b 4-96 /dev/kel4/root

Mount /dev/kel4/root /mnt

6.     Mkfs.vfat -F32 -n BOOT /dev/nvme0n1p4

Mount –mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p4 /mnt/boot

7.     Lvcreate -L 8G kel4 -n vars

Mkfs.ext4 -b 4096 /dev/kel4/vars

Mount --mkdir -o rw,nodev,nosuid,relatime /dev/kel4/vars /mnt/var

8.     lvcreate -L 1G kel4 -n vlog

mkfs,ext4 -b 4096 /dev/kel4/vtmp

mount –mkdir -o rw,nodev,nosuid,relatime /dev/kel4/vtmp /mnt/var/tmp

9.     lvcreate -L 1G kel4 -n vlog

mkfs.ext4 -b 4096 /dev/kel4/vlog

mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/vlog /mnt/var/log

10.  lvcreate -L 1G kel4 -n vaud

mkfs.ext4 -b 4096 /dev/kel4/vaud

mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/vlog /mnt/var/log/audit

11.  lvcreate -L 8G kel4 /dev/kel4/home

mkfs.ext4 -b 4096 /dev/kel4/home

mount –mkdir -o rw,nodev,nosuidnoexec,relatime /dev/kel4/home /mnt/home
 
12.  pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base base=devel neopvim firewalldopenssh
 
iwctl

station wlan0 connect “Kelawai Ciputat 01_ 5G”

exit

systemctl enable iwd

systemctl start iwd

systemctl enable system-networkd

systemctl start system-networkd

systemctl restart system-networkd

iwctl

station wlan0 connect “Kelawai Ciputat 01_ 5G”

exit

13.  pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman podman networkmanager  

14.  genfstab -U /mnt/ > /mnt/etc/fstab

cp /etc/system/network/* /mnt/etc/system/network

echo “tmpfs /tmp tmpfs deafults,nosuid,noexec,size=1G 0 0” >> /mnt/etc/fstab

cat /mnt/etc/fstab

15.  lvcreate -L 5G kel4 -n podman

mkfs.ext4 /dev/kel4/podman

mount –mkdir -o rw,nosuid,relatime /dev/kel4/podman /mnt/var/lib/containers

16.  arch-root /mnt

17.  Timezone

ln -sf /esr/share/zoneinfo/Asia/Jakarta /etc/localtime’

hwclock –sytohc

18.  nvim /etc/locale.gen

#en_US.UTF-8 UTF-8

#en_US ISO-8859-1

Hapus hastag-nya

en_US.UTF-8 UTF-8

en_US ISO-8859-1

esc kmudian :wq

locale-gen

locale > /etc/locale.conf

nvim /etc/locale.conf

paling atas LANG=en_US.UTF-8

paling bawah LC_ALL=en_US.UTF-8

:wq

19.  useradd -m Nayla

mskuin passwd

echo "nayla ALL=(ALL:ALL) ALL" > /etc/sudoers.d/nayla                                                                                                                                                                       mkdir /etc/cmdline.d                                                                                                                                                                                                        touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}                                                                                                                                                                            echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p5)=name novaria=/dev/kel4/root" > /etc/cmdline.d/01-boot.conf

echo "rw" > /etc/cmdline.d/02-misc.conf 

20.   nvim /etc/mkinitcpio.conf

ALL_config="/etc/mkinitcpio.conf"                                                                                                                                                                                                     

ALL_kver="/boot/kernel/vmlinuz-linux-lts"                                                                                                                                                                                                       
ALL_kerneldest="/boot/kernel/vmlinuz-linux-lts"
 

#default_config="/etc/mkinitcpio.conf"                                                                                                                                                                                                          

#default_image="/boot/initramfs-linux-lts.img"                                                                                                                                                                                                  

default_uki="/boot/efi/Linux/arch-linux-lts.efi"                                                                                                                                                                                                

#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"                                                                                                                                                                         

HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block sd-encrypt lvm2 filesystems fsck) kmudian :wq                                                                                                                    
 
22.  bootctl --path=/boot install
pacman -S linux-lts linux-lts-headers
23.  systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
24.  exit
umount -R /mnt
ctrl+d
ipload asciinema
reboot

