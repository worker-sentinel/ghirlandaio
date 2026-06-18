# Installasi OS Linux
## connect wifi
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

## melihat partisi dan typenya
```
lsblk -o name,fstype,size
```

## melihat partisi
```
lsblk
```
## membagi partisi
```
cfdisk /dev/[partisi] (bisa nvme0n1p atau sda tergantung laptop masing-masing)
```
```
boot= 2G (EFI system)
root= 48G (linux filesystem)
```

## Luks
```
cryptsetup luksFormat /dev/[partisi root]
cryptsetup luksOpen /dev/[partisi root] khaila (nama device diperbolehkan apa saja)
```
```
pvcreate /dev/mapper/khaila (nama device)
```
```
vgcreate system /dev/mapper/khaila 
```

## Membuat LVM
```
lvcreate -L 10G system -n root
lvcreate -L 10G system -n vars
lvcreate -L 1G system -n vlog
lvcreate -L 1G system -n vaud
lvcreate -L 1G system -n vtmp
lvcreate -L 10G system -n home
lvcreate -l100%FREE -n dock
```

## formating LV 
```
mkfs.vfat -F32 -n BOOT /dev/[partisi boot]
mkfs.ext4 /dev/system/root
mkfs.ext4 /dev/system/vars
mkfs.ext4 /dev/system/vlog
mkfs.ext4 /dev/system/vaud
mkfs.ext4 /dev/system/vtmp
mkfs.ext4 /dev/system/home
mkfs.ext4 /dev/system/dock
```
```
lsblk
```

## Mounting
```
mount /dev/system/root /mnt
mount --mkdir -o rw,nodev,nosuid,relatime /dev/system/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/system/dock /mnt/var/lib/docker
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi boot] /mnt/boot
```

## Install packages
INTEL
```
pacstrap /mnt intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep docker
```
AMD
```
pacstrap /mnt amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim firewalld pacman which grep docker
```

## Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```

## Ngeformat tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults,rw,nodev,nosuid,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

## Mengcopy network konfigurasi
```
cp /etc/systemd/network/* /mnt/etc/systemd/network/
```

## Chroot
```
arch-chroot /mnt
```

## Hostname
```
echo "khaila" > /etc/hostname
```
>setelah echo buat nama hostname

## Mengatur waktu dan bahasa
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```
```
nvim /etc/locale.gen
```
>search en_US menggunakan simbol / lalu hapus tanda pagar pada en_US.UTF-8 UTF-8 dan en_US ISO-8859-1
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

## membuat user
```
useradd -m khaila (nama user diperbolehkan apa saja)
```
```
passwd khaila
```
```
echo "khaila ALL=(ALL:ALL) ALL" > /etc/sudoers.d/khaila
```
>gunakan nama user

## Kernel
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisi root])=khaila root=/dev/system/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```
```
ls /boot/
```
```
rm /boot/intramfs-linux-lts.img
```
```
nvim /etc/mkinitcpio.conf
```
>pada hooks paling bawah, tambahkan sd-encrypt lvm2 di samping sd-vconsole
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
>hapus semua # pada baris ALL. lalu berikan # pada baris kedua default. setelah itu hapus # pada baris ketiga default dan ganti efi kecil menjadi boot.
```
touch /etc/vconsole.conf
```
```
bootctl --path=/boot install
```
```
mkinitcpio -P
```
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
```
```
systemctl enable firewalld
```

## untuk manager
```
pacman -S xfce4 sddm pipewire pipewire-pulse pipewire-jack networkmanager network-manager-applet firefox
```
>untuk download desktop
```
systemctl enable NetworkManager
```
```
systemctl enable sddm
```

## selesai
```
exit
```
```
umount -R /mnt
```
```
reboot
```

# Dokumentasi Install Apache
## Menginstall Apache
```
pacman -S apache
```
## Edit konfigurasi utama apache
```
nvim /etc/httpd/conf/httpd.conf
```
> hapus tanda pagar LoadModule proxy_wstunnel_module modules/mod_proxy_wstunnel.so

> hapus tanda pagar LoadModule rewrite_module modules/mod_rewrite.so

> hapus tanda pagar Include conf/extra/httpd-vhosts.conf

> hapus tanda pagar LoadModule proxy_module modules/mod_proxy.so

> kemudian esc :wq

## Edit konfigurasi Virtual Host 
```
nvim /etc/httpd/conf/extra/httpd-vhosts.conf
```
> ServerName (namaserverkelompok).local

> ErrorLog "/var/log/httpd/jellyfin-error.log"

> CustomLog "/var/log/httpd/jellyfin-access.log" common

>ProxyPreserveHost On

>ProxyPass / httpd://ip worker:8096/

>ProxyPassReverse / http://ip worker:8096/

## Menghubungkan domain ke alamat ip
```
nvim /etc/hosts
```
> ip worker           local.host

> ip worker           servername.local

## Mengaktifkan apache saat booting
```
systemctl enable httpd
```
## Untuk menjalan apache
```
systemctl start httpd
```
## Untuk memeriksa status apache
```
systemctl status httpd
```
## Restart apache
```
systemctl restart httpd
```
## Keluar
```
exit
```

# Dokumentasi disable modul kernel

---
## Membuka dan mengedit file blacklist
```
nvim /etc/modprobe.d/blacklist.conf
```
## Menambahkan aturan untul modul crafms
```
install cramfs /bin/false
blacklist cramfs
instal freevxfs /bin/false
blacklist freevxfs
install usb_storage /bin/false
blacklist usb_storage 
install hfs /bin/false
blacklist hfs
install hfsplus /bin/false
blacklist hfsplus
install jffs2 /bin/false
blacklist jffs2
install udf /bin/false
blacklist udf
install firewire_core /bin/false
blacklist firewire_core 
```
## Membangun ulang initramfs
```
mkinitcpio -P
```
## Melihat modul yang sedang aktif
```
lsmod
```
## Mengecek apakah cramfs aktif
```
lsmod | grep cramfs
```
## Cek freevxfs
```
lsmod | grep freevxfs
```
## Cek usb_storage
```
lsmod | grep usb_storage
```
## Cek hfs
```
lsmod | grep hfs
```
## Cek hfsplus
```
lsmod | grep hfsplus
```
## Cek jffs2
```
lsmod | grep jffs2
```
## Cek udf
```
lsmod | grep udf
```
## Menampilkan isi konfigurasi modprobe
```
cat /etc/modprobe.d/*.conf
```
## Menampilkan hanya baris blacklist
```
cat /etc/modprobe.d/*.conf | grep blacklist
```
## Keluar dari Terminal
```
exit
```

# Dokumentasi Docker dan jellyfin
## Aktifkan Docker 
```
systemctl enable docker
```
## Jalankan Docker
```
systemctl start docker
```
## Periksa status Docker
```
systemctl status docker 
```
## Set up firewall Manager
membuka port 2377 tcp untuk menghubungkan manager dan worker
```
firewall-cmd --zone=public --add-port=2377/tcp --permanent
```
membuka port 7946 tcp untuk menghubungkan antar node dalam cluster
```
firewall-cmd --zone=public --add-port=7946/tcp --permanent
```
membuka port 7496 udp untuk proses discovery dan pertukaran informasi antar node
```
firewall-cmd --zone=public --add-port=7946/udp --permanent
```
membuka port 4789 udp digunakan oleh jaringan overlay docker untuk komunikasi container antar node
```
firewall-cmd --zone=public --add-port=4789/udp --permanent
```
Untuk menerapkan aturan firewall untuk memuat ulang konfigurasi firewall agar aturan yang baru ditambahkan aktif
```
firewall-cmd --reload
```
## Set up firewall Worker
membuka port 7946 tcp untuk menghubungkan antar node dalam cluster
```
firewall-cmd --zone=public --add-port=7946/tcp --permanent
```
membuka port 7946 udp untuk proses discovery dan pertukaran informasi antar node
```
firewall-cmd --zone=public --add-port=7946/udp --permanent
```
membuka port 4789 udp digunakan oleh jaringan overlay ddocker untuk komunikasi container antar node
```
firewall-cmd --zone=public --add-port=4789/udp --permanent
```
membuka layanan cockpit untuk membuka akses web cockpit (port 9090)
```
firewall-cmd --zone=public --add-service=cockpit --permanent
```
Untuk menerapkan aturan firewall untuk memuat ulang konfigurasi firewall agar aturan yang baru ditambahkan aktif
```
firewall-cmd --reload
```
## Cek alamat ip
```
ip a
```
## Membuat docker swarm 
```
docker swarm init --advertise-addr (ip manager)
```
## Install cockpit di terminal worker
```
pacman - S cockpit
```
## Aktifkan cockpit di terminal worker
```
systemctl enable cockpit
```
## Jalankan cockpit di terminal worker
```
systemctl start cockpit
```
## Cek status cockpit
```
systemctl status cockpit
```
## akses melalui browser untuk membuka terminal worker di device manager
```
ip worker:9090
```
## login user worker, masuk ke terminal worker, copy paste token docker swarm dari terminal manager (hasil dari docker swarm init)

### Jellyfin
## Masuk ke folder Downloads
```
cd Dowwnloads
```
## Tampilkan folder
```
ls
```
## Masuk ke direktori Docker
```
cd /var/lib/docker
```
## Salin file konfigurasi jellyfin
```
cp /home/(namaroot)/Downloads/jellyfin.yml /var/lib/docker/jellyfin
```
## Masuk ke folder jellyfin
```
cd jellyfin
```
## Buat volume konfigurasi
```
docker volume create jellyfin-config
```
## Buat volume media
```
docker volume create jellyfin-media
```
## Buat volume cache 
```
docker volume create jellyfin-cache
```
## Edit file konfigurasi
```
nvim jellyfin.yml
```
> hilangkan ports yang tidak diperlukan sisakan ports target: 8096 dan published: 8096

> pada volumes buat menjadi jellyfin-config:/config, jellyfin-cache:/cache, jellyfin-media:/media``

> pada deploy replicas menjadi 3 dan mas_replicas_per_node menjadi 2

> pada volumes ditambahkan jellyfin-media:

## Deploy jellyfin ke Docker swarm
```
docker stack deploy -c jellyfin.yml media
```
## Lihat daftar service
```
docker service ls
```
## Keluar dari terminal
```
exit
```
