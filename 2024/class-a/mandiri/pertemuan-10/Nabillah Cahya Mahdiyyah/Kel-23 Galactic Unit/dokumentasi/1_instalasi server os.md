# Instalasi Server OS

## Melihat Struktur Partisi

```bash
lsblk
```

Menampilkan disk, partisi, dan mount point yang tersedia pada sistem.

---

## Memeriksa Partisi

```bash
cfdisk /dev/nvme0n1
```

Membuka utilitas untuk melihat struktur partisi pada disk NVMe.

---

## Memeriksa Konfigurasi Linux LTS

```bash
nvim /etc/mkinitcpio.d/linux-lts.preset
```

Membuka file konfigurasi preset kernel Linux LTS.

---

## Regenerasi Initramfs

```bash
mkinitcpio -P
```

Membuat ulang initramfs untuk seluruh kernel yang terpasang.

---

## Memeriksa Kembali Partisi

```bash
lsblk
```

Memastikan struktur partisi dan volume telah terdeteksi dengan benar.

---

## Mencoba Mount Home

```bash
mount /mnt/home
```

Mencoba me-mount partisi home ke direktori `/mnt/home`.

---

## Masuk ke Sistem dengan Chroot

```bash
arch-chroot /mnt
```

Masuk ke sistem Linux yang berada pada direktori `/mnt`.

---

## Verifikasi Mount Point

```bash
lsblk
```

Memeriksa kembali volume dan mount point yang aktif.

---

## Keluar dan Reboot

```bash
exit
reboot
```




