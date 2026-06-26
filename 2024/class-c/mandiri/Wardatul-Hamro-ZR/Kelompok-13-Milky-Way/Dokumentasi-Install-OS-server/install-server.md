#### buat asciinema nya dulu buat ngerecord
```
asciinema rec (namafile).cast
```
#### connect wifi 
```
iwctl
```
```
station wlan0 get-networks
```
```
station wlan0 connect (nama wifi)
```
```
exit
```
#### kemudian cek wifi nya udah connect atau belum 
```
ping 8.8.8.8
```
```
cryptsetup LuksFormat /dev/sda7
```
```
masukin password
```
```
cryptsetup LuksOpen /dev/sda7/ping
```
```
pvcreate /dev/mapper/ping
```
```
vgcreate list /dev/mapper/ping
```
```
lsblk
```
```
lvcreate -L 8G lit -n root
```
```
lvcreate -L 5G lit -n vars
```
```
lvcreate -L 1G lit -n vlog
```
```
lvcreate -L 1G lit -n vaud
```
```
lvcreate -L 1.5G lit -n vtmp
```
lvcreate l50%FREE lit -n home
```
```
lvcreate /100%FREE lit -n podman
```
```
lsblk
```
```
mkfs.ext4 /dev/lit/root
```
```
mkfs.vfat -F32 /dev/sda5
```
```
mkfs.ext4 /dev/lit/vars
```
```
mkfs.ext4 /dev/lit/vlog
```
```
mkfs.ext4 /dev/lit/vaud
```
```
mkfs.ext4 /dev/lit/vtmp
```
```
mkfs.ext4 /dev/lit/home
```
```
mkfs.ext4 /dev/lit/podman
```
```
mount /dev/lit/root /mnt
```
```
lsblk
```
```
mount --mkdir -o uid-0,gid-0,fmask=0077,dmask=0077 /dev/sda5 /mnt/boot
```
```
mount --mkdir -o rw,nodev.nosuid,relatime /dev/lit/vars /mnt/var
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/lit/vlog /mnt/var/log
```
mount --mkdir -o rw.nodev.nosuid,noexec,relatime /dev/lit/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/lit/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nodev,relatime /dev/lit/home /mnt/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/lit/podman /mnt/var/lib/containers
```
```
lsblk
```
#### kemudian masuk ke install os servernya 
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman vget curl grup iwd podman
```
#### kemudian setelah terinstall seperti dibawah ini
```
genfstab -U /mnt > /mnt/etc/fstab
```
```
echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
```
```
cp /etc/systemd/network/* /mnt/etc/sytemd/network
```
```
arch-chroot /mnt
```
```
echo server > /etc/hostname
```
```
ls -sf /usr/share/zoneinfo/Asia/Jakarta/etc/localtime
```
```
hwclock --systohc
```
nvim /etc/locale.gen
```
```
locale-genwritten
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
```
useradd -m milky
```
```
passwd milky
```
#### masukin passowrdnya
```
echo "milky ALL=(ALL:ALL) ALL" > /etc/sudoers.d/mmilky
```
```
su milky
```
```
sudo su
```
#### masukin password lagi
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-nisc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/sda7)=ping root=/dev/lit/root" > /etc/cmdlie.d/01-boot.conf
```
```
cat /etc/cmdline.d/01-boot.conf
```
```
echo rw > /etc/cmdline.d/02-nisc.conf
```
```
nvim /etc/mkinitcpio.conf
```
```
nvim /etc/mkinitcpio.d/linux-lts.conf.preset
```
```
bootctl --path=/boot installwritten
```
```
mkinitcpio -p
```
```
systemctl enable systemd-networkd
```
```
sysytemctl enable systemd-resolved
```
```
systemctl enable iwd
```
```
systemctl enable firewalld
```
```
systemctl enable sshd
```
```
exit
```
#### kalo udah sampe keinstall lanjut ke step selanjutnya
```
lsmod | grep cramfs
```
```
lsmod | grep freevxfs
```
```
lsmod | grep hfs
```
```
lsmod | grep hfsplus
```
```
lsmod | grep jffs2
```
```
lsmod | grep overlayfs
```
```
lsmod | grep squashfs
```
```
lsmod | grep udf
```
```
lsmod | grep usb-storage
```
```
nvim /etc/modprobe,d/01-disable-module.conf
```
```
install usb-storage /bin/false
```
```
blacklist usb-storage
```
```
lsmod | grep afsnf
```
```
lsmod | grep ceph
```
```
lsmod | grep cifs
```
```
lsmod | grep exfat
```
```
lsmod | grep ext
```
```
lsmod | grep fat
```
```
lsmod | grep fscache
```
```
lsmod | grep fuse
```
```
lsmod | grep gfs2
```
```
lsmod | grep nfs-commmon
```
```
lsmod | grep nfsd
```
```
lsmod | grep smbfs_common
```
```
nvim /etc/modprobe.d/disable-enable.conf
```
```
mkinitcpio -p
```
```
exit
```
