## installasi OS blackbird-amanda dengan CIS

isntallasi linux bllackbird-amanda-os dengan implementasi CIS 

## nyalakan internet 

iwctl 

cari device wifi
```
device list
```
 
untuk me list device yang ada, setelah ada dan fungsinya on bisa kalian lanjutlkan tetapi jika beluk kalian bisa lihat pada [https://wiki.archlinux.org/title/Iwd](url) untuk configurasinya 

```
station wlan0 scan 
```
biasanya device menggunakan wlan0, scan berfungsi untuk menscanning wifi yang ada 

```
station <device_network> get-network 
```
untuk mendapatkan network <br>

```
station wlan0 connect "namanetwork"
```
masukan pharaphrashe; paswordnya ke parapharse 

ping 8.8.8.8  

## membuat partisi untuk CIS 
```
cryptsetup luksFormat /dev/[partisimu] 
```
cryptsetup untuk mengunci dan membuat encrypsi pada partisi tersebut , menggunakan partisi yang kamu pilih 

kemudian buka partisi dengan 
```
cryptsetup luksOpen /dev/[partisi] [nama-device-yang-ingin-dibuat] 
```

buat volume
* pvcreate 
* vgcreate 
* lvcreate 

## membuat logical volume pada volume gruop 
```
pvcreate /dev/mapper/[namadevice-yang-telah-dibuat] 
vgcreate [namavolumegrup-yang-ingin-dibuat] /dev/mapper/[namadevice-yang-ingin-dibuat] 

// untuk size atau besaran isi volume menyesuaikan dari besaran partisi 

lvcreate -L ( G | M ) [namavolumegruop] -n root <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vars <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vtmp <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vlog <br>
lvcreate -L ( G | M ) [namavolumegruop] -n vaud <br>
lvcreate -L ( G | M ) [namavolumegruop] -n home <br>
lvcreate -l100%FREE  [namavolumegruop] -n podman  // podman sedikit berbeda karena ini untuk server <br>
```

## membuat format 
```
mkfs.vfat  -F 32 -n BOOT /dev/[partisibuatmu] 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /root 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vars 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vtmp 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vlog 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /vaud 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /home 
mkfs.ext4 -b4096 /dev/[namavolumgroup] /podman
```

## mounting 
```
mount /dev/[namavolumegroup]/root /mnt 
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/[partisi-boot] /mnt/boot 
mount --mkdir -o rw,nosuid,nodev,relatime /dev/[namavolumegroup]/var /mnt/var 
mount --mkdir -o rw,nosuid,nodev,,noexec,relatime /dev/[namavolumegroup]/vtmp /mnt/var/tmp 
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/vlog /mnt/var/log 
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/vaud /mnt/var/log/audit 
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/home /mnt/home 
mount --mkdir -o rw,nosuid,nodev,noexec,relatime /dev/[namavolumegroup]/podman /mnt/var/lib/containers 
```

mkdir itu fungsinya untuk membuat directory dari root,vars,etc 
root dan vars tidak menggunakan option noexec agar system dapat membaca root,vars dan mengeksekusinya

## pacstrap 
pacstrap fungsinya untuk mengclone packages repository dari Arch karena balckbird-amanda berbasis Arch dan memasukannya ke dalam ISO sebelum menggunakan dan masuk dalam environment arch-chroot <br> 

sesuaikena dengan prosesor laptop 
* gunakan [amd-ucode] untuk amd
*  gunakan [intel-ucode] untuk intel 
```
pacstrap /mnt pacstrap /mnt base intel-ucode linux-lt linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman docker
```

## copy configurasi network dari ISO ke arch-chroot 
cp /etc/systemd/network/* /mnt/etc/systemd/network 

## fstab 
```
genfstab -U /mnt >> /mnt/etc/fstab      
```
untuk mengenerate mounting pada fstab ketika file menyiapkan initramfs, dapat menentukan outomatis file mana yang akan di mounting atau di gunakan lalau menyiapkannya pada bootloader untuk menggunakan initramfs 

```
 echo "tmpfs /tap tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab  /tmpfs
```
untuk temporay seperti chache agar langsung menggunakan ram dan dapat memproses percepatan mounting juga penghapusan chache secara automatis, diharapkan tidak salah dalam penulisan 

## arch-chroot 
```
arch-chroot /mnt 
```

## set hostname
```
echo "namalaptopkamu" >> /etc/hostname  
cat /etc/hostname
```

## set time zone 
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime 
hwclock --systhoc
```

## localegen 
```
nvim etc/locale.gen 
```
uncomenting tanda # pada en_US contoh : #en_US <br> 
kemudian
```
locale-gen
```
pindahkan file locale
``` 
locale >> /etc/locale.conf 
```

## config useradd
```
useradd -m [namausermu] 
passwd [namausermu] 
echo "[namausermu] ALL=(ALL:ALL) ALL" >> /etc/sudoers.d/namamu
```

## membuat paramenter untuk initramfs atau kernel parameters 
untuk membuat direktori untuk kernel parameternya
```
mkdir /etc/cmdline.d
```
memasukan kernel parameter ke file
```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/[partisirootkamu])=nama [devicemu] root=/dev/[volumegroup]/root" > /etc/cmdline.d/01-boot.conf 
echo "rw" > /etc/cmdline.d/02-misc.conf
```

## configurasi mkinitcpio

### menghapus intiramfs-linux-lts.img 
```
ls /boot 
rm /boot/initramfs-linux-lts.img 
``` 
```
nvim /etc/mkinitcpio.conf 
```
cari hooks yang tidak memiliki comment# lalumasukan sd-encrypt lvm2 setelah sd-vconsole <br> 

```
nvim /etc/mkinitcpio.d/linux-lts-preset <br>
```
uncomenting #  ALL_config, ALL_kver,ALL_kerneldest 
commenting # default_config 
uncomennting # default_uki , hapus /efi/ yang kecil lalu ganti dengan /boot/ 

## boot looader 
bootloader menggunakan systemd-boot lakukan dengan ,   
```
bootctl --path=/boot install  <br> 
```

## generate 
```
mkinitcpio -P 
```

## enable service 
```
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd
```

## penutup
```
exit
umount -R /mnt
reboot
```


# referensi
https://github.com/worker-sentinel/ghirlandaio/blob/main/2024/class-b/mandiri/Kaafi_Alfath_Syahri/Tugas_kelompok_10_Supernova/Install_os_linux_server.md
umount -R /mnt <br> 

reboot <br>
