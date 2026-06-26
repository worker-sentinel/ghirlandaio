# Menginstall OS Client

## Mengaktifkan rekaman asciinema
```
asciinema record serverandromeda.cast
```

## Menghubungkan ke jaringan wifi
```
iwctl
```
```
device list
```

```
station wlan0 get-networks
```
menampilkan hasil scan berupa daftar SSID yang ditemukan

```
station wlan0 scan
```
meminta adapter Wi-Fi wlan0 mencari jaringan Wi-Fi di sekitar
```
station wlan0 connect (nama wifi)
```
```
station wlan0 connect "wifi sekolah"
```
jika nama wifi lebih dari 1 kata. gunakan tanda "..."
```
exit
```
## Memeriksa partisi
```
lsblk
```

## Membuat dan mengatur partisi
```
cfdiks /dev/(partisi [sda/nvme0n1p-])
```

## Jumlah partisi
```
boot = 2g [efi system] nvme0n1p7
root = 51.7g [linux filesystem] nvme0n1p8
```

## Cek kembali partisi
```
lsblk
```
## Format luks
dengan menggunakan root 
```
cryptsetup luksFormat /dev/nvme0n1p7
```
masukkan kata sandi
```
cryptsetup luksOpen /dev/nvme0n1p8 sayyida
```

3# Setup lvm
```
pvcreate /dev/mapper/sayyida
```
```
vgcreate sayyida /dev/mapper/sayyida
```

## Membuat logical volume
```
lvcreate -L 12G sayyida -n root
lvcreate -L 10G sayyida -n vars
lvcreate -L 2G sayyida -n vlog
lvcreate -L 3G sayyida -n vtmp
lvcreate -L 4G sayyida -n vaud
lvcreate -L 4G sayyida -n home
lvcreate -l100%FREE sayyida -n container
```

## Format
```
mkfs.vfat -F32 -n boot /dev/nvmeon1p7
mkfs.ext4 /dev/sayyida/root
mkfs.ext4 /dev/sayyida/vars
mkfs.ext4 /dev/sayyida/vlog
mkfs.ext4 /dev/sayyida/vtmp
mkfs.ext4 /dev/sayyida/vaud
mkfs.ext4 /dev/sayyida/home
mkfs.ext4 /dev/sayyida/container
```

## Periksa partisi Kembali untuk memastikan
```
lsblk
```
## Mount
```
mount /dev/sayyida/root /mnt
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p7 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/sayyida/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/sayyida/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/sayyida/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/sayyida/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/sayyida/home /mnt/home
mount --mkdir -o rw,nosuid,relatime /dev/sayyida/container /mnt/var/lib/docker
```
## Periksa partisi Kembali
```
lsblk
```
## Instal package
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 base sudo curl neovim iwd firewalld pacman which grep docker
```
## Generate fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```

## Copy konfigurasi network dari live environment ke sistem baru
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Tambahkan
```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```

## Periksa Kembali
```
cat /mnt/etc/fstab
```

## Masuk ke system
```
arch-chroot /mnt
```
## Sinkronisasi Waktu
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

## Atur Bahasa dan lokasi
```
nvim /etc/locale.gen
```
Ketik tanda "/" agar pencarian lebih cepat
/en_US

Tekan enter lalu tekan "i"

Lalu hapus tanda "#" pada
en_US.UTF-8 UTF-8
en_US ISO-8859-1

Tekan esc setelah itu :wq

## Menghasilkan dan mengatur konfigurasi locale sistem agar menggunakan bahasa dan format regional en_US.UTF-8
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
isi lang=C menjadi lang=en_US.UTF-8

isi ALL=en_US.UTF-8
```

## Membuat user
```
useradd -m server
```
```
passwd server
```
```
echo "server ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/server
```
## Mengatur parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p7=sayyida) root=/dev/sayyida/root" > /etc/cmdline.d/01-boot.conf
```
## Mengatur mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
<img width="681" height="67" alt="fto 111" src="https://github.com/user-attachments/assets/779becaf-0630-48bf-83f9-224a2aac1fa3" />


Setelah kata 'block' ketik 'sd-encrypt lvm2'

## Mengubah konfigurasi initramfs kernel Linux LTS
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```

## Mengubah seperti yang ada di foto

<img width="517" height="289" alt="fto 22" src="https://github.com/user-attachments/assets/e102fd77-fc37-442d-b566-cf4869e5e530" />


## Install bootctl
```
bootctl --path=/mnt/boot install 
```
Lanjut
```
mkinitcpio -P
```

## Aktifkan systemctl enable
```
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd 
```
## Selesai instalasi os
```
exit
umount -R /mnt
```
## Menghentikan rekaman
ctrl+d

asciinema upload serverandromeda.cast

## Selesai
```
reboot
```
## Kami melakukan penginstallan desktop saat sudah berada di dalam linux-lts
```
sudo iwctl
```
```
device list
```
```
station wlan0 get-networks
```
```
station wlan0 connect (pilih yang ingin dipakai)
```
```
exit
```
## Setelah mengecek jaringan dan berhasil terhubung, lakukan penginstallan desktop dengan command
``` 
sudo su
```
```
pacman -S xfce4 sddm pipewire pipwire-pulse pipewire-jack network-manager network-manager-applet firefox
```
```
pacman -R iwd
```
## Setelah semua terinstall, lakukan command
```
systemctl enable NetworkManager
```
```
systemctl enable docker
```
```
systemctl enable sddm
```
```
exit
```
```
reboot
```
