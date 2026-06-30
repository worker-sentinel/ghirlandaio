# Connect Wifi
```
iwctl
```
Membuka menu untuk mengatur koneksi WiFi.

```
station wlan0 get-networks
```
Melihat daftar WiFi.

```
station wlan0 scan
```
Mencari jaringan WiFi yang ada di sekitar.

```
station wlan0 connect (nama jaringan)
```
Menghubungkan laptop ke WiFi.

```
exit
```
Keluar dari _iwctl_

```
ping 8.8.8.8
```
Mengecek apakah internet sudah tersambung.

# Partisi
## Melihat Partisi
```
lsblk
```
Melihat semua partisi yang ada.

## Membagi Partisi
```
cfdisk /dev/(nama partisi masing masing bisa sda atau nvme0n1p)
```
Membuka menu untuk membuat atau mengatur partisi, dan sesuaikan dengan penyimpanan yang ada

```
boot= 3G
root= 56G
```

```
lsblk
```
Melihat kembali hasil pembagian partisi yang telah dibuat

# Setup Luks
```
cryptsetup luksFormat /dev/(partisi root)
```
Memberikan enkripsi pada partisi. agar data di dalam partisi tersebut lebih aman.

```
cryptsetup luksOpen /dev/(partisi root) proc (nama device - boleh apa aja)
```
Umtuk membuka partisi yang sudah dienkripsi dan supaya partisi bisa digunakan untuk proses instalasi.

```
pvcreate /dev/mapper/proc
```
Membuat Physical Volume.

```
vgcreate slims /dev/mapper/proc
```
Membuat Volume Group bernama slims.

# Setup LVM
```
lvcreate -L 15G slims -n root 
```
Membuat Logical Volume bernama root dan digunakan untuk menyimpan sistem operasi Linux.
>lvcreate: untuk membuat logical volume

>-L: Untuk menentukan ukuran kapasitas penyimpanan yang akan dibuat pada logical volume baru yang akan dibuat

>15G: Besaran partisi yang dibuat 

>Slims: Nama volume grup

>-n: Untuk menentukan nama logical volume baru yang dibuat

>root: Partisi yang ingin dibuat

```
lvcreate -L 10G slims -n vars 
```
Digunakan untuk menyimpan data aplikasi dan sistem.
>lvcreate: untuk membuat logical volume

>-L: Untuk menentukan ukuran kapasitas penyimpanan yang akan dibuat pada logical volume baru yang akan dibuat

>10G: Besaran partisi yang dibuat 

>Slims: Nama volume grup

>-n: Untuk menentukan nama logical volume baru yang dibuat

>vars: Partisi yang ingin dibuat

```
lvcreate -L 1G slims -n vlog
```
Digunakan untuk menyimpan log atau catatan aktivitas sistem.
>lvcreate: untuk membuat logical volume

>-L: Untuk menentukan ukuran kapasitas penyimpanan yang akan dibuat pada logical volume baru yang akan dibuat

>1G: Besaran partisi yang dibuat 

>Slims: Nama volume grup

>-n: Untuk menentukan nama logical volume baru yang dibuat

>vlog: Partisi yang ingin dibuat

```
lvcreate -L 1G slims -n vaud
```
Digunakan untuk menyimpan log audit atau keamanan sistem.
>lvcreate: untuk membuat logical volume

>-L: Untuk menentukan ukuran kapasitas penyimpanan yang akan dibuat pada logical volume baru yang akan dibuat

>1G: Besaran partisi yang dibuat 

>Slims: Nama volume grup

>-n: Untuk menentukan nama logical volume baru yang dibuat

>vaud: Partisi yang ingin dibuat

```
lvcreate -L 1G slims -n vtmp
```
Untuk menyimpan file sementara.
>lvcreate: untuk membuat logical volume

>-L: Untuk menentukan ukuran kapasitas penyimpanan yang akan dibuat pada logical volume baru yang akan dibuat

>1G: Besaran partisi yang dibuat 

>Slims: Nama volume grup

>-n: Untuk menentukan nama logical volume baru yang dibuat

>vtmp: Partisi yang ingin dibuat

```
lvcreate -L 8G slims -n home
```
Untuk menyimpan data pengguna.
>lvcreate: untuk membuat logical volume

>-L: Untuk menentukan ukuran kapasitas penyimpanan yang akan dibuat pada logical volume baru yang akan dibuat

>8G: Besaran partisi yang dibuat 

>Slims: Nama volume grup

>-n: Untuk menentukan nama logical volume baru yang dibuat

>home: Partisi yang ingin dibuat

```
lvcreate -l70%FREE slims -n podman 
```
Menggunakan sisa ruang penyimpanan untuk podman yang digunakan untuk menyimpan data container Podman.
>lvcreate: untuk membuat logical volume

>-l70%FREE: Untuk mengalokasikan ukuran sebesar 70% dari total sisa ruang kosong yang masih tersedia didalam volume group

>Slims: Nama volume grup

>-n: Untuk menentukan nama logical volume baru yang dibuat

>podman: Partisi yang ingin dibuat

```
lsblk
```
Melihat hasil pembuatan Logical Volume.

# Formating LV
```
mkfs.vfat -F32 -n BOOT /dev/(partisi boot)
```
Untuk memformat partisi boot supaya bisa digunakan sebagai partisi boot UEFI.
>mkfs.vfat: singkatan dari "Make File System vfat" untuk memformat partisi menjadi sistem file vfat

>-F32: Menentukan jenis alokasi file yang akan dibuat menjadi FAT32

>-n: Untuk menentukan nama logical volume baru yang dibuat

>BOOT: Membuat nama partisi yang akan diformat

>/dev/(partisi boot): Lokasi partisi yang ingin diformat 

```
mkfs.ext4 /dev/slims/root
```
Memformat volume root supaya bisa digunakan untuk menginstal Linux.
>mkfs.ext4: singkatan dari "Make File System ext4" untuk memformat partisi menjadi sistem file ext4 

>/dev/slims/root: Lokasi partisi root yang ingin diformat

```
mkfs.ext4 /dev/slims/vars
```
Menyiapkan volume untuk menyimpan data sistem.
>mkfs.ext4: singkatan dari "Make File System ext4" untuk memformat partisi menjadi sistem file ext4

>/dev/slims/vars: Lokasi partisi vars yang ingin diformat

```
mkfs.ext4 /dev/slims/vtmp
```
Menyiapkan tempat penyimpanan file sementara.
>mkfs.ext4: singkatan dari "Make File System ext4" untuk memformat partisi menjadi sistem file ext4

>/dev/slims/vars: Lokasi partisi vtmp yang ingin diformat

```
mkfs.ext4 /dev/slims/vlog
```
Menyiapkan tempat penyimpanan log sistem.
>mkfs.ext4: singkatan dari "Make File System ext4" untuk memformat partisi menjadi sistem file ext4

>/dev/slims/vlog: Lokasi partisi vlog yang ingin diformat

```
mkfs.ext4 /dev/slims/vaud
```
Menyiapkan tempat penyimpanan log audit.
>mkfs.ext4: singkatan dari "Make File System ext4" untuk memformat partisi menjadi sistem file ext4

>/dev/slims/vaud: Lokasi partisi vaud yang ingin diformat

```
mkfs.ext4 /dev/slims/home
```
Menyiapkan tempat penyimpanan data pengguna.
>mkfs.ext4: singkatan dari "Make File System ext4" untuk memformat partisi menjadi sistem file ext4

>/dev/slims/home: Lokasi partisi home yang ingin diformat

```
mkfs.ext4 /dev/slims/podman
```
Menyiapkan tempat penyimpanan data Podman.
>mkfs.ext4: singkatan dari "Make File System ext4" untuk memformat partisi menjadi sistem file ext4

>/dev/slims/podman: Lokasi partisi podman yang ingin diformat

```
lsblk
```
Mengecek hasil format.

# Mounting
```
mount /dev/slims/root /mnt
```
Memasang partisi root, sebagai tempat instalasi Linux.
>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>/dev/slims/root: Lokasi partisi root yang akan dimounting

>/mnt: Folder bawaan standar saat install arch linux yang digunakan sebagai pintu masuk sementara

```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/(partisi boot) /mnt/boot
```
Memasang partisi boot. Untuk tempat menyimpan file boot.
>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>--mkdir: Untuk otomatis membuat folder jika folder tersebut belum ada

>-o: Singkatan dari "options" untuk menyisipkan pengaturan atau parameter keamanan khusus tambahan pada partisi

>uid=0: Menentukan pemilik seluruh file boot adalah user root atau superuser

>gid=0: Menentukan kepemilikan file-file yang ada didalam partisi tersebut adalah root 

>fmask=0077: Mengatur hak akses khusus untuk file biasa

>dmask=0077: Mengatur hak akses khusus untuk folder atau direktori dalam partisi boot

>/dev/(partisi boot): Lokasi partisi boot yang akan dimounting

>/mnt/boot: Folder target tempat partisi boot berada

```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/slims/vars /mnt/var         
```
Untuk menyimpan data sistem.
>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>--mkdir: Untuk otomatis membuat folder jika folder tersebut belum ada

>-o: Singkatan dari "options" untuk menyisipkan pengaturan atau parameter keamanan khusus tambahan pada partisi

>rw: Akses edit file dan untuk membaca file 

>nodev: Mencegah pembuatan replika perangkat keras (misal flashdisk berisikan virus)

>nosuid: Pengguna biasa tidak bisa menjalankan program dengan akses root atau sudo

>relatime: Jika habis update, relatime akan membuat update file tersebut menjadi last update sehingga mempercepat performa arch linux

>/dev/slims/vars: Lokasi partisi vars yang akan dimounting

>/mnt/var: Folder target tempat partisi var berada

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/slims/vtmp /mnt/var/tmp     
```
Tempat untuk file sementara.
>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>--mkdir: Untuk otomatis membuat folder jika folder tersebut belum ada

>-o: Singkatan dari "options" untuk menyisipkan pengaturan atau parameter keamanan khusus tambahan pada partisi

>rw: Akses edit file dan untuk membaca file 

>nodev: Mencegah pembuatan replika perangkat keras (misal flashdisk berisikan virus)

>nosuid: Pengguna biasa tidak bisa menjalankan program dengan akses root atau sudo

>noexec: Memblokir eksekusi virus atau maleware

>relatime: Jika habis update, relatime akan membuat update file tersebut menjadi last update sehingga mempercepat performa arch linux

>/dev/slims/vtmp: Lokasi partisi vtmp yang akan dimounting

>/mnt/var/tmp: Folder target tempat partisi tmp berada

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/slims/vlog /mnt/var/log  
```
Untuk menyimpan log sistem.

>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>--mkdir: Untuk otomatis membuat folder jika folder tersebut belum ada

>-o: Singkatan dari "options" untuk menyisipkan pengaturan atau parameter keamanan khusus tambahan pada partisi

>rw: Akses edit file dan untuk membaca file 

>nodev: Mencegah pembuatan replika perangkat keras (misal flashdisk berisikan virus)

>nosuid: Pengguna biasa tidak bisa menjalankan program dengan akses root atau sudo

>noexec: Memblokir eksekusi virus atau maleware

>relatime: Jika habis update, relatime akan membuat update file tersebut menjadi last update sehingga mempercepat performa arch linux

>/dev/slims/vlog: Lokasi partisi vlog yang akan dimounting

>/mnt/var/log: Folder target tempat partisi log berada

```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/slims/vaud /mnt/var/log/audit      
```
Untuk menyimpan log audit.
>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>--mkdir: Untuk otomatis membuat folder jika folder tersebut belum ada

>-o: Singkatan dari "options" untuk menyisipkan pengaturan atau parameter keamanan khusus tambahan pada partisi

>rw: Akses edit file dan untuk membaca file 

>nodev: Mencegah pembuatan replika perangkat keras (misal flashdisk berisikan virus)

>nosuid: Pengguna biasa tidak bisa menjalankan program dengan akses root atau sudo

>noexec: Memblokir eksekusi virus atau maleware

>relatime: Jika habis update, relatime akan membuat update file tersebut menjadi last update sehingga mempercepat performa arch linux

>/dev/slims/vaud: Lokasi partisi vaud yang akan dimounting

>/mnt/var/log/audit: Folder target tempat partisi audit berada

```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/slims/podman /mnt/var/lib/container
```
Untuk menyimpan data container.
>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>--mkdir: Untuk otomatis membuat folder jika folder tersebut belum ada

>-o: Singkatan dari "options" untuk menyisipkan pengaturan atau parameter keamanan khusus tambahan pada partisi

>rw: Akses edit file dan untuk membaca file 

>nodev: Mencegah pembuatan replika perangkat keras (misal flashdisk berisikan virus)

>nosuid: Pengguna biasa tidak bisa menjalankan program dengan akses root atau sudo

>relatime: Jika habis update, relatime akan membuat update file tersebut menjadi last update sehingga mempercepat performa arch linux

>/dev/slims/podman: Lokasi partisi podman yang akan dimounting

>/mnt/var/lib/container: Folder target tempat partisi container berada

```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/slims/home /mnt/home
```
Untuk menyimpan data pengguna.
>mount: Mengaktifkan sistem file atau partisi ke dalam sebuah direktori agar bisa diakses lewat file manager atau terminal

>--mkdir: Untuk otomatis membuat folder jika folder tersebut belum ada

>-o: Singkatan dari "options" untuk menyisipkan pengaturan atau parameter keamanan khusus tambahan pada partisi

>rw: Akses edit file dan untuk membaca file 

>nodev: Mencegah pembuatan replika perangkat keras (misal flashdisk berisikan virus)

>nosuid: Pengguna biasa tidak bisa menjalankan program dengan akses root atau sudo

>relatime: Jika habis update, relatime akan membuat update file tersebut menjadi last update sehingga mempercepat performa arch linux

>/dev/slims/home: Lokasi partisi home yang akan dimounting

>/mnt/home: Folder target tempat partisi home berada

```
lsblk
```
Memastikan semua partisi sudah berhasil di-mount.

# Install Packages
```
lspci
```
>Untuk melihat jenis hardware yang digunakan

AMD
```
pacstrap /mnt amd-ucode base pacman sudo linux-lts linux-lts-headers lvm2 mkinitcpio linux-firmware-intel podman neovim git networkmanager asciinema linux-firmware-realtek firewalld
```
Menginstal sistem operasi beserta paket penting seperti kernel, firmware, NetworkManager, Podman, Git, Neovim, Firewalld, dan lain-lain.

# Fstab
```
genfstab -U /mnt > /mnt/etc/fstab
```
Membuatu file fstab, agar semua partisi otomatis terpasang setiap kali Linux dinyalakan.

# Format tmpfs ke tmp
```
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```
Mengatur tmpfs ke fstab, supaya folder _/tmp_ menggunakan RAM sehingga lebih cepat dan otomatis bersih setelah komputer dimatikan.

# Chroot
```
arch-chroot /mnt
```
Masuk ke sistem Linux yang baru diinstal, untuk melakukan konfigurasi sistem sebelum digunakan.

# Membuat Hostname
```
echo "cosmic-crew" > /etc/hostname
```
Memberikan nama pada device.

# Mengatur Waktu Dan Bahasa
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
Mengatur zona waktu ke Asia/Jakarta.

```
hwclock --systohc
```
Menyamakan jam device dengan waktu sistem.

```
nvim /etc/locale.gen
```
Membuka file pengaturan bahasa.
>Hapus tanda pagar pada en_US.UTF-8 UTF-8 dan en_US ISO-8859-1

```
locale-gen
```
Membuat pengaturan bahasa agar bisa digunakan.

```
locale > /etc/locale.conf
```
Menyimpan pengaturan bahasa.

```
nvim /etc/locale.conf
```
Mengatur bahasa utama yang digunakan sistem.
>Pada LANG=en_US.UTF-8 ubah menjadi LANG=en_US.UTF-8 kemudian dibagian LC_ALL= tambahkan juga en_US.UTF-8

# Membuat User
## Membuat Home Directory
```
mkdir -p /home/user
```
Membuat folder home untuk pengguna.

## Menambahkan User
Menambahkan user 'kel10' dengan home directory
```
useradd -d /home/user kel10
```
Menambahkan user baru bernama kel10.

## Mengatur Password User
```
passwd
```
Membuat password untuk akun root.

## Mengatur Hak Kepemilikan Home Directory
```
chown -R kel10:kel10 /home/user
```
Memberikan hak akses folder home kepada user.

## Mengatur Password User
```
passwd kel10
```
Membuat password untuk user kel10.

## Konfigurasi Sudo
```
echo 'kel10 ALL=(ALL:ALL) ALL' >> /etc/sudoers.d/kel10
```
Memberikan hak sudo kepada user.

```
cat /etc/sudoers.d/kel10
```
Mengecek apakah hak sudo sudah berhasil ditambahkan.

# Kernel Parameter
```
mkdir /etc/cmdline.d
```
Membuat folder konfigurasi kernel.

```
touch /etc/cmdline.d/{01-boot.conf,06-misc.conf}
```
Membuat file konfigurasi kernel.

```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/p.root)=proc root=/dev/slims/root" > /etc/cmdline.d/01-boot.conf
```
Memberi tahu kernel lokasi partisi yang terenkripsi saat proses boot.

```
echo "rw" > /etc/cmdline.d/06-misc.conf
```
Mengatur agar sistem bisa membaca dan menulis data setelah boot.

# mkinitcpio
```
cd /boot
```
Masuk ke folder boo

```
mkdir kernel efi
```
Membuat folder kernel dan efi.

```
cd efi
```

```
mkdir linux
```
Membuat folder Linux di dalam EFI.

```
cd ..
```

```
mv vmlinuz-linux-lts amd-ucode.img kernel
```
Memindahkan file kernel ke folder kernel.

```
rm -fr inittramfs-linux-lts.img
```
Menghapus file initramfs lama.

```
mv /etc/mkinitcpio.conf /etc/mkinitcpio.d/default.conf
```
Memindahkan file konfigurasi.

```
nvim /etc/mkinitcpio.d/default.conf
```
Mengedit konfigurasi mkinitcpio.
>Pada hooks paling bawah, tambahkan lvm2 sd-encrypt setelah sd-vconsole

```
nvim /etc/mkinitcpio.d/linux-lts.preset
```
Mengatur lokasi kernel dan initramfs.
>Hapus semua pagar pada baris ALL, kemudian ubah baris pertama ALL jadi "/etc/mkinitcpio.d/default.conf", untuk dua baris setelahnya tambahkan 'kernel' setelah 'boot'.

>Tambahkan pagar pada baris kedua default. Selanjutnya, hapus pagar pada baris ketiga default, kemudian hapus 'EFI' dan ubah 'Linux' jadi 'linux'

```
mkinitcpio -P 18L, 639B written
```

```
bootctl --path=/boot install
```
Menginstal bootloader agar komputer bisa menjalankan Arch Linux saat dinyalakan.

```
systemctl enable NetworkManager
```
Mengaktifkan NetworkManager agar internet otomatis aktif saat boot.

```
systemctl enable systemd-networkd.socket
```
Mengaktifkan layanan jaringan systemd.

```
systemctl enable systemd-resolved
```
Mengaktifkan layanan DNS.

# Selesai
```
exit
```
Keluar dari mode _arch-root_

```
umount -R /mnt
```
Melepas semua partisi yang sebelumnya di-mount.

```
lsblk
```
Mengecek kembali kondisi partisi.

```
reboot
```
Merestart komputer agar masuk ke sistem Arch Linux yang baru selesai diinstal.
