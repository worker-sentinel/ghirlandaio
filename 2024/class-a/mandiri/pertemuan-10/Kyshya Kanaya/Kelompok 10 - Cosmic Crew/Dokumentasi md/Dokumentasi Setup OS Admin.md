# Setup OS Admin

```
sudo su
```
Masuk ke user root agar semua perintah berikutnya berjalan dengan hak akses penuh.

```
ls
```
Cek isi direktori saat ini untuk memastikan posisi yang benar.

```
nvim /etc/fstab
```  
Ubah baris berikut:
```
/var/lib/docker  →  /var/lib/containers
```
Karena kita menggunakan Podman (bukan Docker), direktori storage container harus disesuaikan.

```
mkinitcpio -P
```
Regenerasi semua preset initramfs setelah perubahan fstab. Flag `-P` berarti semua preset diproses sekaligus.

```
nvim /etc/mkinitcpio.d/default.conf
```
Buka file konfigurasi initramfs. Ubah urutan HOOKS:
```
# SEBELUM (salah):
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole lvm2 sd-encrypt block filesystems fsck)

# SESUDAH (benar):
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-vconsole sd-encrypt lvm2 block filesystems fsck)
```
`sd-encrypt` harus berada **sebelum** `lvm2` agar disk terenkripsi berhasil didekripsi sebelum LVM mencoba membaca volume di dalamnya. Jika urutannya terbalik, sistem akan gagal boot.

```
mkinitcpio -P
```
Regenerasi ulang initramfs setelah urutan HOOKS diperbaiki agar perubahan diterapkan.

```
lvrename /dev/openkm/dock /dev/openkm/pod
```
Rename logical volume `dock` menjadi `pod` di dalam volume group `openkm`. Penamaan `pod` disesuaikan untuk keperluan Podman.

```
lsblk
```
Jalankan lagi untuk memverifikasi nama logical volume sudah berubah menjadi `pod`.

```
mount --mkdir -o rw,nosuid,nodev,relatime /dev/openkm/pod /mnt/var/lib/containers
```

```
nvim /etc/fstab
```
Buka fstab untuk menambahkan entry mount permanen logical volume `pod` agar otomatis ter-mount saat reboot. Setelah dibuka, ketik `:q` untuk keluar tanpa mengubah apapun (pada sesi ini hanya verifikasi).

```
mkinitcpio -P
```
Regenerasi initramfs sekali lagi untuk memastikan semua konfigurasi terbaru sudah masuk ke dalam image boot.
