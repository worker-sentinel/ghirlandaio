# PANDUAN INSTALASI ARCH LINUX 

## Persiapan Sebelum Install

### Download ISO Arch Linux 

Langkah awal sebelum instalasi adalah download file ISO Arch Linux. ini nanti digunakan sebagai media installer. Buka website resmi [archlinux.org/download] nya terlebih dahulu lalu scroll ke bawah sampai menemukan daftar mirror, cari negara singapore, lalu pilih yang aktkn.sg 
 
![gambar negara iso](https://cdn.corenexis.com/files/c/7569511720.png)   


![gambar pilihan iso](https://cdn.corenexis.com/files/c/8442542720.png) 

Setelah itu pilih file ISO yang paling atas.

### Membuat USB Bootable dengan Rufus 
Setelah ISO berhasil didownload, langkah berikutnya adalah membuat flashdisk bootable menggunakan aplikasi Rufus. Pertama download Rufus dengan mengetik “rufus download” pada browser, kemudian pilih versi yang bertipe Standard. Setelah selesai didownload, buka aplikasi Rufus lalu colokkan flashdisk ke laptop. 

![download rufus](https://cdn.corenexis.com/files/c/7933633720.png) 

Pada menu Device, pilih flashdisk yang digunakan. Kemudian pada menu Boot Selection, masukkan file ISO Arch Linux yang sudah didownload tadi. Setelah itu pada bagian Partition Scheme, pilih GPT (sesuai dengan laptop masing-masing). 

![pembuatan bootable](https://cdn.corenexis.com/files/c/3923994720.jpg) 
Setelah semua sudah sesuai, klik tombol Start untuk memulai proses pembuatan bootable. Jika muncul pilihan mode penulisan, pilih opsi Write in DD Image mode, lalu tunggu hingga proses selesai. Setelah selesai, flashdisk tersebut sudah bisa digunakan sebagai installer Arch Linux. 


### Cek Disk Management di Windows 
Tahap selanjut nya adalah shrink untuk memisahkan partisi atau ruang penyimpanan, caranya klik kanan pada logo windows lalu pilih Disk Management, nanti akan muncul tampilan seperti ini 

![cara membuka shrink](https://cdn.corenexis.com/files/c/5798744720.jpg) 

Selanjutnya klik kanan pada bagian os (C:) akan muncul tampilan shrink volume dan klik shrink volume. Setelah di klik akan muncul tampilan seperti ini:

![membuat shrink](https://cdn.corenexis.com/files/c/1411323720.jpg) 
 




### Booting ke Installer Arch Linux 
langkah berikutnya adalah melakukan booting menggunakan flashdisk tersebut. Laptop direstart kemudian masuk ke BIOS atau Boot Menu dengan menekan tombol tertentu seperti F2, F12, ESC, atau DEL (tergantung jenis laptop). Setelah masuk ke Boot Menu, pilih USB sebagai prioritas booting. Setelah itu akan muncul menu Arch Linux dan pilih opsi Arch Linux install medium. Jika berhasil, laptop akan masuk ke mode live system dan menampilkan terminal dengan tampilan seperti root@archiso.
### Memeriksa Mode Boot (UEFI atau Legacy) 
Setelah masuk ke terminal live Arch Linux, langkah selanjutnya adalah memeriksa apakah laptop menggunakan mode boot UEFI atau Legacy. Pemeriksaan ini dilakukan dengan mengetik perintah: 
`cat /sys/firmware/efi/fw_platform_size` 

![periksamodeboot](https://cdn.corenexis.com/files/c/6821985720.jpg) 


Jika hasilnya: 
64 berarti UEFI 64-bit
32 berarti UEFI 32-bit 
Jika error berarti BIOS/Legacy mode 

### Menyambungkan Internet Menggunakan WIFI
Karena instalasi Arch Linux membutuhkan paket yang diunduh dari repository online, maka koneksi internet harus dipastikan aktif terlebih dahulu. 

Masuk ke mode WIFI 
`iwctl` 

Setelah masuk, cari wifi yang tersedia
`device list`
`station wlan0 get-networks`

Untuk connect ke WIFI 
`station wlan0 connect namawifi`

Setelah itu masukan password WIFI 

Untuk mengecek berhasil atau tidak
`ping ping.archlinux.org` 

![wifi](https://cdn.corenexis.com/files/c/4925447720.jpg) 


Jika berhasil dan muncul respon dengan satuan ms, maka internet sudah terhubung. 

Setelah itu keluar dari iwctl
`Exit` 

### Sinkronisasi Waktu
Setelah memastikan internet berjalan, selanjutnya adalah melakukan sinkronisasi waktu otomatis supaya sistem tidak terjadi error.
`timedatectl`
Dengan menjalankan perintah ini sistem akan memastikan waktu dan tanggal sudah sesuai.

### Mengecek Disk yang Digunakan 
Langkah selanjutnya adalah mengecek disk yang akan digunakan untuk instalasi Arch Linux.  biasanya seperti `/dev/sda` atau `/dev/nvme0n1`. Untuk melihat disk ini biasanya menggunakan `lsblk` atau `fdisk -l`

`slblk` 

 ![slblk](https://cdn.corenexis.com/files/c/2759597720.jpg) 



Dari hasil pengecekan tersebut terlihat disk utama yang digunakan adalah /dev/nvme0n1 

### Partisi Disk 
Setelah mengetahui disk yang digunakan, langkah berikutnya adalah melakukan partisi menggunakan cfdisk. 

`cfdisk /dev/nvme0n1`

Struktur partisi yang disarankan: 
EFI = boot
Swap = cadangan RAM
Root = tempat utama instalasi sistem 

Bisa dilihat struktur dibawah ini 

![partisi](https://cdn.corenexis.com/files/c/8667549720.jpg)



![partisilagi](https://cdn.corenexis.com/files/c/7289618720.jpg) 


Di dalam cfdisk, partisi dibuat pada ruang kosong (unallocated) yang sebelumnya sudah disiapkan dari Windows melalui shrink volume. Partisi yang dibuat terdiri dari partisi EFI (boot), swap, dan root. Didapatkan partisi EFI berada di /dev/nvme0n1p6, partisi swap berada di /dev/nvme0n1p7, dan partisi root berada di /dev/nvme0n1p8. Nomor partisi yang tidak berurutan ini terjadi karena pada SSD sudah terdapat beberapa partisi bawaan Windows seperti recovery atau system reserved, sehingga partisi Linux dibuat pada nomor yang tersedia berikutnya. 

### Format partisi Root, Swap dan EFI 

Setelah partisi selesai dibuat, langkah berikutnya adalah memformat partisi sesuai fungsi masing-masing. Untuk partisi root digunakan filesystem ext4 karena umum dipakai di Linux dan cukup stabil. 

**Root**
`mkfs.ext4 /dev/nvme0n1p8`

**Swap**
`mkswap /dev/nvme0n1p7`
`Swapon /dev/swap_partition` 

**EFI**
`mkfs.fat -F 32 /dev/nvme01p6`

Penjelasan 
EFI partition harus menggunakan FAT32

### Mount Filesystem 
Setelah semua partisi diformat, langkah selanjutnya adalah melakukan mount agar sistem tahu lokasi instalasi. Partisi root dipasang ke direktori /mnt 
 
**Mount root**
`mount /dev/[root_partition] /mnt` 

Setelah itu partisi EFI dipasang ke /mnt/boot 

**Mount EFI** 
`mount --mkdir /dev/[efi_system_partition] /mnt/boot` 

### Instalasi Sistem Dasar 
Setelah mount kita masuk ke tahap instalasi sistem dasar yaitu 
` pacstrap -K /mnt base linux linux-firmware base base-devel neovim`

penjelasan : 
base → paket inti sistem 
linux → kernel Linux
linux-firmware → firmware hardware 
### Membuat fstab
Setelah sistem dasar sudah terinstal, selanjutnya adalah membuat fstab untuk menentukan partisi mana yang otomatis di mount saat boot
`genfstab -U /mnt >> /mnt/etc/fstab`
![membuat fstab](https://cdn.corenexis.com/files/c/6254346720.jpg)  
Setelah perintah dijalankan, konfigurasi mount partisi akan tersimpan di dalam sistem yang baru diinstal.
### Masuk ke Sistem Baru (Chroot)
Setelah fstab dibuat, langkah selanjutnya adalah masuk ke sistem Arch Linux yang baru dipasang menggunakan chroot. ini digunakan untuk mengubah root shell ke sistem Arch yang baru dipasang 
`arch-chroot /mnt`


### Mengatur Timezone (set waktu)
`ln -sf /usr/share/zoneinfo/[Area]/[Location] /etc/localtime`
### Sinkronkan hardware clock: 
`hwclock --systohc`

### Localization 
Generate local: locale-gen
`nvim /etc/locale.conf: LANG=en_US.UTF-8`
Penjelasan 
digunakan untuk memproses data bahasa yang dipilih 
### Hostname 
Hostname merupakan nama komputer `nvim /etc/hostname`
Klik i masukan nama komputer, klik esc klik :wq 
### User
untuk menambahkan user command : `useradd -m -G wheel -s /bin/bash [nama user]` usahan menggunakan huruf kecil dan hanya satu kata, lalu masukan password command `passwd ` [nama user] lalu unntuk mengecek berhasil atau tidak untuk user command `su [nama user]` kalau sudah exit lalu buka pengaturan hak akses command nya `EDITOR=nvim visudo` lalu ketik `/` (untuk mencari kata) `% wheel ALL=(ALL:ALL) ALL` lalu klik `i` (untuk mengubah isi file) hapus tanda # yang ada di sebelah katayang dicari, lalu klik esc lalu ketik :wq untuk keluar  
### Generate Initramfs 
`mkinitcpio -P`
Penjelasan 
Membuat image boot awal linux 
### Pasang Password root
`passwd`
Penjelasan 
Mengatur password root
### Install Bootloader 
Pilihan umum: 
GRUB 
systemd-boot
rEFInd
Install grub 
![install GRUB](https://cdn.corenexis.com/files/c/5464744720.jpg) 
`Pacman -S grub efibootmgr dosfstools os-prober mtools`

Proses instalasi bootloader ke (sistem/boot) c
`grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB`

Membuat file konfigurasi grup 
`grub-mkconfig -o /boot/grub/grub.cfg`
### Reboot 
Tahap terakhir adalah keluar dari chroot 
`Exit`
Setelah itu semua partisi yang tadi di-mount dilepas (unmount)
`umount -R /mnt`
Setelah semuanya dilepas, sistem direstart 
Reboot 
Saat laptop mulai restart, flashdisk installer dicabut agar sistem tidak kembali boot dari USB.
