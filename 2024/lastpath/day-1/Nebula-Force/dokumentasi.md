# Installasi OS

## **Menghubungkan ke internet:**
masuk ke aplikasi iwd untuk menghubungkan komputer ke jaringan WiFi sebelum proses instalasi dimulai dengan command

```
iwctl 
```

**Mengaktifkan asciinema**
mulai rekaman pada layar terminal untuk merekam semua aktivitas yang dilakukan di terminal dengan command

```
asciinema rec (nama rekaman)
```

## Periksa partisi terlebih dahulu
tampilkan daftar penyimpanan untuk melihat daftar partisi yang ada di masing-masing laptop dengan command

```
lsblk
```

## cryptsetup dan format luks
membuat enskripsi pada partisi untuk melindungi data dengan menggunakan command

```
cryptsetup luksFormat /dev/mmcblk0p6
```

**Kemudian, masukan password.**

## Membuka partisi luks
buka partisi yang sudah dienskripsi supaya bisa diakses selama proses instalasi dengan menggunakan command

```
cryptsetup luksOpen /dev/mmcblk0p6 advan7
```

## Membuat physical volume (PV)
sebuah ruang penyimpanan yang bisa dikelola oleh LVM (Logical Volume Manager) dengan menggunakan command

```
pvcreate /dev/mapper/advan7
 ```

## Selanjutnya membuat volume grub (VG)
sebagai tempat pengumpulan satu atau beberapa physical volume menjadi satu ruang penyimpanan besar dengan menggunakan command

```
vgcreate nebula /dev/mapper/advan7
```

## Membuat logical volume (LV)
Membagi ruang menjadi partisi-partisi kecil yang mempunyai tugas-tugas khusus di Linux. Jumlah partisi root yang kelompok kami miliki sebanyak 30G, kemudian kami membagi partisi root sesuai dengan yang ada dibawah ini
```
lvcreate -L 8G nebula -n root
```
```
lvcreate -L 8G nebula -n vars
```
```
lvcreate -L 1G nebula -n vlog
```
```
lvcreate -L 1G nebula -n vtmp
```
```
lvcreate -L 1G nebula -n vaud
```
```
lvcreate -L 5G nebula -n home
```
```
lvcreate -L 5G nebula -n podman
```

## Kemudian dilanjutkan dengan Format Partisi
memformat partisi untuk boot dengan menggunakan command

```
mkfs.vfat -F32 -n BOOT /dev/mmcblk0p5
```

```
mkfs.ext4 /dev/nebula/root
```

```
mkfs.ext4 /dev/nebula/vars
```

```
mkfs.ext4 /dev/nebula/vlog
```

```
mkfs.ext4 /dev/nebula/vtmp
```

```
mkfs.ext4 /dev/nebula/vaud
```

```
mkfs.ext4 /dev/nebula/home
```

```
mkfs.ext4 /dev/nebula/podman
```

## Lalu kami melakukan pemeriksaan kembali 

```
lsblk
```


## mounting harus dilakukan secara berurutan
menghubungkan partisi ke direktori tertentu dengan menggunakan command

```
mount /dev/nebula/root /mnt        
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/mmcblk0p5 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/nebula/vars /mnt/var
mount --mkdir -o rw,nosuid,noexec,relatime /dev/nebula/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/nebula/vtmp /mnt/var/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/nebula/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/nebula/home /mnt/home 
mount --mkdir -o rw,nosuid,noexec,relatime /dev/nebula/podman /mnt/var/lib/containers
```

## Setelah kami melakukan mounting, kami periksa kembali

```
lsblk
```

## Menginstall linux package
Menginstal paket dasar Arch Linux ke partisi yang telah di-mount dengan menggunakan command

```
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

## Kemudian kami melakukan Fstab (Menyimpan partisi yang telah dibuat)
membuat file fstab yang berisi daftar partisi dengan menggunakan command

```
genfstab -U /mnt > /mnt/etc/fstab
```

## Melakukan penyalinan setingan jaringan
menyalin settingan internet supaya internet di sistem baru langsung otomatis jalan meniru settingan saat ini dengan menggunakan command

```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## Dilanjutkan kami menambahkan partisi tmpfs ke dalam partisi
menambahkan folder RAM sementara supaya kalau laptop dimatiin, file sampah disana otomatis kehapus dengan menggunakan command

```
echo "tmpfs /tmp tmpfs defaults, nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```

## Kemudian kami melakukan pemeriksaan kembali
memastikan tulisan di file catatan sudah benar dengan menggunakan command

```
cat /mnt/etc/fstab
```

## Setelah itu masuk ke system 
masuk ke sistem Arch Linux yang baru di install dengan menggunakan command

```
arch-chroot /mnt
```

## Melakukan sinkronisasi waktu
mengatur zona waktu sesuai wilayah yang dipilih dengan menggunakan command

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

## Selanjutnya kami mengatur bahasa dan lokalisasi
memilih bahasa yang ingin digunakan dengan menggunakan command

```
nvim /etc/locale.gen
```

**Setelah masuk ketik “I” kemudian cari dua (en_US) dengan menghapus tagar untuk mengatur bahasa dan lokalisasi**
Untuk kembali ke tampilan klik “esc” kemudian ketik :wq

## Melakukan generate lokasi
mengunci bahasa yang ingin digunakan sebagai bahasa utama dengan menggunakan command

```
locale-gen 
```

```
locale > /etc/locale.conf
```

```
nvim /etc/locale.conf
```

** Setelah masuk ketik “I” pada bagian paling atas "C" diubah menjadi en_US dan pada bagian paling bawah tambahkan en_US.UTF-8**

## Membuat root dan user, kalua kelompok kami menggunakan (force)
```
passwd 12345
```
useradd -m force
```

```
passwd force 12345
```

```
echo "force ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/force
```

## Mengatur parameter
membuat direktori baru dengan menggunakan command

```
mkdir /etc/cmdline.d
```

membuat file kosong dengan menggunakan command

```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```
menuliskan isi ke dalam file dengan menggunakan command

```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/mmcblk0p6=advan7 root=/dev/nebula/root" > /etc/cmdline.d/01-boot.conf
```

```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

## Selanjutnya mengatur mkinitcpio
mengeedit setelan kernel supaya Linux bisa membaca hardisk yang dienskripsi dan di LVM pas laptop pertama kali dipencet tombol powernya dengan menggunakan command

```
nvim /etc/mkinitcpio.conf
```

***cari bagian Hooks yang tidak memiliki tagar dan berada di atas kata “COMPRESSION”, kemudian cari tulisan block lalu berikan spasi sekali dan ketik "sd-encrypt" lalu berikan spasi sekali lagi dilanjutkan dengan mengetik "lvm2"***

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
 Foto

## Menginstall bootctl
menginstall sistem boot supaya bisa menjalankan sistem operasi dengan menggunakan command 

```
bootctl --path=/mnt/boot install 
```
mkinitcpio -P
```

## Mengaktifkan aplikasi yang ingin digunakan 
mengaktifkan suatu layanan agar otomatis berjalan dengan menggunakan command
```
systemctl enable systemd-networkd
systemctl enable systemd-resolved 
systemctl enable iwd 
```

## melepas mounting yang telah dibuat
melepas semua mount yang dilakukan secara aman dengan menggunakan command

```
exit
umount -R /mnt
```

### **STOP REKAMAN**

**CTRL+D**

**asciineme upload kel10.cast**

**setelah melakukan upload rekaman asciinema, lalu foto link yang ditampilkan**
nyalakan ulang laptop menuju tampilan baru Arch Linux dengan menggunakan command
```
reboot
```

## Selesai



