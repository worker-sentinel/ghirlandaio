# Panduan Instalasi Arch Linux 
untuk memenuhi tugas matakuliah Perpustakaan dan Arsip Digital

## Cara Download Arch Iso
<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/763ed29b-fbd6-4be2-aeb2-a504e98e9e1b" />

Langkah pertama yang perlu dilakukan adalah mengunduh Archiso.

Buka situs __[archlinux.org/download](https://archlinux.org/download/)__, lalu scroll ke bawah hingga menemukan bagian Indonesia dan pilih __[citrahost.com](https://mirror.citrahost.com/archlinux/iso/2026.05.01/)__ sebagai mirror unduhan.
## Menggunakan `archinstall`

```bash
archinstall
```

Dengan `archinstall`, kita tidak perlu mengetik semua perintah instalasi secara manual.  
Kita cukup memilih menu yang tersedia satu per satu.

---

## Masuk ke Installer

Ketik perintah berikut:

```bash
archinstall
```

Lalu tekan **Enter**.

Setelah itu, menu instalasi Arch Linux akan muncul.

---

## Menu yang Perlu Diisi
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/bdb9b260-ee0b-41da-a401-fbb7b71856fb" />

Di sebelah kiri layar, biasanya akan ada beberapa menu seperti:

```text
Archinstall language
Locales
Mirrors and repositories
Disk configuration
Swap
Bootloader
Kernels
Hostname
Authentication
Profile
Applications
Network configuration
Additional packages
Timezone
Automatic time sync (NTP)
Install
```

Isi menu-menu tersebut secara bertahap.

---

## Archinstall Language
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/689f6eef-763e-4a94-85ae-302b7c0e54bc" />

Pilih:

```text
English
```

Bahasa Inggris lebih disarankan karena istilah Linux dan dokumentasinya kebanyakan menggunakan bahasa Inggris.

---

## Locales

Masuk ke menu:

```text
Locales
```

Gunakan pilihan seperti ini:

```text
Keyboard layout: us
Locale language: en_US
Locale encoding: UTF-8
```

Kalau bingung, kamu bisa menggunakan pilihan default yang tersedia.

---

## Mirrors and Repositories

Mirror adalah server tempat Arch Linux mengambil file dan paket instalasi.

Masuk ke:

```text
Mirrors and repositories
```

Pilih negara terdekat, misalnya:

```text
Indonesia
```

Kalau mirror Indonesia lambat, kamu bisa memilih:

```text
Singapore
```

---

## Disk Configuration

Bagian ini penting karena berhubungan dengan tempat instalasi Arch Linux.

Masuk ke:

```text
Disk configuration
```

Pilih:

```text
Use a best-effort default partition layout
```

Artinya, installer akan membantu membuat susunan partisi secara otomatis.
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/d7b2254e-98a9-45b2-93ad-abf6dcda0f02" />

Setelah itu, pilih disk virtual kamu. Biasanya namanya seperti:

```text
/dev/sda
```

Kalau diminta memilih filesystem, pilih:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/9f0c861a-a4d1-4fe9-8f0f-571ca530d2e2" />

```text
ext4
```

Kalau ditanya tentang encryption, pilih:

```text
No
```

Untuk praktikum awal, encryption belum diperlukan.

Ringkasnya:

```text
Disk layout: default
Filesystem: ext4
Encryption: No
```

---

## Swap

Swap adalah ruang cadangan di disk yang dapat membantu sistem ketika RAM mulai penuh.
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/4792bd69-ca6b-400f-9617-768737016349" />

Masuk ke:

```text
Swap
```

Pilih:

```text
Enabled
```

atau:

```text
True
```

Kalau tidak yakin, biarkan mengikuti pilihan default.

---

## Bootloader

Bootloader adalah bagian yang membantu komputer menyalakan sistem operasi.

Masuk ke:

```text
Bootloader
```

Pilih:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/8cd23e52-229f-4485-ace0-c5ca840a5a98" />

```text
GRUB
```

GRUB adalah pilihan yang umum dan aman untuk pemula.

---

## Kernels

Kernel adalah inti dari sistem operasi Linux.

Masuk ke:

```text
Kernels
```

Pilih:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/ade23d20-6eb5-43eb-8b0c-c834bb0c91ab" />

```text
linux
```

Untuk awal belajar, cukup gunakan kernel standar.

---

## Hostname

Hostname adalah nama komputer di dalam sistem Arch Linux.

Masuk ke:

```text
Hostname
```

Isi nama yang sederhana, misalnya:

```text
arch-vm
```

Boleh juga memakai nama lain, misalnya:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/c286c0bc-9cef-4cdf-a869-f48bd6a8d29d" />

```text
archlinux
```

---

## Authentication

Bagian ini digunakan untuk membuat password dan user.

Kalau di layar muncul pesan seperti ini:

```text
Missing configurations:
- Either root-password or at least 1 user with sudo privileges must be specified
```

artinya kamu belum membuat password root atau user yang punya hak admin.

Masuk ke menu:

```text
Authentication
```

Di bagian ini, kamu bisa memilih membuat **root password**.

---

## Root Password

Kalau ingin membuat password untuk akun root, pilih:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/b334b799-1b91-4789-a3cc-204202a5d1ed" />

```text
Root password
```

Masukkan password dua kali.

Root adalah akun admin utama di Linux.
---

## Profile

Bagian ini menentukan bentuk sistem yang akan di-install.

Masuk ke:

```text
Profile
```

Kalau kamu ingin memakai tampilan desktop seperti Windows, pilih:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/2a020dde-2a19-400e-bdb6-4b760df9a873" />

```text
Desktop
```

Setelah itu pilih desktop environment.

Rekomendasi untuk pemula:

```text
KDE Plasma
```

atau:

```text
GNOME
```

Kalau ingin tampilan yang cukup familiar dan nyaman digunakan di VirtualBox, pilih:

```text
KDE Plasma
```

---

## Graphics Driver

Kalau diminta memilih graphics driver, pilih:

```text
All open-source
```

Kalau ada pilihan khusus VirtualBox atau VM, boleh pilih pilihan tersebut.

---


## Timezone

Masuk ke:

```text
Timezone
```

Pilih:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/16b2e4fc-119c-4adb-abe7-ce10fd532b0d" />

```text
Asia/Jakarta
```

---

## Automatic Time Sync atau NTP

Masuk ke:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/1a948290-1eb2-4e79-9890-124413223cc9" />

```text
Automatic time sync (NTP)
```

Pilih:

```text
yes
```

Fungsinya supaya jam sistem otomatis sinkron.

---

## Mulai Instalasi

Pilih menu:

```text
Install
```

Lalu tekan **Enter**.

Kalau diminta konfirmasi, pilih:
<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/e0b4c642-2713-45bd-b50c-c99ad1507ee6" />

```text
Yes
```

Setelah itu tunggu sampai proses instalasi selesai.

Jangan menutup VirtualBox saat proses masih berjalan.

---

## Reboot

Setelah instalasi selesai, ketik:

```bash
reboot
```

Lalu tekan **Enter**.

---

## Login ke Arch Linux

Setelah reboot, kamu akan masuk ke sistem Arch Linux yang sudah ter-install.

Login menggunakan username yang tadi dibuat.

Contoh:

```text
Username: Nadya
Password: 123456
```


Selesai. :))
