# Instalasi <img width="960" height="546" alt="719827555_1631578731262225_6242484077355546964_n" src="https://github.com/user-attachments/assets/fdafb652-5c17-48f2-bb59-9f5481538f95" />
<img width="960" height="546" alt="716389964_1399572942222894_7955005953849238079_n" src="https://github.com/user-attachments/assets/0a122637-0baa-41b6-b580-d8355a456978" />
<img width="960" height="546" alt="716289904_1376006034433892_5938722405745322594_n" src="https://github.com/user-attachments/assets/88bb3769-8176-4e07-962b-292cbbd75a33" />
<img width="960" height="720" alt="715463660_1643363726730121_4318970356919555355_n" src="https://github.com/user-attachments/assets/e5ea7317-da43-4410-b506-be9bcc9140f8" />
<img width="960" height="546" alt="715263017_1544433370416171_3553942927055657228_n" src="https://github.com/user-attachments/assets/b9cb23bd-8167-40ae-ad21-87b1917abcff" />
<img width="960" height="720" alt="714954703_1511150300481638_2348229925858542982_n" src="https://github.com/user-attachments/assets/6aa836e5-5618-453a-bc61-826226aed5bc" />
<img width="960" height="546" alt="714698686_985083930807280_3233290880590722448_n" src="https://github.com/user-attachments/assets/9bef8bb7-47e7-4ea2-9668-4cb86ab0712d" />
<img width="960" height="546" alt="713663132_2791971964512500_303831181638257125_n" src="https://github.com/user-attachments/assets/26710e5d-d4d3-46ad-9ed2-fca1fedd8f79" />
<img width="960" height="546" alt="713343148_2183902335789333_1050327620028100101_n" src="https://github.com/user-attachments/assets/1efc5413-fae8-49c9-b089-7262bab61e5d" />
<img width="960" height="546" alt="713163126_2057599788181520_8399220712224777598_n" src="https://github.com/user-attachments/assets/4174b2c7-2439-474a-9cc1-b6548694bb35" />
<img width="960" height="546" alt="712783078_1471884147583857_3194870545272475823_n" src="https://github.com/user-attachments/assets/de45f548-bfc4-43a7-9ff0-698f64bc9d62" />
<img width="960" height="546" alt="711543134_865398386615744_566956582063129660_n" src="https://github.com/user-attachments/assets/5a24c1d7-3908-4bdb-b7cf-e86f772132bb" />
<img width="960" height="546" alt="711168577_1526478392597741_2501481059580661621_n" src="https://github.com/user-attachments/assets/961c733c-f785-4f27-a552-a6afe0e7e598" />
<img width="960" height="720" alt="710516705_3936174530021804_7851734375160177228_n" src="https://github.com/user-attachments/assets/1665b9d7-f3e5-4340-b6d1-76f4ab3e4f5f" />
<img width="960" height="546" alt="709004804_26920509014307401_4287895050904833041_n" src="https://github.com/user-attachments/assets/6f5ccacf-3fc9-4d57-aae4-03d3d8a1a8d2" />
<img width="960" height="546" alt="708937664_965746866292046_7390020998421303379_n" src="https://github.com/user-attachments/assets/7c641c4d-2dd9-4571-83ca-ccbb02a609d9" />
Arch Linux Hardened  

## Fase Live ISO

Pertama-tama, setelah booting dari USB installer, kamu perlu terhubung ke internet. Jalankan `iwctl`, lalu cari nama device wifimu dengan `device list`. Setelah itu scan jaringan yang tersedia, sambungkan ke wifi pilihanmu, dan verifikasi koneksi dengan `ping 1.1.1.1`  kalau ada balasan, berarti internet sudah aktif.

Setelah online, saatnya menyiapkan disk. Ketik `lsblk` untuk melihat daftar disk yang ada, lalu buka `cfdisk` untuk membuat dua partisi dasar: partisi **boot** 1GB bertipe EFI system, dan partisi **root** sisanya bertipe Linux filesystem. Hati-hati saat di sini kalau salah hapus partisi penting, langsung tekan Quit, jangan Write.

Partisi root yang sudah dibuat lalu dijadikan **Physical Volume** LVM dengan `pvcreate`, kemudian dibungkus dalam sebuah **Volume Group** bernama `proc` via `vgcreate`. Dari sana, kamu iris-iris menjadi beberapa Logical Volume:

| Volume | Ukuran |
|--------|--------|
| root   | 20G    |
| vars   | 2G     |
| vtmp   | 2G     |
| vlog   | 5G     |
| vaud   | 2G     |
| home   | 10G    |

Pemisahan ini mengikuti standar CIS Benchmark agar setiap partisi sistem terisolasi satu sama lain.

Selanjutnya, enkripsi. Setiap Logical Volume yang ingin dilindungi diformat dengan **LUKS** menggunakan `cryptsetup luksFormat`, di sinilah kamu memasukkan password master enkripsimu. Setelah itu buka partisi tersebut dengan `luksOpen` sehingga muncul sebagai device virtual di `/dev/mapper/`, siap diformat. Format partisi-partisi itu dengan `mkfs.ext4`, kecuali partisi boot yang harus `mkfs.vfat -F32` karena spesifikasi UEFI hanya bisa membaca FAT32.

Semua partisi lalu di**mount** ke dalam `/mnt` dengan flag-flag keamanan CIS:

- `nodev` — mencegah pembuatan device ilegal
- `nosuid` — mencegah privilege escalation
- `noexec` — mencegah eksekusi binary langsung dari partisi seperti `/tmp` dan `/home`

Dengan struktur disk siap, jalankan `pacstrap` untuk menginstal semua package utama ke `/mnt`. Daftar pentingnya meliputi:

- `linux-lts` — kernel Long Term Support yang lebih stabil untuk sistem hardened
- `intel-ucode` / `amd-ucode` untuk menambal celah keamanan hardware seperti Spectre dan Meltdown
- `booster`, `networkmanager`, `pam_mount`, dan lain-lain

Terakhir di fase ini, generate file `/etc/fstab` otomatis dengan:

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

Lalu tambahkan entri **tmpfs** secara manual agar `/tmp` berjalan di RAM:

```bash
echo "/tmpfs /tmp tmpfs defaults,nosuid,nodev,noexec,size=1G 0 0" >> /mnt/etc/fstab
```

---

## Transisi: Masuk ke Sistem Baru

```bash
arch-chroot /mnt
```

Dengan perintah ini kamu berpindah dari lingkungan Live USB masuk seolah-olah sudah berada di dalam sistem Arch Linux yang baru terinstal di harddisk.

---

## Fase Arch-Chroot

Di dalam chroot, isi file `/etc/hostname` dengan nama komputermu, buat symbolic link timezone ke `Asia/Jakarta`, lalu sinkronkan jam hardware:

```bash
echo namakomputer > /etc/hostname
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```

Untuk bahasa sistem, buka `/etc/locale.gen` dan hapus tanda `#` di depan `en_US.UTF-8`, jalankan `locale-gen`, lalu isi `/etc/locale.conf` dengan `LANG=en_US.UTF-8`.

```bash
locale-gen
locale > /etc/locale.conf
```

Buat user baru dengan `useradd`, set passwordnya dan ini **krusial**  password user harus sama persis dengan password LUKS yang kamu buat tadi. Ini bukan kebetulan, ini disengaja agar modul `pam_mount` bisa membongkar enkripsi secara transparan saat kamu login tanpa harus mengetik password dua kali. Tambahkan user ke sudoers agar punya akses administrator penuh.

> [!WARNING]
> Password user **wajib sama** dengan password LUKS. Jika berbeda, `pam_mount` tidak bisa membuka enkripsi secara otomatis saat login.

Konfigurasi `pam_mount.conf.xml` untuk memberitahu sistem bahwa ketika user tersebut login, ambil passwordnya dan gunakan untuk membuka volume LUKS yang sesuai, lalu mount otomatis ke direktori homenya:

```bash
nvim /etc/security/pam_mount.conf.xml
```

Kemudian edit `/etc/pam.d/system-login` dan sisipkan baris `auth required pam_mount.so` agar PAM mengikutsertakan modul ini dalam proses otentikasi login.

Untuk initramfs, gunakan **Booster** — pengganti `mkinitcpio` yang ditulis dalam Go dan jauh lebih cepat saat booting. Konfigurasi `booster.yaml`:

```yaml
network:
  dhcp: on
universal: false
modules: -*,ext4
extra_files: fsck,fsck.ext4
strip: true
enable_lvm: true
```

Lalu build image-nya:

```bash
booster build --kernel-version /boot/booster-linux-lts-new.img
```

Terakhir, instal bootloader **systemd-boot**:

```bash
bootctl --path=/boot install
```

Buat file konfigurasi entry di `/boot/loader/entries/booster.conf`:

```
title   arch with booster
linux   /vmlinuz-linux-lts
initrd  /intel-ucode.img
initrd  /booster-linux-lts-new.img
options root=/dev/proc/root rw
```

Set entry ini sebagai default di `/boot/loader/loader.conf`:

```
default booster.conf
```

---

## Finalisasi

```bash
exit
umount -R /mnt
reboot
```

Ketik `exit` untuk keluar dari chroot, lalu `umount -R /mnt` untuk melepas semua partisi dengan aman agar data tersimpan dengan benar ke disk. Terakhir, `reboot` — dan jangan lupa **cabut flashdisk** saat logo BIOS muncul.

Nah, terus kata bapaknya LVM cocok buat perpustakaan karena bisa buat manajemen kapasitas storage untuk pengelolaan buku baru masuk. Kalau LUKS cocok buat arsip karena kan ga semua arsip bersifat publik, jadi dengan LUKS ini bisa kita atur agar data arsip tersimpan dalam enkripsi biar kalaupun server dicuri, data tidak akan terbaca tanpa kunci yang benar. Pake Iptables kita pastikan setiap jenis akses bisa masuk lewat port mana dan mana juga port yang ditutup.

