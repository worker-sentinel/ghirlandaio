# Panduan Instalasi Arch Linux 

Selamat datang di panduan instalasi Arch Linux Kelompok 5

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

Tahap awal dimulai dengan memasukkan flashdisk ke laptop, kemudian klik tombol __Select__ dan pilih fle Archiso yang sudah diunduh sebelumnya. Untuk bagian __Partition Scheme__, laptop keluaran baru umumnya sudah menggunakan __GPT__.  

__Diperlukan:__
- **USB Drive:** Minimal kapasitas 2GB (direkomendasikan 4GB atau lebih).

__Perhatian!__
- Proses ini akan menghapus **SELURUH DATA** di dalam USB drive.

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


Setelah itu, masuk ke Boot Menu dengan menekan F12 (khusus p


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5a149ab2-6ed8-42c2-9fcb-051c2aa6c577" />

> Pencet usb


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/92555886-552a-4826-8e01-1026f12e737d" />

> Pencet enter



<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/94602b06-99fa-48d0-9708-41540148c7dc" />

> Tunggu sampai tampilan seperti ini lalu ketik iwctl

# Kondisi Khusus

Ketika device list di iwctl tidak terlihat wlan0 wlan1 dan semacamnya,ketik rfkill list


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/0e30f0bd-392d-434a-926d-051a9fdc5393" />

Contoh seperti ini, lalu ketik rfkill unblock wlan, Semisal ingin mengunblock bluetooth ketik saja unblock bluetooth

Ketik device list


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/25b6ef19-b6f1-49a0-8b40-269feecce651" />

Station wlan(0)(menyesuaikan name di tabel)

Contoh: 

```bash
Station wlan0 connect (nama wifi)
```

Kalo ada spasi dalam nama wifi memakai petik terlebih dahulu, Contoh "uin fah"


<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/74161428-1d8f-429d-8d0d-db4d143604c7" />

Masukan password

lalu enter saja

Cek jaringan yaitu dengan mengetik 

exit

Terlebih dahulu lalu

```bash
ping 8.8.8.8
```

<img width="1599" height="899" alt="image" src="https://github.com/user-attachments/assets/5090e3f0-8037-4aff-b1b7-a37ba61bf03c" />

Kalau seperti ini connect ke wifi, jika ga konek network unreachable


habis ni ctrl+c

---

# Sinkronisasi Waktu

```bash
timedatectl
```

### Penjelasan
Jam sistem harus benar agar verifikasi SSL dan package signature tidak gagal.

untuk partisi pakai lsblk. Untuk melihat partisi yang akan dipakai dari 50 gb tadi
<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 38 07 PM (1)" src="https://github.com/user-attachments/assets/9f64b4ac-1a4c-4a78-b790-edc7dff007af" />

Berikut nya gunakan cfdisk /dev/nvme0n1 (bisa jadi sda sesuai hardisk)
<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 48 16 PM" src="https://github.com/user-attachments/assets/457b1dc0-da8c-4a58-aa4e-d498013ccb72" />

Geser ke kiri untuk ke new dan masukkan untuk partisinya 512M, 4G, (dan sisa dari berapa partisi)
<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 51 02 PM" src="https://github.com/user-attachments/assets/ae5c3765-8289-44de-a272-af0462f88004" />

Gunakan Tombol ini untuk navigasi (Geser kanan & Kiri)
<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 51 15 PM" src="https://github.com/user-attachments/assets/74a4787d-52d2-4301-b262-0bc1393d101d" />

Masukan angka 4G, 512M Dan sisa ruang partisi untuk root. Contoh dari 50G Setelah di partisi menjadi sisa 44,3G Maka masukan 44,5G
<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 53 02 PM" src="https://github.com/user-attachments/assets/d06d0474-f66a-4787-8d0b-6ac699c4293f" />

Sebelum enter bagian write, pastikan untuk partisinya sudah di setting type nya (Boot = 512M Untuk EFI System, Swap = 4G Untuk Linux Swap dan sisa ruang partisi (Contoh untuk free disk 50G Sudah kepakai untuk EFI & Swap yaitu 5,7G. Maka untuk sisanya yaitu 44.3G) untuk Root yaitu Linux File System)
<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 53 54 PM" src="https://github.com/user-attachments/assets/c077b472-9fd4-44a1-8d90-2579adcb9264" />

Setelah write, geser ke opsi quit dan enter

Struktur Partisi yang Disarankan

UEFI

| Mount Point | Fungsi |
|---|---|
| `/boot` | EFI Partition |
| `swap` | Virtual memory |
| `/` | Root system |

Selajutnya masuk ke tahap formating yaitu format partisi, swapping dan format EFI

Catatan : Partisi harus sesuai dengan type yang sudah di write tadi. jika sampai salah, dapat menyebabkan data di partisi hilang karena datanya telah timpa (salah satu nya Kehilangan OS). Untuk memastikan kembali partisi untuk digunakan agar tidak salah, ketik lslbk. kemudian lihat dan ingat semua type partisinya.

<img width="1599" height="899" alt="WhatsApp Image 2026-05-14 at 9 38 07 PM (2)" src="https://github.com/user-attachments/assets/d55b8c13-a684-42ed-9199-0b69556888f5" />
Contohnya seperti ini untuk Partisi nvme0n1p5 dengan size 512M (EFI), partisi nvme0n1p6 dengan size 4G (SWAP) Dan partisi nvme0n1p7 dengan size 44.3G yaitu sisa ruang kosong dari Partisi 50G (root)

Format Partisi (root)

```bash
mkfs.ext4 /dev/nvme0n1p7
```

Penjelasan
Membuat filesystem ext4 pada partisi root.

---

Membuat Swap

```bash
mkswap /dev/nvme0n1p6
```

Aktifkan swap:

```bash
swapon /dev/swap_partition
```

Penjelasan
Swap digunakan sebagai memori cadangan ketika RAM penuh.

---

Format EFI Partition

```bash
mkfs.fat -F 32 /dev/efi_system_partition
```

Penjelasan
EFI partition harus menggunakan FAT32.


Mount Filesystem

Sebelum masuk ke tahap penginstalan, partisi terlebih dahulu harus dipasang (Mount)

Mount root:

```bash
mount /dev/root_partition /mnt
```

Mount EFI:

```bash
mount --mkdir /dev/efi_system_partition /mnt/boot

catatan : untuk root_partition & efi_System_partition disesuaikan dengan nama partisi yang tadi (Contoh, tadi Partisi root yaitu nvme0n1p7 dan EFI nvme0n1p5, maka perintahnya menjadi mount /dev/nvme0n1p7 dan mount --mkdir /dev/nvme0n1p5)

setelah mount kita masuk ke dalam tahap penginstalan dengan perintah :
Instalasi Sistem Dasar

```bash
pacstrap -K /mnt base linux linux-firmware base base-devel iwd
```

Penjelasan

- `base` → paket inti sistem
- `linux` → kernel Linux
- `linux-firmware` → firmware hardware
- `iwd` → Mengakses jaringan setelah penginstalan
   
---

Tunggu penginstalan hingga selesai, Kecepatan bergantung pada internet. Disarankan menggunakan wifi. meski bisa dengan hotspot HP, Tetapi prosesnya memakan waktu yang lama daripada menggunakan WIFI.

setelah proses install selesai, buat fstab nya untuk menentukan partisi mana yang otomatis dimount saat boot.

Membuat fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```









