# Panduan Instalasi Arch Linux 

Panduan ini disusun untuk membantu melewati setiap langkah proses instalasi Arch Linux dengan mudah dan terstruktur menggunakan live system dari media resmi. 

Panduan ini juga dibuat sebagai bentuk dokumentasi dari tugas mata kuliah Perpustakaan dan Arsip Digital.

## Cara Download Arch Iso

Langkah pertama yang perlu dilakukan adalah mengunduh Archiso.

Buka situs __[archlinux.org/download](https://archlinux.org/download/)__, lalu scroll ke bawah hingga menemukan bagian Indonesia dan pilih __[citrahost.com](https://mirror.citrahost.com/archlinux/iso/2026.05.01/)__ sebagai mirror unduhan.

---

<img width="1600" height="1200" alt="WhatsApp Image 2026-05-15 at 20 00 43" src="https://github.com/user-attachments/assets/88246516-ca2e-44e4-af35-25c4be2e60d7" />

---

Lalu, klik yang muncul ke dua dalam baris, yaitu __archlinux-2026.05.01-x86_64.iso__

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/c8afb423-76b7-486c-a2d1-db897269d7cb" />

---

Setelah file Arch Iso berhasil diunduh, langkah berikutnya adalah mengunduh __Rufus__. ketik __[rufus.ie/id/](https://rufus.ie/id/)__ untuk mengakses situs Rufus berbahasa indonesia, lalu klik unduh untuk melakaukan pengunduhan.

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/a51a3c48-7fce-4c9d-9a00-149805dbf073" />

---

Sesuaikan dengan PC atau Laptop kalian. Untuk kelompok kami menggunakan versi yang ada di paling atas, __rufus-4.14.exe__ untuk platform Windows x64. 

---

https://github.com/user-attachments/assets/04ccc2bb-7ae9-4079-8077-a58033916058

---

## Memulai

Tahap awal dimulai dengan memasukkan flashdisk ke laptop, kemudian klik tombol __Select__ dan pilih file Archiso yang sudah diunduh sebelumnya. Untuk bagian __Partition Scheme__, laptop keluaran baru umumnya sudah menggunakan __GPT__.  

__Diperlukan:__
- **USB Drive:** Minimal kapasitas 2GB (direkomendasikan 4GB atau lebih).

__Perhatian!__
- Proses ini akan menghapus **SELURUH DATA** di dalam USB drive.

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/c6cdf2fa-bfdc-4ba4-9240-5993030daca3" />

---

Setelah mengklik start akan muncul tampilan seperti ini, klik bawah yaitu __Write in DD Image Mode__. 

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/7f5258eb-f462-4b5b-a00c-c5013b2595c8" />

---

Setelah muncul seperti, lanjut klik __OK__ untuk menaruh Archiso. (Gunakan flashdisk yang dibutuhkan, disarankan flashdisk lama yang sudah tidak terpakai lagi atau bisa juga menggunakan flashdisk baru)


Jika sudah selesai proses sebelumnya, klik __close__.


Untuk mengecek partisi, buka __Disk Management__ dengan menekan tombol __Windows + X__ atau mengklik kanan mouse pada ikon Windows di taskbar, Lalu pilih opsi tersebut dari menu yang muncul. 


Selanjutnya, Periksa berapa banyak ruang partisi yang tersedia atau masih kosong.


Setelah itu, pilih partisi dengan kapasitas terbesar, lalu klik kanan dan pilih opsi __Shrink Volume__

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/ab5ee5bd-a7f4-4d55-bd09-ee20e37da779" />

---

Jika ruang yang tersedia masih cukup besar, disarankan memasukkan nilai 50000 (setara dengan 50GB) pada kolom yang tersedia, lalu klik __Shrink__.

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/938ada5e-6fac-49fb-9f66-4087ac2fa757" />

---

Kurang lebih seperti inilah tampilan setelah partisi berhasil dibuat. Namun, kita akan melanjutkan proses pembuatan partisi lebih lanjut di dalam Arch Linux.


Selanjutnya, Masuk ke __Entering Setup__ (cara masuk nya dapat dicari di Google sesuai tipe dan merk laptop masing-masing, sebagai contoh untuk modul dokumentasi ini kelompok kami menggunakan laptop Lenovo). Akses BIOS dengan menekan __F1__ saat logo Lenovo muncul, kemudian buka tab Security -> Secure Boot dan ubah statusnya menjadi __Disabled__.  Simpan perubahan dengan menekan __F10__, dan perangkat akan otomatis melakukan reboot.


Setelah itu, masuk ke Boot Menu dengan menekan __F12__ (khusus untuk pengguna Lenovo Thinkpad, selain itu dapat mencari lebih lanjut di google untuk cara Boot Menu di berbagai tipe dan merk laptop berbeda) pastikan flashdisk sudah tercolok dan telah berisi file Archiso sebelumnya.

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5a149ab2-6ed8-42c2-9fcb-051c2aa6c577" />

---

> Setelah itu akan muncul tampilan seperti ini, pencet pilihan USB

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/92555886-552a-4826-8e01-1026f12e737d" />

---

> Pencet enter di keyboard

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/94602b06-99fa-48d0-9708-41540148c7dc" />

---

> Tunggu sampai tampilan seperti ini lalu ketik:
```bash
iwctl
```

---

# Kondisi Khusus

jika daftar perangkat di __iwctl__ tidak menampilkan __wlan0__, __wlan1__, atau yang sejenisnya, dapat mengetik perintah:
```bash
rfkill list
```

untuk memeriksa status perangkat jaringan.

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/0e30f0bd-392d-434a-926d-051a9fdc5393" />

---

Contoh dari perintah tersebut akan terlihat seperti ini. Selanjutnya, ketikkan perintah: 
```bash
rfkill unblock wlan
```

Perintah ini digunakan untuk mengaktifkan perangkat WiFi. Jika ingin mengaktifkan Bluetooth, cukup ketikkan:
```bash
rfkill unblock bluetooth
```

Setelah itu dapat dilanjut dengan mengetik:
```bash
device list
```

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/25b6ef19-b6f1-49a0-8b40-269feecce651" />

---

Selanjutnya akan muncul tampilan __Station wlan(0)(menyesuaikan name di tabel)__

Setelah itu, sambungkan perangkat dengan koneksi Wi-Fi (Disarankan menggunakan Wi-Fi yang berkoneksi cepat dan stabil). Sambungkan dengan cara mengetik perintah:
```bash
station wlan0 connect (nama wifi)
```
__Contoh:__
- station wlan0 connect wifirumahh


Apabila nama WiFi mengandung spasi, pastikan untuk mengapit nama tersebut dengan tanda petik (") terlebih dahulu.


__Contoh:__
- station wlan0 connect "uin fah"

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/74161428-1d8f-429d-8d0d-db4d143604c7" />

---

Setelah mengetik perintah untuk menyambungkan ke koneksi WiFi, masukkan password WiFi yang akan disambungkan.


Lalu klik key __Enter__ di keyboard.


Jika sudah mengetik password dengan benar, exit terlebih dahulu dengan mengetik:
```bash
exit
```

lanjutkan dengan memeriksa koneksi jaringan dengan mengetikkan perintah: 
```bash
ping 8.8.8.8
```

---

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5090e3f0-8037-4aff-b1b7-a37ba61bf03c" />

---

Tampilan seperti ini menandakan perangkat berhasil terhubung ke koneksi WiFi. Namun, jika muncul pesan Network Unreachable, berarti koneksi gagal dan perlu dicoba kembali.


Setelah koneksi berhasil terhubung, hentikan proses ping dengan menekan __Ctrl + C__ untuk melanjutkan ke tahap berikutnya.

---

# Sinkronisasi Waktu

Untuk melakukan __Sinkronasi Waktu__, ketik perintah ini:
```bash
timedatectl
```

### Penjelasan
Pastikan jam sistem sudah disetel dengan benar, karena hal ini penting agar proses verifikasi SSL dan Package Signature tidak mengalami kegagalan.

Untuk melihat daftar partisi, gunakan perintah:
```bash
lsblk
```
Perintah ini akan menampilkan partisi yang sebelumnya telah disiapkan dari ruang 50GB tadi.

---

<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 38 07 PM (1)" src="https://github.com/user-attachments/assets/9f64b4ac-1a4c-4a78-b790-edc7dff007af" />

---

Selanjutnya, jalankan perintah:
```bash
cfdisk /dev/nvme0n1
```
untuk mengelola partisi.


Perlu diperhatikan bahwa nama perangkat bisa berbeda, misalnya __sda__, tergantung dari jenis __harddisk__ yang digunakan.

---

<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 48 16 PM" src="https://github.com/user-attachments/assets/457b1dc0-da8c-4a58-aa4e-d498013ccb72" />

---

Geser ke kiri untuk memilih opsi __New__, lalu masukkan ukuran partisi secara berurutan, yaitu __512M__ untuk partisi pertama, __4G__ untuk partisi kedua, dan __sisa ruang__ yang tersedia untuk partisi ketiga.

---

<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 51 02 PM" src="https://github.com/user-attachments/assets/ae5c3765-8289-44de-a272-af0462f88004" />

---

Gunakan __key__ anak panah kanan & kiri untuk __bernavigasi__ (bergerak ke kanan dan kiri pada tampilan terminal)

---

<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 51 15 PM" src="https://github.com/user-attachments/assets/74a4787d-52d2-4301-b262-0bc1393d101d" />

---

Masukkan ukuran 512M untuk partisi __EFI__, 4G untuk partisi __Swap__, dan sisa ruang yang tersedia untuk partisi __Root__. 

__Untuk Contoh:__
-  jika total ruang adalah 50GB dan setelah dipartisi tersisa 44,3G, maka masukkan nilai 44,3G untuk partisi __Root__.

---
<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 53 02 PM" src="https://github.com/user-attachments/assets/d06d0474-f66a-4787-8d0b-6ac699c4293f" />

---

Sebelum memilih opsi Write, pastikan setiap partisi sudah diatur tipe nya dengan benar, yaitu:
- 512M -> EFI System (untuk partisi Boot)
- 4G -> Linux Swap (untuk partisi Swap)
- Sisa Ruang ->  Linux Filesystem (untuk partisi Root)

__Sebagai Contoh:__
- jika total ruang bebas adalah 50G dan sudah terpakai 5,7G untuk __EFI__ dan __Swap__, maka sisa ruang untuk __Root__ adalah sekitar 44,3G.

---

<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 53 54 PM" src="https://github.com/user-attachments/assets/c077b472-9fd4-44a1-8d90-2579adcb9264" />

---

Setelah melakukan __write__, geser menggunakan key anak panah ke opsi quit, kemudian enter.

Struktur Partisi yang Disarankan:

UEFI

| Mount Point | Fungsi |
|---|---|
| `/boot` | EFI Partition |
| `swap` | Virtual memory |
| `/` | Root system |

---

Selanjutnya, masuk ke tahap __Formatting__ yang meliputi tiga proses utama, yaitu format partisi __Root__, mengaktifkan __Swap__, dan format partisi __EFI__.

Catatan : 
- Partisi harus sesuai dengan type yang sudah di write tadi. jika sampai salah, dapat menyebabkan data di partisi hilang karena datanya telah tertimpa (salah satu nya Kehilangan OS). Untuk memastikan kembali partisi untuk digunakan agar tidak salah, ketik:
```bash
lslbk
```
kemudian lihat dan ingat semua type partisinya.

---

<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 38 07 PM (2)" src="https://github.com/user-attachments/assets/d55b8c13-a684-42ed-9199-0b69556888f5" />

---

Contohnya seperti dalam gambar ini

- nvme0n1p5 dengan ukuran 512M -> Partisi __EFI__
- nvme0n1p6 dengan ukuran 4G -> Partisi __Swap__
- nvme0n1p7 dengan ukuran 44,3G (sisa ruang dari partisi 50G) -> Partisi __Root__

---

# Format Partisi (root)

Selanjutnya, ketik perintah ini:
```bash
mkfs.ext4 /dev/nvme0n1p7
```

### Penjelasan
Perintah tersebut digunakan untuk membuat filesystem ext4 pada partisi __Root__.

---

# Membuat Swap

Untuk membuat partisi __Swap__, ketik:
```bash
mkswap /dev/nvme0n1p6
```

Selanjutnya aktifkan __Swap__ yang telah dibuat dengan cara mengetik:
```bash
swapon /dev/swap_partition
```

### Penjelasan
__Swap__ berfungsi sebagai memori cadangan yang akan digunakan secara otomatis ketika kapasitas __RAM__ sudah penuh.

---

# Format EFI Partition

Untuk membuat partisi __EFI__ (Boot), ketik:
```bash
mkfs.fat -F 32 /dev/efi_system_partition
```

### Penjelasan
Partisi EFI __WAJIB__ menggunakan format __FAT32__ agar bisa berfungsi dengan baik.

---

# Mount Filesystem

Sebelum lanjut ke tahap instalasi, seluruh partisi yang telah dibuat harus dipasang terlebih dahulu lewat proses __Mount__.

Untuk Mount partisi __Root__, ketik:
```bash
mount /dev/root_partition /mnt
```

Untuk Mount partisi __EFI__, ketik:
```bash
mount --mkdir /dev/efi_system_partition /mnt/boot
```

__catatan:__
- Sesuaikan __root_partition__ dan __efi_system_partition__ dengan nama partisi yang telah dibuat sebelumnya.

__contoh:__
- jika partisi Root adalah __nvme0n1p7__ dan partisi EFI adalah __nvme0n1p5__, maka perintahnya menjadi mount __/dev/nvme0n1p7__ dan __mount --mkdir /dev/nvme0n1p5__.

Setelah proses __Mount__ selesai, kita dapat melanjutkan ke tahap instalasi dengan menjalankan perintah berikut untuk menginstal sistem dasar:
```bash
pacstrap -K /mnt base linux linux-firmware base base-devel networkmanager
```

### Penjelasan

- `base` → paket inti sistem Arch Linux
- `linux` → kernel Linux
- `linux-firmware` → Firmware untuk berbagai perangkat keras
- `iwd` → digunakan untuk mengakses jaringan setelah proses instalasi selesai
   
---

Tunggu hingga proses instalasi selesai. Kecepatan instalasi sangat bergantung pada koneksi internet yang digunakan. Disarankan untuk menggunakan __WiFi__ karena meskipun hotspot dari HP tetap bisa digunakan, prosesnya akan memakan waktu jauh lebih lama dibandingkan menggunakan __WiFi__.

---

# Membuat fstab

Setelah proses instalasi selesai, buat file `fstab` dengan mengetik:
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Penjelasan
File ini berfungsi untuk menentukan partisi mana saja yang akan otomatis di-mount saat perangkat melakukan boot.

---

# Masuk ke Sistem Baru (Chroot)

Selanjutnya ketiklah:
```bash
arch-chroot /mnt
```

### Penjelasan
Perintah tersebut digunakan untuk memindahkan root shell ke sistem __Arch Linux__ yang baru saja diinstal.

---

# Mengatur Timezone

Untuk mengatur timezone, ketik:
```bash
ln -sf /usr/share/zoneinfo/Area/Location /etc/localtime
```

__Contoh: Mengatur timezone yang sesuai dengan timezone Indonesia__
```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

Untuk mengsinkronkan __Hardware Clock__ ketik:
```bash
hwclock --systohc
```

---

 # Localization

Untuk melanjutkan ke tahap lokalisasi, unduh dan instal `Neovim` terlebih dahulu. 

__Neovim__ merupakan fork dari __Vim__ yang bertujuan untuk meningkatkan basis kode, memungkinkan implementasi API yang lebih mudah, pengalaman pengguna yang lebih baik, dan implementasi plugin.[^1]

---

Selanjutnya ketik:
```bash
pacman -S neovim
```
lalu:
```bash
 nvim /etc/locale.gen
```
terus uncommenting #en_US.UTF-8
(Pagarnya yang dihapus) 

```bash
 locale-gen
```
Dan di lanjutkan dengan:
```bash
locale > /etc/locale.conf
```

```bash
nvim /etc/locale.conf
```

---


Setelah sudah terisi, tekan key __escape__ pada keyboard dan ketik `:wq`

---

# Hostname

untuk membuat __Useradd__, ketik:
```bash
useradd -m (nama user) 
```

---


Selanjutnya, Insert lagi dan masukkan nama hostname yang diinginkan. 

Contoh: __"Luthfi"__
- useradd -m Luthfi


### Penjelasan
__useradd__ adalah nama yang digunakan untuk mengidentifikasi perangkat di jaringan.
---


# Password Root

Selanjutnya, ketik:
```bash
passwd (nama user) 
```

### Penjelasan
Perintah tersebut digunakan untuk mengatur __password__ administrator/root.

---
# Masuk Mode Root

```bash
echo "(nama user) ALL=(ALL:ALL) ALL " > /etc/sudoers.d/00_namauser
```

---


# Install Bootloader (GRUB)

Langkah berikutnya adalah melakukan instalisasi `Bootloader` atau `GRUB`, GRUB (GRand Unified Bootloader) adalah sebuah boot loader. GRUB saat ini juga dikenal sebagai `GRUB 2`. Sementara GRUB asli, atau dikenal dengan `GRUB Legacy`, yang sesuai dengan versi 0.9x.[^2] 

Ikuti langkah langkah ini untuk menginstall Bootloader (GRUB):
```bash
pacman -S grub efibootmgr os-prober
```

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```


---


# Reboot

Jika sudah mengikuti langkah-langkah diatas dengan benar. maka tahap terakhir yang dilakukan dari instalisasi adalah melakukan __Reboot__.

Keluar dari chroot:
```bash
exit
```

Selanjutnya, lakukan Unmount dengan mengetik:
```bash
umount -R /mnt
```

Lalu Reboot dengan mengetik:
```bash
reboot
```

Perhatian:
- Pastikan USB installer telah dilepas setelah melakukan proses restart.

---

__SELESAI.__

---

## Sumber Tambahan:

[^1]: https://wiki.archlinux.org/title/Neovim

[^2]: https://wiki.archlinux.org/title/GRUB






