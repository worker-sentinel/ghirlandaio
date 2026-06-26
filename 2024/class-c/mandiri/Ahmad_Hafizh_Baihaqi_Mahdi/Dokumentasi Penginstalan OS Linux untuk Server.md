# Dokumentasi Penginstalan OS Linux untuk Server
## Kelompok 11 Pluto Pioneer
1. Silvi Nur Aini
2. Salfa Firyal Hasanah
3. Fatma Ramadhani
4. Aditya Pangruwating Dhiyu
5. Fauzan Azhiimi
6. Ahmad Hafiz Baihaqi

## Hubungkan ke jaringan internet yang ada
```
iwctl
```
```
station wlan0 get-networks
```
```
station wlan0 connect (nama_jaringan)
```
**ketik password jaringan internetnya**
## Mulai rec Asciinema
```
asciinema rec (nama file).cast
```
## Check partisi yang ada
```
lsblk
```
## Lakukan Crypsetup dan format luks
```
cryptsetup luksFormat /dev/partisi_root
```
lalu, jangan lupa masukin passwd yaaa
## Lanjut kita buka partisi luks
```
crypsetup luksOpen /dev/(partisi_root) (nama_device)
```
## Lanjut kita buat physical volume nya
```
pvcreate /dev/mapper/(nama_device)
```
## Selanjutnya kita buat volume grup
```
vgcreate nama_grup /dev/mapper/(nama_device)
```
## Habis ituuu, kita buat logical volume nya
```
lvcreate -L 10G (nama_grup) -n root
```
```
lvcreate -L 10G (nama_grup) -n vars
```
```
lvcreate -L 1G (nama_grup) -n vlog
```
```
lvcreate -L 1G (nama_grup) -n vtmp
```
```
lvcreate -L 1G (nama_grup) -n vaud
```
```
lvcreate -L 10G (nama_grup) -n home
```
```
lvcreate -l100%FREE (nama_grup) -n podman
```
## Lanjut, kita format Partisi
```
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```
```
mkfs.ext4 /dev/nama_grup/root
```
```
mkfs.ext4 /dev/nama_grup/vars
```
```
mkfs.ext4 /dev/nama_grup/vtmp
```
```
mkfs.ext4 /dev/nama_grup/vaud
```
```
mkfs.ext4 /dev/nama_grup/vlog
```
```
mkfs.ext4 /dev/nama_grup/home
```
```
mkfs.ext4 /dev/nama_grup/podman
```
## Selanjutnya kita periksa kembali yaaa
```
lsblk
```
kalau sudah, kita lanjut ke step berikutnya yaa
## Langkah selanjutnya, kita mounting
**root**
```
mount /dev/nama grup/root /mnt
```
**boot**
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi_boot /mnt/boot
```
**vars**
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/namagrup/vars /mnt/var
```
**vtmp**
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/namagrup/vtmp /mnt/var/tmp
```
**vaud**
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/vaud /mnt/var/log/audit
```
**vlog**
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/vlog /mnt/var/log
```
**home**
```
mount --mkdir -o rw,nosuid,relatime /dev/namagrup/home /mnt/home
```
**podman**
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namagrup/podman /mnt/var/lib/containers
```
## Jangan lupa kita periksa kembali
```
lsblk
```
## Lanjut, kita akan install package yang kita butuhkan ya
```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```
nah itu kalau prosessor laptop kalian intel ya, kalau prosessor kalian amd ikuti command dibawah ini:
```
pacstrap /mnt base amd-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```
## Habis itu kita lanjut Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
## Menyalin settingan jaringan
```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
## Selanjutnya kita tambahkan
```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab
```
## Jangan lupa kita check kembali
```
cat /mnt/etc/fstab
```
## Habis itu, kita bisa langsung masuk ke sistem Chroot
```
arch-chroot /mnt
```
## Selanjutnya kita akan singkronisasi waktunya
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
## Atur bahasa dan lokalisasi nya
```
nvim /etc/locale.gen
```
**nah, kita cari dua en_US dan kita hapus hashtagnya (uncommenting)**
klik esc, lalu ketik:
```
:wq
```
## Generate lokasi
```
locale.gen
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```
**Pada bagian atas yang "C" kita ubah dengan en_US dan jangan lupa pada bagian paling bawahnya kita tambahkan en_US.UTF-8**
## Selanjutnya kita akan membuat user untuk server (disini kami akan menggunakan nama user pluto yaaa)
```
useradd -m pluto
```
```
passwd pluto
```
**kalian bebas untuk buat passwordnya, asal jangan susah susah ya, nanti malah ribet**
**selanjutnya kita akan kasih akses ke usernya**
```
echo "pluto ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/pluto
```
## Langkah selanjutnya kita akan Atur parameter
```
mkdir /etc/cmdline.d
```
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/partisi root)=nama device root=/dev/nama grup/root" > /etc/cmdline.d/01-boot.conf
```
```
echo "rw" > /etc/cmdline.d/02-misc.conf
```
## Lalu, kita atur mkinitcpio
```
nvim /etc/mkinitcpio.conf
```
**Scroll kebawah sampai kalian lihat ada "Hooks" yang ga ada hashtagnya, abis itu kalian klik huruf "i" pada keyboard laptop kalian yang berarti insert. nah di bagian itu ada yang bacaan nya "sd-console" nah sampingnya kalian tambahkan "sd-encrypt" lalu dispasi dan ketik "lvm2"**
Klik esc, dan ketik:
```
:wq
```
## Selanjutnya kita akan mengkonfigurasi mkinitcpio agar ketika mkinitcpio -P dijalankan tidak terjadi eror
```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
**nah, dibagian ini kalian bisa sesuaikan aja ya sama foto dibawah ini:**
<img width="679" height="394" alt="image" src="https://github.com/user-attachments/assets/0f4e7a90-2c13-41d2-8ef2-99afc9e90bf4" />
## Lalu, kita Install bootctl ya
```
bootctl --path=/mnt/boot install
```
```
mkinitcpio -P
```
## Setelah itu kita aktifkan
```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved
```
```
systemctl enable iwd
```
## Selanjutnya kita akan keluar dari Chroot
```
exit
```
```
umount -R /mnt
```
## Stop rec Asciinema
**CTRL+D**
```
asciinema upload (nama_file).cast
```
**Jangan lupa untuk difoto ya link asciinema yang sudah tampil**
## Langkah Akhir
```
reboot
```
