# Disable Module

> Pada tahap ini dilakukan disable kernel module yang tidak terpakai demi hardening sistem untuk mengurangi attack surface.

## Masuk Mode Root
```
sudo su
```
> Masuk ke mode root agar semua command setelahnya berjalan dengan privilege admin tanpa perlu mengetik "sudo" berkali-kali.

## Mengecek Status Module Kernel
> Untuk mengeceknya, gunakan command:
```
lsmod | grep [nama_module]
```
> Nama-nama module sebagai berikut:
1. cramfs
2. freevxfs
3. hfs
4. hfsplus
5. jffs2
6. overlay
7. squashfs
8. udf
9. irewire-core
10. usb-storage

> Jadi, gunakan command yang sama tapi sesuaikan dengan nama module yang hendak dicek.

> Contoh:
```
lsmod | grep cramfs
lsmod | grep freevxfs
lsmod | grep hfs
lsmod | grep hfsplus
lsmod | grep jffs2
lsmod | grep overlay
lsmod | grep squashfs
lsmod | grep udf
lsmod | grep irewire-core
lsmod | grep usb-storage
```
> Apabila setiap command tidak ada output apa-apa, artinya module tersebut memang tidak aktif. Tapi untuk usb-storage, meskipun tidak ada output apapun, harus tetap diblokir karena usb-storage bisa berpotensi mengalami serangan begitu ada USB yang tertancap sehingga ia akan otomatis aktif secara sendirinya sehingga perlu diblokir. Caranya dengan mengetik command:
```
sudo nvim /etc/modprobe.d/01-custom.conf
```
> Command tersebut untuk membuka/ membuat file konfigurasi modprobe dengan menggunakan editor nvim. File di folder ```/etc/modprobe.d/``` itu adalah tempat kernal membaca aturan mengenai module mana yang boleh atau tidak boleh di-load. Untuk ```01-custom``` merupakan nama file jadi bebas untuk menamai file nya apa asal diakhirnya ditambahkan ```.conf```

> Setelah masuk tampilan nvim, pada baris pertama ketik:
```
install usb-storage /bin/false
```
> Guna nya untuk disable usb-storage tersebut.

> Kemudian di baris kedua ketik:
```
blacklist usb-storage
```
> Guna nya untuk nge-blacklist module-nya.

> Setelah selesai, tekan tombol ```Esc (Escape)``` untuk keluar dari mode ```Insert (I)``` kemudian ketik ```:wq (write & quit)``` untuk menyimpan perubahan.

```
sudo mkinitcpio -P
```
> Untuk rebuild ulang initramfs-nya agar versi terbarunya sudah include aturan blacklist yang sudah ditulis/ketik.
