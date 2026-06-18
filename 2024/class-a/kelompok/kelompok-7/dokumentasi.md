mencoba menjalankan command:

```
cryptsetup luksFormat \
--type luks2 \
--cipher aes-xts-plain64 \
--key-size 512 \
--hash sha512 \
--iter-time 5000 \
/dev/nvme0n1p7
```
nanti bakalah disuruh bikin password, nvme0n1p7 typenya linux filesystem, garis miring ke kiri hanya untuk merapihkan

mencoba
```
cryptsetup luksFormat /dev/nvme0n1p7
cryptsetup open /dev/nvme0n1p7 cryptlvm
pvcreate /dev/mapper/cryptlvm
vgcreate vg0 /dev/mapper/cryptlvm
pvs
vgs
```

mencoba
```
lvcreate -L 2G vg0 -n lv_tmp
lvcreate -L 3G vg0 -n lv_var
lvcreate -L 2G vg0 -n lv_var_log
lvcreate -L 1G vg0 -n lv_var_log_audit
lvcreate -L 1G vg0 -n lv_var_tmp
lvcreate -L 5G vg0 -n lv_home
lvcreate -l 100%FREE vg0 -n lv_root
lvs
```
mencoba
```
mkfs.ext4 /dev/vg0/lv_root
mkfs.ext4 /dev/vg0/lv_tmp
mkfs.ext4 /dev/vg0/lv_var
mkfs.ext4 /dev/vg0/lv_var_log
mkfs.ext4 /dev/vg0/lv_var_log_audit
mkfs.ext4 /dev/vg0/lv_var_tmp
mkfs.ext4 /dev/vg0/lv_home
mkfs.fat -F 32 /dev/nvme0n1p6
```
lanjut
```
mount /dev/vg0/lv_root /mnt
mkdir -p /mnt/{boot,tmp,var,home}
mkdir -p /mnt/var/{log,tmp}
mkdir -p /mnt/var/log/audit
mount /dev/nvme0n1p6 /mnt/boot
mount /dev/vg0/lv_tmp /mnt/tmp
mount /dev/vg0/lv_var /mnt/var
mount /dev/vg0/lv_var_log /mnt/var/log
mount /dev/vg0/lv_var_log_audit /mnt/var/log/audit
mount /dev/vg0/lv_var_tmp /mnt/var/tmp
mount /dev/vg0/lv_home /mnt/home
```
kalau ada error saat menjalankan mount /dev/vg0/lv_var_log /mnt/var/log

bertuliskan point doesn't exist, jalankan
```
mkdir -p /mnt/var/log
mkdir -p /mnt/var/log/audit
mkdir -p /mnt/var/tmp
```
lalu jalani
```
mount /dev/vg0/lv_var_log /mnt/var/log
mount /dev/vg0/lv_var_log_audit /mnt/var/log/audit
mount /dev/vg0/lv_var_tmp /mnt/var/tmp
```
lalu error di:
```
mount /dev/vg0/lv_var_log_audit /mnt/var/log/audit
mount point doesn't exist
```
coba:
```
mkdir -p /mnt/var/log/audit
```
mencoba
```
pacstrap -K /mnt base base-devel linux linux-firmware lvm2 cryptsetup mkinitcpio networkmanager sudo vim amd-ucode
```
CATATAN PENTING: sesuai device, kalau Intel, amd-ucode diganti intel-ucode dengan cara:
lscpu | grep "Vendor ID"

mencoba
```
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
```
kalau /home tidak ada, gunakan
```
mount /dev/vg0/lv_home /mnt/home
genfstab -U /mnt >> /mnt/etc/fstab
```
lalu cek lagi dengan
```
cat /mnt/etc/fstab
```
mencoba
```
arch-chroot /mnt
```
untuk waktu:
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc

untuk bahasa:
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf

untuk membuat user, contoh: dika
echo "dika" > /etc/hostname
passwd
useradd -m -G wheel -s /bin/bash dika
passwd dika

lalu jalankan
```
EDITOR=vim visudo
```
saat menjalankan EDITOR=vim visudo, scroll ke paling bawah dan hapus # di baris:
#%wheel ALL=(ALL:ALL) ALL

saat di EDITOR=vim visudo gunakan / untuk mencari kata, i untuk edit :q! untuk cancel edit dan :wq untuk save

mencoba
```
vim /etc/mkinitcpio.conf
```
lalu cari HOOKS yang tidak ada pagar/# seperti di contoh:
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block filesystems fsck)
```
dan HOOKS nya diganti dengan:
```
HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block encrypt lvm2 filesystems fsck)
```
save HOOKS lalu:
```
arch-chroot /mnt
mkinitcpio -P
```
kalau ada error /boot/vmlinux-linux must be readable

coba
```
mount /dev/nvme0n1p6 /boot
```
kalau masih tidak bisa:
```
ls /boot (kalau kosong lanjut ke bawah)
pacman -S linux linux-firmware
```
kalau masih error, cek apakah ada typo di vim /etc/mkinitcpio.conf

lalu pacman lalu mkinitcpio -P

mencoba (abaikan massage merah)
```
blkid -s UUID -o value /dev/nvme0n1p7
bootctl install
```
mencoba **CATATAN PENTING: initrd /amd-ucode.img ganti dengan intel kalau kalian memakai intel**
```
vim /boot/loader/entries/arch.conf
```
lalu memasukkan:
```
title Arch Linux
linux /vmlinuz-linux
initrd /amd-ucode.img
initrd /initramfs-linux.img
options cryptdevice=UUID=90cc9214-41b7-46e2-903b-c1158809e36a:cryptlvm root=/dev/vg0/lv_root rw quiet
```
lalu
```
vim /boot/loader/loader.conf
```
memasukkan:
```
default arch.conf
timeout 3
console-mode max
```
mencoba
```
systemctl enable NetworkManager
exit
umount -R /mnt
reboot
```
masuk ke grub rescue, mencoba
```
cryptsetup open /dev/nvme0n1p7 cryptlvm
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
arch-chroot /mnt
bootctl --esp-path=/boot install
bootctl list
exit
umount -R /mnt
reboot
```
mencoba
```
mount /dev/vg0/lv_root /mnt
mkdir -p /mnt/boot
mount /dev/nvme0n1p6 /mnt/boot
mkdir -p /mnt/sys/firmware/efi/efivars
mount --bind /sys/firmware/efi/efivars /mnt/sys/firmware/efi/efivars
arch-chroot /mnt
bootctl install
cryptsetup open /dev/nvme0n1p7 cryptlvm
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
pacman -S efibootmgr
efibootmgr --create --disk /dev/nvme0n1 --part 6 --label "Arch Linux" --loader /EFI/systemd/systemd-bootx64.efi --unicode
efibootmgr --bootorder 0002,0001,0000
reboot
```
kalau masih hitam coba:

buka enkripsi
```
cryptsetup open /dev/nvme0n1p7 cryptlvm
```
mount root dan boot
```
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
```
cek apakah kernel ada di /mnt/boot:
```
ls /mnt/boot
```
buka enkripsi & mount:
```
cryptsetup open /dev/nvme0n1p7 cryptlvm
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
```
cek isi /boot
```
ls /mnt/boot
```
harus ada:
```
vmlinuz-linux
initramfs-linux.img
initramfs-linux-fallback.img
intel-ucode.img (atau amd-ucode.img)
```

masalah saya tidak ada intel-ucode, caranya:
```
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
mount /dev/vg0/lv_var /mnt/var
arch-chroot /mnt
pacman -S intel-ucode
exit
umount -R /mnt
reboot
```
kalo masih:
```
cryptsetup open /dev/nvme0n1p7 cryptlvm
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
mount /dev/vg0/lv_var /mnt/var
vim /mnt/boot/loader/entries/arch.conf
initrd /intel-ucode.img
```
isi:
```
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options cryptdevice=UUID=350b3bdc-6ca8-479e-aa26-6474c526bf50:cryptlvm root=/dev/vg0/lv_root rw quiet
```
lalu:
```
umount -R /mnt
reboot
```
untuk ganti password:
```
cryptsetup open /dev/nvme0n1p7 cryptlvm
mount /dev/vg0/lv_root /mnt
mount /dev/nvme0n1p6 /mnt/boot
mount /dev/vg0/lv_var /mnt/var
arch-chroot /mnt
passwd
passwd dika
exit
umount -R /mnt
reboot
```
sudah masuk arch linux, lanjut
```
nmtui
ping -c 3 8.8.8.8
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter xorg-server pipewire pipewire-pulse
sudo systemctl enable lightdm
sudo systemctl enable NetworkManager
```
sudah masuk xfce
```
sudo pacman -S git go
cd /tmp
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
yay -S superfile-bin podman-desktop
sudo pacman -S keepassxc openssh mpd mpc mpv podman slirp4netns fuse-overlayfs
```
mencoba
```
sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 riski
systemctl --user enable --now podman.socket
mkdir -p ~/.config/mpd ~/.local/share/mpd ~/Music
sudo pacman -S nano
nano ~/.config/mpd/mpd.conf
```
masukkan
```
music_directory "~/Music"
db_file "~/.local/share/mpd/database"
log_file "~/.local/share/mpd/log"
pid_file "~/.local/share/mpd/pid"
audio_output {
    type "pipewire"
    name "PipeWire"
}
```
lalu CTRL + X lalu Y dan ENTER

lalu
```
systemctl --user enable --now mpd
sudo systemctl enable --now sshd
```
jika sudah, bisa cek status:
```
systemctl --user status mpd
sudo systemctl status sshd
podman info
```
coba
```
sudo pacman -S iptables-nft
sudo nano /etc/iptables/iptables.rules
```
tulis:
```
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]
```
lalu:
```
-A INPUT -i lo -j ACCEPT
-A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p tcp --dport 2222 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT
-A INPUT -p tcp -s 127.0.0.1 --dport 6600 -j ACCEPT
-A INPUT -p tcp -s 127.0.0.1 --dport 8080 -j ACCEPT
```
lalu:
```
COMMIT
```
coba
```
sudo iptables-restore < /etc/iptables/iptables.rules
sudo systemctl enable --now iptables
sudo iptables -L -v --line-numbers
yay -S podman-compose
```
coba
```
cd ~/containers/omeka
nano docker-compose.yml
```
isi:
version: "3.8"
```
services:
db:
image: docker.io/library/mariadb:10.11
environment:
MYSQL_ROOT_PASSWORD: riski123
MYSQL_DATABASE: omeka
MYSQL_USER: omeka
MYSQL_PASSWORD: riski123
volumes:
- ./db-data:/var/lib/mysql:Z
restart: unless-stopped
```
```
omeka:   
image: docker.io/omeka/omeka-s:latest
ports:
- "8080:80"
environment:
OMEKA_DB_HOST: db
OMEKA_DB_NAME: omeka
OMEKA_DB_USER: omeka
OMEKA_DB_PASSWORD: riski123
volumes:
- ./omeka-files:/var/www/html/files:Z
depends_on:
- db
restart: unless-stopped
```
lalu:
```
podman-compose down -v
rm -rf db-data omeka-files
mkdir -p db-data omeka-files
chmod -R 777 omeka-files
podman-compose up -d
podman ps
```
lalu:
```
podman exec omeka_omeka_1 sh -c 'cat > /var/www/html/config/database.ini <<EOF
user = "omeka"
password = "riski123"
dbname = "omeka"
host = "db"
EOF'
```
lanjut:
```
podman-compose restart omeka
```
masuk browser:
http://localhost:8080

coba
```
sudo cp /etc/fstab /etc/fstab.bak
```
lalu jalankan:
```
sudo bash -c 'cat > /etc/fstab << "EOF"
# Static information about the filesystems.
# <file system> <dir> <type> <options> <dump> <pass>

# root
UUID=6838d2c2-9158-4cf5-adbb-9b1676788aee  /               ext4  rw,relatime  0 1

# boot
UUID=E61C-2F7A                              /boot           vfat  rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro  0 2

# tmp
UUID=e076cbb0-9c67-4b51-9949-bbf968974ced  /tmp            ext4  defaults,nodev,nosuid,noexec  0 2

# var
UUID=999ef9b1-2740-48a8-8280-6dbfde8f7b1c  /var            ext4  defaults,nodev,nosuid  0 2

# var/log
UUID=42dc0d23-584a-4dc6-aa35-95260eeb998d  /var/log        ext4  defaults,nodev,nosuid,noexec  0 2

# var/log/audit
UUID=eb4fad80-7913-4523-92f0-b6a45f611fda  /var/log/audit  ext4  defaults,nodev,nosuid,noexec  0 2

# var/tmp
UUID=055393dc-e227-4a90-a4b7-e6f5bfc06f57  /var/tmp        ext4  defaults,nodev,nosuid,noexec  0 2

# home
UUID=GANTI-DENGAN-UUID-LV-HOME             /home           ext4  defaults,nodev,nosuid  0 2
EOF'
```
lalu:
```
sudo blkid /dev/vg0/lv_home
```
UUID yang muncul copy, lalu jalankan dengan command ini:
```
sudo nano /etc/fstab
```
GANTI-DENGAN-UUID-LV-HOME ganti dengan:
```
UUID=dbd8fa3f-63a9-4f3f-a9fd-4eeba9761c9b /home ext4 defaults,nodev,nosuid 0 2
```
save, lalu:
```
sudo mount -a
```
coba
```
sudo nano /etc/sysctl.d/99-cis.conf
```
isi:
```
kernel.dmesg_restrict = 1
kernel.kptr_restrict = 2
kernel.yama.ptrace_scope = 1
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.log_martians = 1
net.ipv6.conf.all.disable_ipv6 = 1
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
fs.suid_dumpable = 0
```
lalu:
```
sudo sysctl --system
```
coba
```
sudo nano /etc/ssh/sshd_config
```
di bawah Include /etc/ssh/sshd_config.d/*.conf tambahkan:
```
Port 2222
PermitRootLogin no
PasswordAuthentication no
MaxAuthTries 3
LoginGraceTime 30
```
lalu:
```
sudo systemctl restart sshd
sudo pacman -S audit
sudo systemctl enable --now auditd
```
coba:
```
ssh-keygen -t ed25519 -C "riski@archsec"
[enter 3x]
```
```
mkdir -p ~/.ssh
chmod 700 ~/.ssh
cp ~/.ssh/id_ed25519.pub ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
verifikasi dengan:
```
cat ~/.ssh/authorized_keys
```

coba:
```
keepassxc
(klik create database)
(2 detik)
(ketik password)
(simpan file)
```
saat di keepasxc:
```
(klik entries)
(klik new entry/CTRL+N)
(Title: LUKS encryption
Username: riski
Password:)
(klik ok)
(CTRL+S untuk save)
```
lalu
```
reboot
```
coba:
```
sudo systemctl status sshd
sudo systemctl status auditd
sudo systemctl status iptables
systemctl --user status mpd
podman ps
```
lalu:
```
sudo cryptsetup luksHeaderBackup /dev/nvme0n1p7 --header-backup-file ~/luks-header-backup.img
```

coba
```
cd ~/containers/omeka
podman-compose up -d
nano ~/.config/systemd/user/omeka.service
```
isi:
```
[Unit]
Description=Omeka-S via Podman Compose
After=network.target

[Service]
WorkingDirectory=%h/containers/omeka
ExecStart=podman-compose up -d
ExecStop=podman-compose down
Type=oneshot
RemainAfterExit=yes

[Install]
WantedBy=default.target
```
setelah save sebelumnya, lanjut:
```
systemctl --user enable --now omeka
loginctl enable-linger riski
```
