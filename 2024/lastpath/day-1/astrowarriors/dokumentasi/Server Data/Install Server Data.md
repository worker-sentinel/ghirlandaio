# Install Server Data
## 1. Membagi Partisi
```bash
cfdisk /dev/nvme0n1
```
```bash
nvme0n1p6 = 3G EFI FILE SYSTEM
```
Sisanya untuk Linux File Sytem <br>
Digunakan untuk membuat, menghapus, atau mengubah partisi sebelum proses instalasi. CIS menyarankan pemisahan filesystem untuk meningkatkan keamanan sistem.
## 2. Mengecek Partisi
```bash
lsblk
```
Untuk menampilkan informasi seluruh perangkat partisi, serta Logical Volume yang dikenali oleh sistem. 
## 3. Menghubungkan ke Wifi
```bash
iwctl
```
Untuk menghubungkan sistem ke jaringan
## 4. Konfigurasi LUKS
```bash
cryptsetup luksFormat /dev/nvme0n1p7
```
Menginisialisasi partisi sebagai media penyimpanan terenkripsi menggunakan LUKS untuk melindungi kerahasiaan data serta mendukung penerapan mekanisme perlindungan media penyimpanan sesuai praktik keamanan yang direkomendasikan CIS.
```bash
cryptsetup luksOpen /dev/nvme0n1p7 proc
```
Membuka partisi LUKS sehingga dapat diakses melalui device mapper sebagai tahapan penggunaan media penyimpanan terenkripsi sebelum konfigurasi LVM.
## 5. Konfigurasi LVM
Membuat Physical Volume
```bash
pvcreate /dev/mapper/proc
```
Membuat Physical Volume sebagai dasar pengelolaan penyimpanan menggunakan LVM sehingga memudahkan penerapan filesystem terpisah sesuai rekomendasi CIS.
Membuat Volume Grup
```bash
vgcreate saw /dev/mapper/proc
```
Membuat Volume Group sebagai kumpulan ruang penyimpanan untuk Logical Volume sehingga direktori penting dapat dipisahkan sesuai kebutuhan keamanan sistem.
Membuat Logical Volume
```bash
lvcreate -L 8G saw -n root
lvcreate -L 5G saw -n vars
lvcreate -L 1G saw -n vlog
lvcreate -L 1G saw -n vaud
lvcreate -L 1.5G saw -n vtmp
lvcreate -l50%FREE saw -n home
lvcreate -l100%FREE saw -n podman
```
Membuat Logical Volume (LV) pada Volume Group (VG) saw dengan ukuran yang telah ditentukan.
- Logical Volume root digunakan sebagai filesystem utama (/) untuk menyimpan sistem operasi.
- Logical Volume vars digunakan sebagai filesystem /var untuk menyimpan data aplikasi dan sistem.
- Logical Volume vlog digunakan sebagai filesystem /var/log untuk menyimpan file log sistem.
- Logical Volume vaud digunakan sebagai filesystem /var/log/audit untuk menyimpan log audit sistem.
- Logical Volume vtmp digunakan sebagai filesystem /var/tmp untuk penyimpanan sementara.
- Logical Volume home menggunakan 50% sisa ruang penyimpanan sebagai filesystem /home untuk menyimpan data pengguna.
- Logical Volume podman menggunakan 100% sisa ruang penyimpanan untuk menyimpan data kontainer Podman.
Pemisahan filesystem seperti /var, /var/log, /var/log/audit, /var/tmp, dan /home mendukung rekomendasi CIS agar setiap filesystem dapat menerapkan kontrol keamanan secara terpisah, mengurangi dampak apabila salah satu filesystem penuh atau mengalami gangguan, serta memudahkan penerapan opsi mount yang aman.
## 6. Membuat Filesystem
```bash
mkfs.ext4 /dev/proc/root
mkfs.vfat -F32 /dev/nvme0n1p6
mkfs.ext4 /dev/proc/vars
mkfs.ext4 /dev/proc/vlog
mkfs.ext4 /dev/proc/vaud
mkfs.ext4 /dev/proc/vtmp
mkfs.ext4 /dev/proc/home
mkfs.ext4 /dev/proc/podman
```
Membuat filesystem pada setiap Logical Volume dan partisi agar dapat digunakan oleh sistem operasi
- mkfs.ext4 digunakan untuk membuat filesystem EXT4 pada Logical Volume root, vars, vlog, vaud, vtmp, home, dan podman karena EXT4 merupakan filesystem yang umum digunakan pada sistem Linux.
- mkfs.vfat -F32 digunakan untuk membuat filesystem FAT32 pada partisi EFI System Partition (ESP) agar dapat digunakan sebagai media boot yang kompatibel dengan sistem UEFI.
Proses pembuatan filesystem merupakan tahap persiapan sebelum filesystem di-mount dan digunakan oleh sistem operasi.
## 7. Mount Filesystem
```bash
mount /dev/saw/root
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,relatime /dev/saw/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/podman /mnt/var/lib/containers
```
- Melakukan mount setiap filesystem ke direktori tujuan agar dapat digunakan selama proses instalasi sistem.
- Opsi --mkdir digunakan untuk membuat direktori tujuan secara otomatis apabila belum tersedia.
- Partisi EFI System Partition (ESP) di-mount ke /mnt/boot dengan pengaturan izin (uid, gid, fmask, dan dmask) untuk membatasi akses ke file boot.
- Filesystem /var, /var/log, /var/log/audit, /var/tmp, /home, dan /var/lib/containers di-mount secara terpisah sesuai struktur Logical Volume yang telah dibuat.
- nodev digunakan untuk mencegah penggunaan device file pada filesystem sehingga mengurangi potensi penyalahgunaan perangkat.
- nosuid digunakan untuk mencegah penggunaan bit SUID/SGID, sehingga dapat mengurangi risiko privilege escalation.
- noexec digunakan untuk mencegah eksekusi program pada filesystem tertentu, seperti /var/log, /var/log/audit, dan /var/tmp, sehingga membantu mengurangi attack surface.
Penggunaan opsi mount seperti nodev, nosuid, dan noexec serta pemisahan filesystem sesuai dengan rekomendasi CIS untuk membatasi akses, meningkatkan keamanan filesystem, dan melindungi direktori yang menyimpan data penting maupun data sementara.
## 8. Install Package
```bash
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers Linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```
Perintah pacstrap digunakan untuk menginstal sistem dasar Arch Linux beserta paket-paket yang diperlukan ke direktori /mnt. Paket yang diinstal meliputi kernel Linux LTS, firmware, utilitas LVM, mkinitcpio, serta layanan seperti Firewalld, OpenSSH, dan iwd yang mendukung konfigurasi keamanan dan administrasi sistem.
## 9. Genfstab
```bash
genfstab -U /mnt > /mnt/etc/fstab
```
Perintah genfstab digunakan untuk membuat file /etc/fstab secara otomatis berdasarkan filesystem yang telah di-mount. Opsi -U menggunakan UUID (Universally Unique Identifier) sebagai identitas setiap filesystem sehingga proses mount tetap konsisten meskipun nama perangkat berubah. Penggunaan UUID mendukung konfigurasi filesystem dan meminimalkan kesalahan saat proses boot.
## 10. Mounting RAM
```bash
echo "tmpfs /tmp tmpfs deafults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
```
Perintah ini digunakan untuk menambahkan konfigurasi tmpfs pada direktori /tmp ke dalam file /etc/fstab sehingga direktori tersebut menggunakan RAM sebagai media penyimpanan sementara. Penerapan opsi nodev, nosuid, dan noexec sesuai dengan rekomendasi CIS untuk membatasi penggunaan device file, mencegah eksekusi program, serta mengurangi risiko privilege escalation dan attack surface pada direktori sementara.
```bash
cp /etc/systemd/network/* /mnt/etc/systemd/network
```
Perintah cp digunakan untuk menyalin konfigurasi systemd-networkd dari lingkungan instalasi ke sistem yang baru diinstal agar konfigurasi jaringan tetap tersedia setelah proses boot.
```bash
arch-chroot /mnt
```
Perintah arch-chroot digunakan untuk masuk ke lingkungan sistem yang baru diinstal sehingga seluruh konfigurasi selanjutnya diterapkan langsung pada sistem tersebut.
kalau berhasil tulisan root yang berwarna merah akan berubah menjadi putih
## 11. Konfigurasi System
Hostname
```bash
echo (nama computer) > /etc/hostname
```
Perintah echo digunakan untuk menetapkan hostname atau nama komputer dengan menuliskannya ke dalam file /etc/hostname.
Timezone
```bash
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
Perintah ln -fs digunakan untuk membuat symbolic link dari file zona waktu Asia/Jakarta ke /etc/localtime sehingga sistem menggunakan zona waktu yang sesuai dengan lokasi server.
```bash
nvim /etc/locale.gen
```
hapus # en_US<br>
Perintah nvim digunakan untuk membuka dan mengubah file /etc/locale.gen. Pada tahap ini, tanda # pada locale en_US.UTF-8 UTF-8 dihapus untuk mengaktifkan locale yang akan digunakan oleh sistem.
Locale
```bash
locale-gen
```
Digunakan untuk menghasilkan (generate) locale berdasarkan konfigurasi yang telah diaktifkan pada file /etc/locale.gen.
```bash
locale > /etc/locale.conf
```
Digunakan untuk menampilkan konfigurasi locale yang sedang aktif, kemudian hasilnya disimpan ke dalam file /etc/locale.conf sebagai konfigurasi locale sistem.
```bash
nvim /etc/locale.conf
```
Digunakan untuk membuka dan mengubah file /etc/locale.conf agar konfigurasi locale sesuai dengan kebutuhan sistem.
## 12. Konfigurasi User
```bash
useradd -m (nama user)
```
Digunakan untuk membuat nama pengguna baru beserta home directory
```bash
passwd (nama user)
```
Digunakan untuk membuat kata sandi pengguna
```bash
echo "(nama user) ALL=(ALL:ALL) ALL" > /etc/sudoers.d/(nama user)
```
Perintah echo digunakan untuk membuat file konfigurasi pada direktori /etc/sudoers.d yang memberikan hak akses sudo kepada pengguna.
Uji coba
```bash
su (nama user)
```
Jika berhasil nama root akan berubah menjadi nama user
```bash
sudo su
```
Masukkan Password
## 13. Konfigurasi Kernel
Membuat direktori
```bash
mkdir /etc/cmdline.d
touch /etc/cmdline.d/{01-boot.conf,02-nisc.conf}
```
- Perintah mkdir digunakan untuk membuat direktori /etc/cmdline.d sebagai lokasi penyimpanan file konfigurasi parameter kernel.
- perintah touch digunakan untuk membuat file 01-boot.conf dan 02-nisc.conf yang akan berisi parameter kernel saat proses boot.
Cek
```bash
lsblk
```
Konfigurasi boot
```bash
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p7)=proc root=/dev/saw/root" > /etc/cmdline.d/01-boot.conf
```
Perintah echo digunakan untuk menuliskan parameter kernel ke dalam file 01-boot.conf. 
Parameter tersebut mengarahkan sistem agar membuka partisi LUKS berdasarkan UUID dan menggunakan Logical Volume root sebagai filesystem utama saat proses boot.
Memastikan isi file
```bash
cat /etc/cmdline.d/01-boot.conf
echo rw > /etc/cmdline.d/02-nisc.conf
```
- Perintah cat digunakan untuk menampilkan isi file 01-boot.conf sebagai proses verifikasi bahwa parameter kernel telah ditulis dengan benar sebelum digunakan pada proses boot.
- Perintah echo digunakan untuk menambahkan parameter rw ke dalam file 02-nisc.conf sehingga root filesystem di-mount dalam mode read-write saat sistem melakukan boot.
## 14. Konfigurasi Initramfs
```bash
nvim /etc/mkinitcpio.conf
```
Perintah nvim digunakan untuk membuka file konfigurasi /etc/mkinitcpio.conf dan menyesuaikan HOOKS yang digunakan saat proses pembentukan initramfs.
Masuk ke HOOKS, tambahkan
```bash
sd-encypt lvm2
```
Pada bagian HOOKS ditambahkan sd-encrypt dan lvm2 agar sistem dapat membuka partisi LUKS dan mengenali LVM
Edit preset
```bash
nvim /etc/mkinitcpio.d/linux-lts.preset
```
Digunakan untuk membuka file preset mkinitcpio pada kernel Linux LTS dan menyesuaikan konfigurasi yang digunakan saat proses pembentukan initramfs.
## 15. Install Sytem.d Boot
```bash
bootctl --path=/boot install
```
Digunakan untuk menginstal systemd-boot pada EFI System Partition sehingga sistem memiliki bootloader yang digunakan untuk memuat kernel Linux saat proses boot
```bash
mkinitcpio -P
```
Digunakan untuk membangun kembali seluruh initramfs berdasarkan konfigurasi yang telah dibuat.
## 16. Mengaktifkan Service System
```bash
systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd
```
Perintah systemctl enable digunakan untuk mengaktifkan layanan agar berjalan secara otomatis saat sistem melakukan boot. Layanan yang diaktifkan meliputi systemd-networkd untuk konfigurasi jaringan, systemd-resolved untuk resolusi DNS, iwd untuk koneksi jaringan nirkabel, firewalld untuk pengelolaan firewall, dan sshd untuk akses administrasi jarak jauh menggunakan SSH.
Pengaktifan firewalld dan OpenSSH mendukung penerapan kontrol keamanan yang direkomendasikan oleh CIS, dengan tetap memastikan hanya layanan yang diperlukan yang diaktifkan.
Keluar dari chroot
```bash
exit
reboot
```
- Digunakan untuk keluar dari lingkungan chroot dan kembali ke lingkungan instalasi
- Reboot digunakan untuk memulai ulang sistem agar seluruh konfigurasi yang telah dilakukan diterapkan pada sistem yang baru diinstal.
## 17. Konfigurasi Jaringan Setelah Instalasi
```bash
nvim /etc/resolv.conf
```
Digunakan untuk membuka file /etc/resolv.conf dan menambahkan alamat DNS Server yang akan digunakan oleh sistem.
Tambahkan
```bash
nameserver 8.8.8.8
nameserver 1.1.1.1
```
Restart systemd
```bash
systemctl restart systemd-networkd
systemctl restart systemd-resolved
```
Perintah systemctl restart digunakan untuk memulai ulang layanan systemd-networkd dan systemd-resolved agar konfigurasi jaringan dan DNS yang baru diterapkan tanpa perlu melakukan reboot sistem.
Konfigurasi iwd
```bash
nvim /etc/iwd/main.conf
```
Digunakan untuk membuka file konfigurasi iwd agar layanan jaringan dapat dikonfigurasi.
Isi file
```bash
[general]
EnableNetworkConfiguration=true
```
Konfigurasi tersebut mengaktifkan fitur Network Configuration pada iwd sehingga layanan dapat mengelola konfigurasi jaringan secara otomatis.
Restart iwd
```bash
systemctl restart iwd
```
Digunakan untuk memulai ulang layanan iwd agar perubahan konfigurasi diterapkan. 
Uji Koneksi
```bash
ping 8.8.8.8
```
Digunakan untuk menguji konektivitas jaringan dengan mengirimkan ke alamat IP 8.8.8.8.
