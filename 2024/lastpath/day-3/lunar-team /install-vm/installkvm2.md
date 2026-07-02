# Dokumentasi Instalasi KVM/QEMU di Arch Linux

## 1. Instalasi Paket Virtualisasi
```
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```
Perintah ini digunakan untuk menginstal seluruh paket yang dibutuhkan agar komputer dapat menjalankan mesin virtual menggunakan KVM dan QEMU.

## 2. Pemilihan Provider QEMU

Saat menjalankan instalasi muncul pilihan:
```
1) qemu-base
2) qemu-desktop
3) qemu-full
```
Arch Linux menyediakan beberapa varian QEMU.
- qemu-base
  Instalasi minimal yang hanya berisi komponen inti.

- qemu-desktop
  Berisi fitur desktop tambahan.

- qemu-full
  Menginstal seluruh fitur QEMU.

Jika hanya ingin menjalankan virtual machine biasa, qemu-base sudah cukup.

## 3. Error bridge-utils

Muncul pesan:
error: target not found: bridge-utils

Penyebab:
Paket bridge-utils sudah tidak tersedia pada repository Arch Linux karena fungsinya telah digantikan oleh utilitas dari paket iproute2.

Solusi
Hapus bridge-utils dari daftar paket.
```
sudo pacman -S qemu-base virt-manager virt-viewer dnsmasq vde2 openbsd-netcat
```
---

## 4. Memperbarui Keyring
```
sudo pacman -S archlinux-keyring
```
Menginstal atau memperbarui keyring Arch Linux agar proses verifikasi tanda tangan paket berjalan dengan benar.
Keyring berisi public key yang digunakan untuk memverifikasi keaslian paket.

---

## 5. Konfirmasi Instalasi

Saat muncul
```
Proceed with installation? [Y/n]
```
Ketik
```
y
```
untuk melanjutkan instalasi.

---

## 6. Error Dependency

Muncul pesan:
```
failed to prepare transaction
```
atau
```
breaks dependency
```
Penyebab:
Versi paket yang akan diinstal tidak sesuai dengan paket yang sudah ada di sistem.
Biasanya terjadi karena sistem belum diperbarui.

---

## 7. Update Sistem
```
sudo pacman -Syu
```

- -S
  Sinkronisasi paket.

- -y
  Memperbarui database repository.

- -u
  Meng-upgrade seluruh paket ke versi terbaru.

Perintah ini memastikan seluruh paket memiliki versi yang kompatibel.

---

## 8. Instalasi Setelah Update
```
sudo pacman -Syu qemu-base virt-manager virt-viewer dnsmasq vde2 openbsd-netcat
```

Perintah ini melakukan dua proses sekaligus:

1. Memperbarui sistem.
2. Menginstal seluruh paket virtualisasi.

---
