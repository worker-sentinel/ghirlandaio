# Kernel modul 

## 1. Masuk Sebagai Root

```
sudo su
```

Berpindah ke user root untuk melakukan konfigurasi sistem.

---

## 2. Memeriksa Modul FreeVXFS

```
lsmod | grep freevxfs
```

Memeriksa apakah modul `freevxfs` sedang dimuat oleh kernel.

---

## 3. Memeriksa Modul HFS

```
lsmod | grep hfs
```

Memeriksa apakah modul `hfs` sedang digunakan.

---

## 4. Memeriksa Modul HFS+

```
lsmod | grep hfsplus
```

Memeriksa keberadaan modul `hfsplus`.

---

## 5. Memeriksa Modul JFFS2

```
lsmod | grep jffs2
```

Memeriksa apakah modul `jffs2` aktif.

---

## 6. Memeriksa Modul SquashFS

```
lsmod | grep squashfs
```

Memeriksa status modul `squashfs`.

---

## 7. Memeriksa Modul UDF

```
lsmod | grep udf
```

Memeriksa apakah modul `udf` sedang dimuat.

---

## 8. Memeriksa Modul USB Storage

```
lsmod | grep usb-storage
```

Memeriksa keberadaan modul penyimpanan USB.

---

## 9. Mengedit File Konfigurasi Modul

```
nvim /etc/modprobe.d/disable-module.conf
```

Membuka file konfigurasi untuk menonaktifkan modul kernel yang tidak diperlukan.

```text
blacklist freevxfs
install freevxfs /bin/false

blacklist hfs
install hfs /bin/false

blacklist hfsplus
install hfsplus /bin/false

blacklist jffs2
install jffs2 /bin/false

blacklist udf
install udf /bin/false

blacklist usb-storage
install usb-storage /bin/false
```

---

## 10. Memeriksa modul kernel 
lsmod | grep fat
Modul kernel untuk sistem berkas FAT/vfat

nvim /etc/modprobe.d/01-disable-module.conf
untuk membuka text editor Neovim





## 11. Regenerasi Initramfs

```
mkinitcpio -P
```

Membangun ulang initramfs agar perubahan konfigurasi modul diterapkan pada proses boot berikutnya.

---

## 11. Keluar dari Sesi Root

```
exit
```

Keluar dari user root dan kembali ke user biasa.
