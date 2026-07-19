# Install Admin

## 1. Hapus Logical Volume Docker

```bash
lvremove /dev/namavolumegrup/dock
```
Perintah lvremove digunakan untuk menghapus Logical Volume (LV) bernama dock yang sebelumnya digunakan oleh Docker. Penghapusan volume yang sudah tidak digunakan bertujuan untuk membebaskan ruang penyimpanan sehingga dapat digunakan kembali oleh sistem operasi.

Menurut CIS Benchmark, menghapus komponen atau layanan yang tidak diperlukan merupakan bagian dari upaya mengurangi attack surface (permukaan serangan) dan membangun konfigurasi sistem yang aman (secure baseline). Sistem sebaiknya hanya menyimpan komponen yang memang dibutuhkan.

## 2. Cek Struktur Disk

```bash
lsblk
```
Perintah lsblk digunakan untuk menampilkan seluruh perangkat penyimpanan (block devices), partisi, serta hubungan antar partisi. Informasi ini diperlukan agar administrator dapat memastikan lokasi partisi root, boot, maupun partisi lainnya sebelum melakukan proses mount.

## 3. Mount Partisi Root

```bash
mount /dev/proc/root /mnt
```
Perintah ini digunakan untuk memasang (mount) partisi root ke direktori /mnt sehingga isi sistem operasi dapat diakses sebagai target instalasi maupun proses pemeliharaan sistem.

Perintah mount memastikan filesystem terpasang dengan benar merupakan bagian dari konfigurasi awal sistem sebelum dilakukan proses hardening.

## 4. Mount Partisi Boot

```bash
mount /dev/namapartisi /mnt/boot
```

Contoh:

```bash
mount /dev/sda1 /mnt/boot
```
Perintah ini memasang partisi boot/EFI sehingga kernel, initramfs, dan bootloader dapat diakses selama proses instalasi maupun pembaruan sistem.

Partisi boot harus dipasang sebelum melakukan perubahan terhadap kernel ataupun bootloader agar seluruh perubahan tersimpan pada partisi yang benar.

## 5. Masuk ke Sistem Arch Linux

```bash
arch-chroot /mnt
```
arch-chroot digunakan untuk masuk ke sistem operasi yang berada pada direktori /mnt. Setelah masuk ke lingkungan tersebut, seluruh konfigurasi yang dilakukan akan diterapkan pada sistem operasi target, bukan pada sistem Live ISO.

Perintah ini digunakan selama proses instalasi maupun pemulihan sistem.

## 6. Edit File fstab

```bash
nvim /etc/fstab
```

- Cari entri `dock`.
- Hapus baris yang mengarah ke volume tersebut.
- Simpan perubahan lalu keluar dari editor.
File /etc/fstab berisi daftar filesystem yang akan dipasang secara otomatis ketika sistem melakukan boot. Karena logical volume dock telah dihapus, maka entri tersebut juga harus dihapus dari fstab agar sistem tidak mencoba melakukan mount terhadap volume yang sudah tidak ada.

Prinsip ini sejalan dengan CIS Benchmark yang menekankan pentingnya konfigurasi sistem yang konsisten serta hanya mempertahankan konfigurasi yang memang diperlukan sebagai bagian dari secure baseline.

## 7. Mount Partisi `/var` (jika dipisah)

```bash
mount /dev/proc/var /mnt/var
```
Jika direktori /var berada pada partisi terpisah, maka partisi tersebut harus dipasang secara manual sebelum digunakan. Direktori /var menyimpan berbagai data penting seperti log sistem, cache, database paket, dan data aplikasi.

CIS Benchmark membahas pentingnya pengelolaan filesystem pada tahap awal konfigurasi sistem agar penyimpanan dapat dikelola dengan baik dan risiko gangguan terhadap filesystem utama dapat dikurangi.

## 8. Install Desktop Environment

```bash
pacman -S xfce4 sddm kitty
```
Perintah ini menginstal:
- XFCE4 sebagai desktop environment.
- SDDM sebagai display manager.
- Kitty sebagai terminal emulator.

## 9. Aktifkan SDDM

```bash
systemctl enable sddm
```
Perintah ini mengaktifkan layanan SDDM agar otomatis berjalan ketika sistem melakukan booting.

Menurut dokumentasi systemd, systemctl enable membuat symbolic link sehingga service akan dijalankan secara otomatis pada saat startup.

## 10. Generate Ulang Initramfs

```bash
mkinitcpio -P
```
Perintah mkinitcpio -P membuat ulang seluruh file initramfs berdasarkan preset yang tersedia. Langkah ini diperlukan setelah terjadi perubahan konfigurasi sistem, kernel, filesystem, maupun driver agar sistem dapat melakukan boot dengan benar.

initramfs merupakan bagian penting dari proses boot Linux karena berisi driver dan modul yang dibutuhkan sebelum root filesystem dipasang.

## 11. Keluar dari Chroot

```bash
exit
```
Perintah exit digunakan untuk keluar dari lingkungan arch-chroot setelah seluruh konfigurasi selesai dilakukan sehingga administrator kembali ke lingkungan Live ISO.

Perintah ini merupakan prosedur standar administrasi sistem Linux.

## 12. Restart Sistem

```bash
reboot
```
Perintah reboot digunakan untuk memulai ulang komputer sehingga seluruh konfigurasi yang telah dilakukan, seperti perubahan fstab, aktivasi SDDM, dan pembaruan initramfs, dapat diterapkan saat sistem melakukan boot berikutnya.

Restart merupakan langkah akhir yang umum dilakukan setelah perubahan konfigurasi sistem.

---

# Ringkasan Command

```bash
lvremove /dev/namavolumegrup/dock

lsblk

mount /dev/volumegroup/root /mnt

mount /dev/sda1 /mnt/boot

arch-chroot /mnt

nvim /etc/fstab

pacman -S xfce4 sddm kitty

systemctl enable sddm

mkinitcpio -P

exit

reboot
```
