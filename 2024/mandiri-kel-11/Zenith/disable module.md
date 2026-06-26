

## 1. Memeriksa Status Modul Kernel

Memeriksa apakah modul filesystem yang tidak digunakan sedang aktif.

```bash
lsmod | grep cramfs
```

Memeriksa modul FreeVxFS.

```bash
lsmod | grep freevxfs
```

Memeriksa modul JFFS2.

```bash
lsmod | grep jffs2
```

Memeriksa modul HFS.

```bash
lsmod | grep hfs
```

Memeriksa modul HFS+.

```bash
lsmod | grep hfsplus
```

Memeriksa modul SquashFS.

```bash
lsmod | grep squashfs
```

Memeriksa modul UDF.

```bash
lsmod | grep udf
```

Memeriksa modul USB Storage.

```bash
lsmod | grep usb-storage
```

Memeriksa status modul Bluetooth.

```bash
lsmod | grep bluetooth
```

---

## 2. Konfigurasi Pemblokiran Modul

Membuka file konfigurasi modprobe untuk menonaktifkan modul yang tidak diperlukan.

```bash
nvim /etc/modprobe.d/01-custom.conf
```

Tambahkan konfigurasi blacklist sesuai kebutuhan keamanan sistem.

---

## 3. Memperbarui Initramfs

Membangun ulang initramfs agar konfigurasi blacklist diterapkan pada saat boot.

```bash
mkinitcpio -P
```

Proses ini akan memperbarui seluruh preset kernel yang terpasang, termasuk Linux Hardened dan Linux LTS.

---

## 4. Keluar dari Sesi Root

Keluar dari mode root setelah konfigurasi selesai.

```bash
exit
```
