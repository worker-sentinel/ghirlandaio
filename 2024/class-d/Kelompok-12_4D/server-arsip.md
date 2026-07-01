# partition
```
pvcreate /dev/partition
```
```
vgcreate proc /dev/partition
```

## root
```
lvcreate -L size (G | M) group_name -n root
```
```
mkfs.ext4 /dev/group_name/root
```
```
mount /dev/group_name/root /mnt
```

## boot
```
mkfs.vfat -F32 -n BOOT /dev/partition
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/paritition /mnt/boot
```


## var
```
lvcreate -L size (G | M) group_name -n vars
```
```
mkfs.ext4 /dev/group_name/vars
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/vars /mnt/var
```


## vtmp
```
lvcreate -L size (G | M) group_name -n vtmp
```
```
mkfs.ext4 /dev/group_name/vtmp
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/vtmp /mnt/var/tmp
```

## vlog
```
lvcreate -L size (G | M) group_name -n vlog
```
```
mkfs.ext4 /dev/group_name/vlog
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/vlog /mnt/var/log
```

## vaud
```
lvcreate -L size (G | M) group_name -n vaud
```
```
mkfs.ext4 /dev/group_name/vaud
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/vaud /mnt/var/log/audit
```

## home public
```
lvcreate -L size (G | M) group_name -n home
```
```
mkfs.ext4 /dev/group_name/home
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/group_name/home /mnt/home
```
## podi
```
lvcreate -L size (G | M) group_name -n podi
```
```
mkfs.ext4 /dev/group_name/home
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/group_name/home /mnt/var/lib/containers
```

## home internal
```
lvcreate -l50%FREE group_name -n priv
```
```
cryptsetup luksFormat /dev/group_name/priv
```

# packages
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep podman openssh
```
# fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```

# tmpfs

```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

# chroot
```
arch-chroot /mnt
```

## hostname
```
echo [nama komputer] > /etc/hostname
```

## localtime
```
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

## locale

```
nvim /etc/locale.gen
```
uncommentin both `en_US.UTF-8`
```
en_US.UTF-8
en_US.ISO-8859-1
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
add value like below
```
LANG=en_US.UTF-8
LC_ALL=en_US.UTF-8
```

## pam_mount
```
cryptsetup luksOpen /dev/group_name/priv internal
```
```
mkfs.ext4 /dev/mapper/internal
```
## user
```
mkdir -p /home/user
```

```
useradd -d /home/user [user_name]
```

```
chown -R [user_name]:[user_name] /home/user
```

```
passwd [user]
```
> password must same like luks for this partition

```
echo '[nama_user] ALL=(ALL:ALL) ALL' > /etc/sudoers.d/none
```
                                                                                 
### Configure the Volume

```
nvim /etc/security/pam_mount.conf.xml
```
> adjust like the lines below
```/etc/security/pam_mount.conf.xml
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
    path="/dev/proc/priv" 
    mountpoint="/home/user" 
/>
		<!-- pam_mount parameters: Volume-related -->

<mkmountpoint enable="1" remove="true" />


</pam_mount>
```

Edit the `pam_mount` configuration file at `/etc/security/pam_mount.conf.xml`. You need to add a `<volume>` entry for your encrypted device.

```xml
<!-- Example entry for a LUKS partition -->
<volume 
    user="[user name]" 
    fstype="crypt" 
    path="/dev/proc/priv" 
    mountpoint="/home/user" 
/>
```
*   **path:** The identifier for your encrypted partition (e.g., `/dev/sdb1` or a UUID).
*   **mountpoint:** Where the partition should be accessible after unlocking.
*   **options:** `allow-discard` is useful for SSD performance.

### Update PAM Configuration

```
nvim /etc/pam.d/system-login
```
> adjust like the lines below
```/etc/pam.d/system-login
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

You must tell the system to use `pam_mount` during the login process. Edit `/etc/pam.d/system-login` to include the following lines in the correct sections:

```/etc/pam.d/system-login
# Add to the 'auth' section
auth        required    pam_mount.so

# Add to the 'session' section
session     optional    pam_mount.so
```
*Note: If you use a Display Manager (like GDM or SDDM), ensure its specific PAM file also includes these or inherits from `system-login`.*

## cmdline
```
mkdir -p /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "root=/dev/group_volume/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

## boot
```
rm -fr /boot/initramfs-linux-*
```

## initramfs
```
nvim /etc/mkinitcpio.conf
```
add `lvm2` after `sd-vconsole`
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole block lvm2 filesystems fsck)
```
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
the same as the configuration below
```
# mkinitcpio preset file for the 'linux-lts' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-linux-lts"
ALL_kerneldest="/boot/vmlinuz-linux-lts"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-lts.img"
default_uki="/EFI/Linux/arch-linux-lts.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-lts-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-lts-fallback.efi"
#fallback_options="-S autodetect"

```
```
touch /etc/vconsole.conf
```
```
bootctl --path=/boot install
```
```
mkinitcpio -P
```
## service
```
systemctl enable systemd-networkd
```

```
systemctl enable systemd-resolved
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

## booting
```
exit
```
```
umount -R /mnt
```
```
reboot
```

## after booting
```
sudo chown -R nama_user:nama_user /home/[nama user]
```
```
sudo systemctl enable sddm
```
```
reboot
```
### setup firewalld
1. check zone. example in below
```
sudo firewall-cmd --list-all-zone
```
2. allow port. example in below

```
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
```
3. allow service. example in below
```
sudo firewall-cmd --zone=public --add-service=ssh --permanent
```
4. allow port rich rules
```
sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source address="ip_admin" port port="port_apk" protocol="tcp" accept'
```
5. remove service. example in below
```
sudo firewall-cmd --zone=internal --remove-service=ssh --permanent
```
>[Note]
> hapus semua service dan port selain di zone public
```
sudo firewall-cmd --reload
```

### setup hardening kernel
#### example for disale module kernel
for check wireless device
```
lspci -knnd ::0280
```
value
```
02:00.0 Network controller [0280]: Intel Corporation Dual Band Wireless-AC 3168NGW [Stone Peak] [8086:24fb] (rev 10)
	Subsystem: Intel Corporation Device [8086:2110]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
```
> iwlwifi is a module
```
sudo nvim /etc/modprobe.d/01-hard.conf
```
value
```
install usb-storage /bin/false
blacklist usb-storage
install iwlwifi /bin/false
blacklist iwlwifi
```
```
sudo modprobe -r usb-storage
```
```
sudo mkinitcpio -P
```
### setup rootless podman
#### adding subuid and subgid

```
sudo nvim /etc/subuid
```

```/etc/subuid
[user]:100000:65536
```

```
sudo nvim /etc/subgid
```

```/etc/subgid
[user]:100000:65536
```

## enable global podman

```
sudo systemctl enable --global podman
```

## configure storage

```
mkdir -p .config/containers/
```

```
nvim .config/containers/storage.conf
```

```
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```

#### registries podman
```
nvim /etc/containers/registries.conf
```
```
unqualified-search-registries = ["docker.io"]
```
