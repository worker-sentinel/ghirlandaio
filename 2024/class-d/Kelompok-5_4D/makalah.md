# INSTALASI BASE ARCH LINUX

## Dosen Pengampu : 
Al Muhdil Karim, S.IP., M.Hum.

## Disusun oleh kelompok 5 : 
1. Fira Lasya Nova  12402051010023
2. Nuraini As Syifa 12402051030088
3. Sri Mulyani Khairiyah 12402051050154

## Pendahuluan
Arch Linux adalah sistem operasi Linux berbasis biner dimana paket-paket aplikasi didistribusikan dalam bentuk telah berkompilasi. Sistem Operasi ini cocok untuk pengguna yang mengontrol penuh sistem operasi dan konfigurasi mereka. Sistem Operasi ini tidak memerlukan tambahan atau modifikasi apapun karena berbasis sederhana.

Arch Linux mendefinisikan kesederhanaan sebagai tidak ada tambahan ataupun modifikasi yang tidak perlu. Sehingga piranti lunak dirilis berdasarkan apa yang pembuat aslinya inginkan walaupun ada modifikasi yang sangat minim supaya dapat berjalan di Arch Linux. Contohnya adalah modifikasi (umumnya berbentuk berkas patch) untuk memperbaiki error yang belum disetujui oleh pembuat/pengurus piranti lunak. 

## Pembahasan
**1. Persiapan File Instalasi  Arch Linux**

Tahap pertama dimulai dengan mendapatkan file instalasi Arch Linux. Untuk mengunduh file ISO atau gambar Netboot sesuai metode boot yang dipilih, pengguna harus mengunjungi halaman unduhan resmi Arch Linux. Untuk memastikan bahwa file yang diunduh asli dan tidak diubah oleh pihak yang tidak bertanggung jawab, disarankan untuk memverifikasi tanda tangan file menggunakan Protocol Group Protocol (PGP). Verifikasi dapat dilakukan dengan menggunakan perintah:

```pacman-key -v archlinux-version-x86_64.iso.sig```

**2. Persiapan Media Instalasi**

Setelah mendapatkan file instalasi, langkah berikutnya adalah menyediakan media untuk instalasi. Setelah media USB flashdisk dipasang pada perangkat, pengguna dapat memulai booting ke lingkungan live Arch Linux. Untuk melakukan ini, media USB flashdisk digunakan sebagai media booting. 

**3. Proses Booting ke Lingkungan Live Arch Linux**

Setelah media instalasi siap, pengguna dapat masuk ke menu boot dengan menekan tombol tertentu selama proses POST, seperti FN + F12 untuk perangkat yang digandakan. Untuk memulai instalasi Arch Linux, pilih opsi install medium.

**4. Konfigurasi Koneksi Jaringan**

Setelah memasuki lingkungan live Arch Linux dengan sukses, langkah berikutnya adalah memastikan bahwa ada koneksi internet. Selama proses instalasi, mengunduh paket sistem memerlukan koneksi internet. Untuk memeriksa perangkat jaringan, gunakan perintah: ip a atau ip link. Jika menggunakan jaringan Wi-Fi, dapat menyelesaikan proses penyambungan menggunakan utilitas iwctl dengan langkah-langkah berikut: 

```iwctl```

```device list```

```station wlan0 scan```

```station wlan0 get-networks```

```station wlan0 connect nama_wifi```

**5. Pengujian Koneksi Jaringan**

Setelah terhubung ke jaringan, Anda dapat menguji koneksi menggunakan perintah: 

```ping archlinux.org```

Jika perangkat menerima balasan (tanggapan), maka koneksi internet telah aktif

**6. Sinkronisasi Waktu Sistem**

Selanjutnya sistem perlu disinkronkan waktunya untuk menghindari kesalahan verifikasi paket maupun sertifikat TLS dengan perintah: 

```timedatectl```

**7. Identifikasi Media Penyimpanan**

Tahap selanjutnya adalah mengidentifikasi media penyimpanan yang tersedia pada sistem dengan menggunakan perintah seperti: 

```lsblk```

atau

```fdisk -1```

Perintah ini akan menampilkan struktur partisi perangkat penyimpanan yang tersedia.

**8. Pembuatan Partisi Disk**

Penggunaan cfdisk dapat digunakan untuk memulai proses partisi setelah disk dikenali dengan perintah:

```cfdisk /dev/nvme0n1```

Dalam proses ini, beberapa partisi dibuat dengan pembagian sebagai berikut: Partisi Boot: 1 GB, Partisi Swap: 4 GB, Partisi Root: lebih dari 14 GB.

**9. Format Sistem File Partisi**

Setelah pembuatan partisi selesai, langkah selanjutnya adalah mengubah format partisi menggunakan sistem file yang sesuai. Untuk partisi root, format Ext4 digunakan dengan perintah:

```mkfs.ext4 /dev/nvme0n1p7```

```mkfs.ext4 /dev/nvme0n1p6```

Sedangkan partisi swap menggunakan perintah:

```swapon /dev/nvme0n1p6```

```mkfs.fat -F 32 /dev/nvme0n1p5```

**10. Pemasangan (Mount) Partisi** 

Sebelum sistem diinstal, format harus diselesaikan dan partisi harus dipasang dengan perintah:

```mount /dev/nvme0n1p7 /mnt```

```mount --mkdir /dev/nvme0n1p5 /mnt/boot```

**11. Instalasi Paket Dasar Sistem**

Tahap selanjutnya adalah instalasi paket dasar sistem dengan pacstrap dengan perintah:

```pacstrap -K /mnt base base-devel linux linux-firmware nvim amd-ucode```

paket amd-ucode dapat diubah menjadi intel-ucode jika prosesor intel digunakan.

**12. Pembuatan File System Table (fstab)**

Setelah paket berhasil diinstal, sistem file table (fstab) dibuat untuk memastikan bahwa partisi terpasang secara otomatis saat booting dengan perintah:

```genfstab -U /mnt >> /mnt/etc/fstab```

**13. Konfigurasi Sistem Baru**

Setelah itu, gunakan ini untuk mengakses lingkungan sistem yang baru diinstal dengan perintah:

```arch-chroot /mnt```

Konfigurasi kompleks, seperti pengaturan zona waktu, dimasukkan ke dalam sistem baru ini dengan perintah:

```ln -sf /usr/share/zoneinfo/Area/Location /etc/localtimehwclock --systohc ```

Selanjutnya, sesuai kebutuhan pengguna, konfigurasi bahasa (locale), keyboard, hostname, dan jaringan dilakukan.

**14. Konfigurasi Keamanan Sistem**

Pengguna harus membuat kata sandi untuk akun root untuk meningkatkan keamanan sistem.

**15. Instalasi Bootloader dan Keluar dari Chroot**

Setelah konfigurasi lengkap selesai, pengguna harus memasang bootloader sesuai dengan skema partisi yang dipilih. Setelah pemasangan bootloader selesai, tahap terakhir dari proses instalasi adalah keluar dari lingkungan chroot. Lakukan dengan perintah:

```exit```

atau menggunakan kombinasi tombol Ctrl +D.

**16. Melepaskan (Unmount) Partisi**

Setelah keluar dari lingkungan chroot, pengguna dapat melepaskan partisi secara keseluruhan dengan perintah:

```unmount -R /mnt```

**17. Restart Sistem**

Meskipun langkah ini adalah opsional, langkah ini dapat membantu menemukan partisi yang masih digunakan (dikenal sebagai partisi sibuk) sehingga penyebabnya dapat diperiksa lebih lanjut. Kemudian sistem dapat dihidupkan ulang dengan perintah:

```reboot```

**18. Proses Boot Awal dan Login Sistem**

Systemd akan melepas partisi yang masih terpasang selama proses restart. Setelah komputer kembali menyala, media instalasi, seperti flashdisk, harus dilepas agar sistem dapat memulai boot dari media penyimpanan utama. Dengan menggunakan akun root yang telah dibuat sebelumnya, pengguna dapat mengakses sistem Arch Linux yang telah diinstal sebelumnya.

## Penutup
Secara keseluruhan, Arch Linux adalah distribusi Linux yang menawarkan fleksibilitas dan kustomisasi tanpa batas. Fleksibilitas dan kemampuan untuk mengontrol sepenuhnya sistem operasi adalah salah satu manfaat utama dari Arch Linux. Pengaturan yang terselubung dan kustomisasi tingkat lanjut memastikan bahwa sistem dapat disesuaikan sepenuhnya berdasarkan kebutuhan kamu.

## Daftar Pustaka
https://wiki.archlinux.org/title/Installation_guide
https://wiki.archlinux.org/title/Arch_Linux_(Bahasa_Indonesia)
https://it.telkomuniversity.ac.id/pengertian-sejarah-kelebihan-dan-kekurangan-arch-linux-os/
https://blog.dewaweb.com/apa-itu-arch-linux-pengertian-kelebihan-kekurangannya/
