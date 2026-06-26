Memuat 3 file asciinema (instalasi server os,  
firewall dan disable module kernel)

## 1. Melihat Struktur Partisi

```bash
lsblk
```

Menampilkan disk, partisi, dan mount point yang tersedia pada sistem.

---

## 2. Memeriksa Partisi

```bash
cfdisk /dev/nvme0n1
```

Membuka utilitas untuk melihat struktur partisi pada disk NVMe.

---

## 3. Memeriksa Konfigurasi Linux LTS

```bash
nvim /etc/mkinitcpio.d/linux-lts.preset
```

Membuka file konfigurasi preset kernel Linux LTS.

---

## 4. Regenerasi Initramfs

```bash
mkinitcpio -P
```

Membuat ulang initramfs untuk seluruh kernel yang terpasang.

---

## 5. Memeriksa Kembali Partisi

```bash
lsblk
```

Memastikan struktur partisi dan volume telah terdeteksi dengan benar.

---

## 6. Mencoba Mount Home

```bash
mount /mnt/home
```

Mencoba me-mount partisi home ke direktori `/mnt/home`.

---

## 7. Masuk ke Sistem dengan Chroot

```bash
arch-chroot /mnt
```

Masuk ke sistem Linux yang berada pada direktori `/mnt`.

---

## 8. Verifikasi Mount Point

```bash
lsblk
```

Memeriksa kembali volume dan mount point yang aktif.

---

## 9. Keluar dan Reboot

```bash
exit
reboot
```

Keluar dari lingkungan chroot dan melakukan restart sistem.





## 1. Mengaktifkan Firewalld


systemctl enable firewalld
systemctl status firewalld
```

Mengaktifkan Firewalld dan memeriksa status service.

---

## 2. Memeriksa Zone Drop


firewall-cmd --info-zone=drop
```

Menampilkan informasi konfigurasi zone `drop`.

---

## 3. Memeriksa Zone Block


firewall-cmd --info-zone=block
```

Menampilkan informasi konfigurasi zone `block`.

---

## 4. Memeriksa Zone Public


firewall-cmd --info-zone=public
```

Menampilkan konfigurasi zone `public`.

---

## 5. Menghapus Service DHCPv6 Client dari Zone Public


firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
firewall-cmd --reload
```

Menghapus service `dhcpv6-client` dari zone `public` dan menerapkan perubahan.

---

## 6. Memeriksa Zone External


firewall-cmd --info-zone=external
```

Menampilkan konfigurasi zone `external`.

---

## 7. Menghapus Service SSH dari Zone External

```bash
firewall-cmd --zone=external --remove-service=ssh --permanent
firewall-cmd --reload
```

Menghapus service SSH dari zone `external`.

---

## 8. Memeriksa Zone Internal

```
firewall-cmd --info-zone=internal
```

Menampilkan konfigurasi zone `internal`.

---

## 9. Menghapus Service pada Zone Internal

```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `internal`.

---

## 10. Memeriksa Zone DMZ

```
firewall-cmd --info-zone=dmz
```

Menampilkan konfigurasi zone `dmz`.

---

## 11. Menghapus Service SSH dari Zone DMZ

```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
firewall-cmd --reload
```

Menghapus service SSH dari zone `dmz`.

---

## 12. Memeriksa Zone Work

```
firewall-cmd --info-zone=work
```

Menampilkan konfigurasi zone `work`.

---

## 13. Menghapus Service pada Zone Work

```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `work`.

---

## 14. Memeriksa Zone Home

```bash
firewall-cmd --info-zone=home
```

Menampilkan konfigurasi zone `home`.

---

## 15. Menghapus Service pada Zone Home

```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
firewall-cmd --reload
```

Menghapus service yang tidak diperlukan dari zone `home`.



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
