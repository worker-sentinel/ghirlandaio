# Penugasan Kelompok 7 
## Nama: Hanifah Dina Pasha
## NIM: 12402051050145
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S.IP., M.Hum

## Analisis
Resume ini disusun untuk mengetahui dan memahami aplikasi pada sistem Linux

## Konteks
Resume ini dibuat berdasarkan penjelasan dari Pak Al Muhdil Karim

## Booster
Booster merupakan alat untuk membuat initramfs yang bekerja lebih cepat dan lebih ringan dibandingkan mkinitcpio maupun Dracut. Sementara itu, initramfs Adalah sistem berkas sementara yang dimuat oleh kernel pada tahap awal proses booting sebelum sistem berkas utama (root filesystem) dipasang. Booster memiliki peran penting dalam penerapan enskripsi disk karena mampu mendukung proses pembukaan partisi Luks saat booting dengan konfigurasi yang lebih sederhana serta pembuatan image yang lebih efisien.

## LUKS on LVM
Pada Luks on LVM, LVM dibuat terlebih dahulu sebelum setiap volume dienkripsi secara terpisah menggunakan kunci yang berbeda. Pendekatan ini memberikan Tingkat isolasi keamanan yang lebih rinci karena setiap volume memliki control akses masing-masing, sehingga lebih cepat digunakan pada sistem yang menyimpan data sensitive dengan kebutuhan pengamanan per volume.

## Super File
Super file merupakan aplikasi pengelola berkas yang dapat dijalankan melalui terminal. Aplikasi ini memiliki fungsi yang mirip dengan windows explorer, tetapi digunakan pada lingkungan CLI. Dengan super file, administrator dapat mengakses, memindahkan, menyalin, dan mengelola file tanpa memerlukan tampilan grafis.

## CIS
CIS merupakan standar keamanan yang berisi panduan dalam melakukan konfigurasi sistem agar lebih aman. Panduan tersebut mencakup berbagai aspek, seperti ketentuan Panjang minimal password, pengaturan port yang diperbolehkan, pengamanan modul kernel, dan konfigurasi layanan server. Penetapan standar CIS bertujuan menciptakan tingkat keamanan yang seragam sehingga sistem dapat dikelola dengan lebih baik dan Risiko keamanan dapat diminimalisir.

## Podman
Podman merupakan container runtime yang bekerja tanpa daemon serta mendukung penggunaan dalam mode rootless, sehingga dianggap lebih aman dibandingkan docker untuk diterapkan pada lingkungan server produksi. Podman memungkinkan berbagai layanan, seperti MySQL maupun SLiMS, dijalankan secara bersamaan di dalam container yang terisolasi tanpa menyebabkan konflik port. Selain itu, integrasinya dengan system memungkinkan container dijalankan secara otomatis ketika server dinyalakan.

## Iptables
Iptables merupakan utilitas firewall berbasis Netfilter pada linux yang digunakan untuk mengatur lalu lintas jaringan dengan menentukan port, alamat IP, dan protocol yang diizinkan. Port yang tidak diperlukan diblokir, sedangkan port penting dibuka sesuai kebutuhan.

## MPD, MPC, MPV
MPD merupakan server music yang berjalan sebagai daemon untuk mengelola  Pustaka dan pemutaran audio, sedangkan MPC berfungsi sebagai klien berbasis terminal untuk mengendalikan MPD. MPV melengkapi keduanya sebagai pemutaran media yang mendukung berbagai format audio, video dan gambar.

## Keepass
Dalam pengelolaan server, administrator dianjurkan untuk menggunakan password yang berbeda pada setiap layanan yang dikelola. Untuk mempermudah pengelolaan password, dapat menggunakan aplikasi keepass. Aplikasi ini berfungsi menyimpan berbagai password secara aman dalam database yang terenkripsi sehingga dapat mengurangi Risiko lupa password maupun penggunaan password yang sama pada beberapa akun.

## Secret 
Secret merupakan aplikasi yang digunakan untuk menyimpan kunci autentikasi secara aman. Dengan adanya secrets, pengguna tidak perlu memasukan SSH key secara manual setiap kali mengakses server. Penggunaan aplikasi ini dapat mempermudah proses login, mengotomasi autentifikasi SSH, serta meningkatkan keamanan akses ke server.

## Openssh
Merupakan implementasi protocol SSH yang digunakan untuk mengakses computer atau server lain secara aman melalui jaringan. 
