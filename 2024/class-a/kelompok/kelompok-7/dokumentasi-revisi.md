# install arch cis disk layout

## 1. wifi dulu
```bash
iwctl
device list
station (driver wifi) get-network
station (driver wifi) scan
station {device wifi} connect "{nama wifi}"
exit
```
cek dulu
```bash
ping 1.1.1.1
```

---

## 2. partisi
```
lsblk -o name,fstype,size
lsblk
```
```
cfdisk /dev/[nama disk]
```
#### MENYESUAIKAN DENGAN PENYIMPANAN YANG ADA
```
boot = 1G → EFI system
root = sisanya → linux filesystem
```
#### kalo partisi PENTING ke hapus, langsung QUIT jangan di WRITE
```
lsblk
```

---

## 3. lvm
```
pvcreate /dev/[partisi root]
vgcreate proc /dev/[partisi root]
lvcreate -L [size]G proc -n root
lvcreate -L [size]G proc -n vars
lvcreate -L [size]G proc -n vtmp
lvcreate -L [size]G proc -n vlog
lvcreate -L [size]G proc -n vaud
lvcreate -L [size]G proc -n home
lvcreate -l50%FREE proc -n [name]
lsblk
```

---

## 4. format
```
mkfs.ext4 /dev/proc/root
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
mkfs.ext4 /dev/proc/vars
mkfs.ext4 /dev/proc/vtmp
mkfs.ext4 /dev/proc/vlog
mkfs.ext4 /dev/proc/vaud
mkfs.ext4 /dev/proc/home
mkfs.ext4 /dev/proc/[name]
```

---

## 5. luks
```
cryptsetup luksFormat /dev/proc/[name]
```
ketik YES lalu masukin passphrase → **harus sama dengan password user nanti**
```
cryptsetup luksOpen /dev/proc/[name] [nama device]
mkfs.ext4 /dev/mapper/[nama device]
```

---

## 6. mounting
```
mount /dev/proc/root /mnt
mount --mkdir -o uid=0,gid=0,dmask=0077,fmask=0077 /dev/[partisi boot] /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/proc/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/proc/home /mnt/home
lsblk
```

---

## 7. pacstrap
intel:
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```
amd:
```
pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware lvm2 base base-devel neovim openssh superfile podman podman-desktop iptables mpd mpc mpv keepassxc secrets booster networkmanager pam_mount
```

---

## 8. fstab
```
genfstab -U /mnt > /mnt/etc/fstab
echo "/tmpfs /tmp  tmpfs  defaults,nosuid,nodev,noexec,size=1G  0  0" >> /mnt/etc/fstab
```

---

## 9. chroot
```
arch-chroot /mnt
```

---

## 10. hostname
kalo 1 kata ga perlu tanda kutip
```
echo [nama komputer] > /etc/hostname
```

---

## 11. timezone
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

---

## 12. locale
```
nvim /etc/locale.gen
```
cari pake `/` di nvim, uncommenting `en_US.UTF-8 UTF-8`
```
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf
```
ubah jadi:
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```
sisanya ganti `C` → `C.UTF-8`

---

## 13. user & sudo
```
mkdir /home/user
useradd -d /home/user [username]
passwd [username]
```
**password harus sama dengan passphrase luks tadi**
```
chown -R [username]:[username] /home/user
passwd
```
set password root, boleh sama boleh beda
```
echo '[username] ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/none
sudo mount -o rw,nodev,nosuid,relatime /dev/mapper/[nama device] /home/[username]
```

---

## 14. pam_mount
```
nvim /etc/security/pam_mount.conf.xml
```
samain sama ini:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE pam_mount SYSTEM "pam_mount.conf.xml.dtd">
<pam_mount>

<debug enable="0" />

<mntoptions allow="nosuid,nodev,loop,encryption,fsck,nonempty,allow_root,allow_other" />
<mntoptions require="nosuid,nodev" />

<logout wait="0" hup="no" term="no" kill="no" />

<volume
    user="[username]"
    fstype="crypt"
    path="/dev/proc/[name]"
    mountpoint="/home/[username]"
/>

<mkmountpoint enable="1" remove="true" />

</pam_mount>
```
bagian penting:
```xml
<volume
    user="[username]"
    fstype="crypt"
    path="/dev/proc/[username]"
    mountpoint="/home/[username]"
/>
```

```
nvim /etc/pam.d/system-login
```
samain sama ini:
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
session    optional   pam_mount.so
-session   optional   pam_systemd.so
session    required   pam_env.so
```
biar sistem tau pake `pam_mount` waktu login

---

## 15. booster
```
nvim /etc/booster.yaml
```
```yaml
network:
  dhcp: on
universal: false
modules: -*,ext4,nvme
extra_files: fsck,fsck.ext4
strip: true
enable_lvm: true
```
```
cd /boot
ls /usr/lib/modules
```
catat versi kernelnya
```
booster build --kernel-version [versi kernel] /boot/booster-linux-lts-new.img
rm -fr booster-linux-lts.img
```

---

## 16. systemd-boot
```
bootctl --path=/boot install
nvim /boot/loader/entries/booster.conf
```
```
title    arch with booster
linux    /vmlinuz-linux-lts
initrd   /intel-ucode.img
initrd   /booster-linux-lts-new.img
options  root=/dev/proc/root rw
```
amd ganti `intel-ucode.img` → `amd-ucode.img`
```
nvim /boot/loader/loader.conf
```
```
default  booster.conf
```
```
bootctl --graceful update
```

---

## 17. desktop
```
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-jack pipewire-alsa xorg-server xorg-xauth xf86-video-vesa xf86-video-fbdev xfce4-screensaver xfce4-power-manager
systemctl enable sddm
systemctl enable NetworkManager
usermod -aG video,input,tty [username]
echo "needs_root_rights = yes" > /etc/X11/Xwrapper.config
```

---

## 18. done
```
exit
umount -R /mnt
reboot
```
