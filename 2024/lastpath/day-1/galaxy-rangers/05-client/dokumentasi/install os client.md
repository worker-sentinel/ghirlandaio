# Install OS Hardened LUKS-on-LVM - client

## Cek Partisi Sistem
> description : 
Menampilkan daftar seluruh perangkat blok (harddisk, SSD, flashdisk) beserta partisinya dalam bentuk pohon (tree).

> rationale : 
Untuk mengidentifikasi dengan pasti nama device target (misalnya /dev/sda, /dev/nvme0n1) agar tidak salah mengeksekusi perintah pada partisi yang berisi data penting.

```bash
lsblk

```
 
## Membuat Physical Volume
> description : 
Mengubah partisi mentah (raw partition) menjadi Physical Volume (PV) agar bisa dibaca oleh sistem LVM.

> rationale : 
LVM tidak bisa menggunakan partisi biasa secara langsung. Perintah ini memberikan "label" dan struktur metadata LVM pada partisi tersebut.

```bash
pvcreate /dev/[nama partisi]

```
 
## Membuat Volume Group
> description : 
Membuat kontainer besar bernama Volume Group (VG) yang wadahnya diambil dari PV yang baru dibuat.

> rationale : 
VG bertindak sebagai "harddisk virtual besar" yang nantinya bisa dipecah-pecah menjadi beberapa partisi logis secara dinamis tanpa terikat batasan partisi fisik tradisional.

```bash
vgcreate [nama grup] /dev/[nama partisi]

```
 
## Membuat Logical Volume 
> description : 
Memotong VG menjadi beberapa partisi logis (Logical Volume) dengan ukuran spesifik (-L) atau persentase sisa ruang (-l).

> rationale : 
> * Isolasi Data: Jika satu direktori penuh (misalnya /var/log karena diserang log flooding), sistem utama (/) tidak akan ikutan crash atau freeze.
> * Penerapan Hak Akses Ketat: Memisahkan /var, /home, dan /tmp memungkinkan kita memasang opsi keamanan khusus (seperti melarang eksekusi aplikasi ilegal) pada partisi tersebut saat di-mount.
```bash
lvcreate -L [size G | M] [nama grup] -n root
lvcreate -L [size G | M] [nama grup] -n vars
lvcreate -L [size G | M] [nama grup] -n vlog
lvcreate -L [size G | M] [nama grup] -n vaud
lvcreate -L [size G | M] [nama grup] -n vtmp
lvcreate -L [size G | M] [nama grup] -n home
lvcreate -l 50%FREE [nama grup] -n podman
lvcreate -l 50%FREE [nama grup] -n [nama home untuk internal]

```
 
## Cek Ulang Struktur Partisi
> description : 
Memeriksa kembali struktur blok pasca pembuatan LV.

> rationale : 
Memastikan semua Logical Volume (root, vars, vlog, dll) sudah tercipta dengan ukuran yang benar di bawah Volume Group sebelum masuk ke tahap pemformatan.

```bash
lsblk

```
 
## Format Partisi LVM
> description : 
Membuat sistem berkas (filesystem) EXT4 di setiap Logical Volume.

> rationale : 
EXT4 adalah filesystem yang stabil, mendukung jurnal (mengurangi risiko korupsi data saat mati lampu), dan mendukung penuh POSIX Access Control Lists (ACL) serta atribut keamanan yang dibutuhkan sistem operasi Linux tingkat tinggi.

```bash
mkfs.ext4 /dev/[nama grup]/root
mkfs.ext4 /dev/[nama grup]/vars
mkfs.ext4 /dev/[nama grup]/vlog
mkfs.ext4 /dev/[nama grup]/vaud
mkfs.ext4 /dev/[nama grup]/vtmp
mkfs.ext4 /dev/[nama grup]/home
mkfs.ext4 /dev/[nama grup]/podman

```
 
## Cek Tipe Filesystem 
> description : 
Menampilkan nama perangkat khusus bersama dengan jenis filesystem yang tertanam di dalamnya.

> rationale : 
Membantu mendeteksi partisi EFI bawaan komputer (yang biasanya bertipe vfat) agar tidak tertukar dengan partisi Linux.

```bash
lsblk -o name,fstype

```
 
## Format Partisi EFI Boot
> description : 
Memformat partisi boot menggunakan filesystem FAT32 (VFAT).

> rationale : 
Spesifikasi standar UEFI (Unified Extensible Firmware Interface) pada motherboard komputer modern wajib membaca partisi bootloader dalam format FAT32 agar sistem bisa melakukan booting.

```bash
mkfs.vfat -F32 /dev/[nama partisi boot]

```
 
## Mounting Direktori Sistem 
> description : 
Mengaitkan partisi root ke folder /mnt media instalasi.

> rationale : 
Agar kita bisa mengisi dan menginstal OS ke dalam harddisk tersebut.

```bash
mount /dev/[nama grup]/root /mnt
```

> description : 
Mengaitkan sub-direktori LVM ke folder tujuannya masing-masing dengan parameter keamanan (-o) yang ketat.

> rationale : 
> * nodev: Mencegah pembuatan atau penggunaan karakter khusus/perangkat blok di partisi ini (mencegah akses hardware ilegal).

> * nosuid: Mematikan fungsi Set User ID. Pengguna tidak bisa membuat skrip berbahaya yang otomatis berjalan dengan hak akses root.

> * noexec: Memblokir eksekusi file biner/skrip di partisi tersebut. Sangat vital untuk /var/tmp, /var/log/audit, dan /home agar malware yang terunduh tidak bisa dieksekusi.

```bash
mount --mkdir -o rw,uid=0,gid=0,fmask=0077,dmask=0077,relatime /dev/[nama partisi boot] /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/[nama grup]/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,relatime /dev/[nama grup]/podman /mnt/var/lib/containers

```

## Format Volume LUKS Terenkripsi
> description : 
Mengenkripsi partisi home internal menggunakan standar industri LUKS (Linux Unified Key Setup).

> rationale : 
Melindungi data sensitif pengguna di dalam direktori home dari pencurian fisik. Jika laptop/workstation hilang atau harddisk dicopot, data tidak akan bisa dibaca tanpa passphrase decryption.

```bash
cryptsetup luksFormat /dev/[nama grup]/[nama home internal]

```

## Install Package & Desktop Environment (Plasma)
> description : 
Memasang paket-paket dasar sistem operasi ke dalam direktori /mnt.

> rationale : 
> * linux-hardened: Menggunakan kernel yang sudah dimodifikasi dengan pengerasan keamanan ekstrem (mitigasi eksploitasi memori, pembatasan akses kernel space).

> * intel-ucode: Menyediakan pembaruan mikrokode mikroprosesor untuk menambal celah keamanan level hardware (seperti Spectre/Meltdown).

> * firewalld & podman: Menyediakan dinding pertahanan jaringan dan isolasi aplikasi kontainer yang aman.
```bash
pacstrap /mnt intel-ucode base linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 sudo pacman git wget curl neovim iwd firewalld openssh grep podman podman-compose asciinema

```

## Konfigurasi Awal Setelah Install
> Membuat berkas konfigurasi mounting otomatis (fstab) berdasarkan partisi yang sedang aktif di-mount menggunakan UUID (Universally Unique Identifier).

> rationale : 
Agar saat komputer dinyalakan, sistem tahu partisi mana saja yang harus di-mount otomatis dan di mana letaknya tanpa risiko tertukar nama drive.

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

> description : 
Memindahkan direktori /tmp ke dalam RAM (Virtual Memory/tmpfs) dengan batasan ukuran 512MB dan opsi keamanan ketat.

> rationale : 
Direktori /tmp sering menjadi target penyerangan tempat menaruh exploit script. Dengan tmpfs ditambah opsi noexec, skrip penyerang tidak akan bisa berjalan, dan seluruh file di /tmp otomatis terhapus bersih saat komputer dimatikan/restart.

```bash
echo "tmpfs /tmp tmpfs defaults,rw,nosuid,nodev,noexec,relatime,size=1G 0 0" >> /mnt/etc/fstab
```

> description : 
Menyalin konfigurasi jaringan dari media instalasi ke sistem baru.

> rationale : 
Memastikan konektivitas jaringan langsung aktif begitu sistem baru melakukan booting pertama kali.

```bash 
cp /etc/systemd/network/* /mnt/etc/systemd/network

```
 
## Masuk ke Lingkungan Chroot
> description : Mengubah direktori akar (root directory) operasi terminal saat ini ke direktori /mnt.

> rationale : 
Memungkinkan kita bekerja dan mengonfigurasi bagian dalam OS baru seolah-olah kita sudah masuk (booted) ke dalam OS tersebut.

```bash
arch-chroot /mnt

```
## Mengatur Identitas Host & Waktu
> description : 
Memberikan nama identitas unik pada komputer di jaringan.

```bash
echo [hostname] > /etc/hostname
```
> description : 
Mengatur zona waktu ke Waktu Indonesia Barat (WIB) dan menyinkronkannya ke jam fisik motherboard (BIOS/UEFI).

> rationale : 
Validitas waktu sangat krusial untuk akurasi pencatatan log audit keamanan (jika terjadi insiden, linimasa waktu di log harus tepat) serta untuk enkripsi SSL/TLS agar tidak error.

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc

```

## Menyetel Lokalisasi Bahasa
> description : 
Menghasilkan karakter lokal bahasa yang digunakan sistem (biasanya menggunakan en_US.UTF-8).

> rationale : 
Mencegah terjadinya kesalahan eror atau malfungsi tampilan teks pada aplikasi terminal berbasis CLI maupun GUI.

```bash
nvim /etc/locale.gen
locale-gen
locale > /etc/locale.conf
nvim /etc/locale.conf

```

## Setup Integrasi PAM Mount & Home Internal
> description : 
Menginstal modul PAM (Pluggable Authentication Modules) untuk manajemen mounting otomatis.

```bash
pacman -S pam_mount
```

> description : 
Membuka enkripsi LUKS untuk sementara waktu guna memformat bagian dalam partisi terenkripsi tersebut dengan sistem berkas EXT4.

> rationale : 
Kontainer enkripsi yang baru dibuat pada langkah 10 masih kosong; kita harus membuka "gemboknya" terlebih dahulu untuk membuat sistem berkas di dalamnya agar bisa diisi data.

```bash 
lsblk
cryptsetup luksOpen /dev/[nama grup]/[nama home internal] [device name]
mkfs.ext4 /dev/mapper/[device name]

```

## Manajemen Akun Pengguna Lokal 
> description : 
Membuat akun pengguna baru, menentukan direktori rumahnya, dan memberikan kata sandi.

```bash
mkdir /home/[nama folder]
useradd -d /home/[nama folder] [nama user]
passwd [nama user]
```
> description : 
Memberikan hak akses administratif kepada pengguna tersebut melalui perintah sudo.

> rationale : 
Demi keamanan, administrator tidak boleh bekerja langsung menggunakan akun root. Kita membuat user biasa yang memiliki privilese sudo terkontrol yang aktivitasnya tercatat dalam log audit.
```bash 
echo "[nama user] ALL = (ALL:ALL)" > /etc/sudoers.d/[nama user]
chown -R [nama user]:[nama user] /home/[nama folder]

```

## Konfigurasi Kaitan Berkas Autentikasi
> description : 
Mengonfigurasi berkas integrasi modul keamanan PAM.

> rationale : 
Mengonfigurasi agar sistem secara otomatis menggunakan kata sandi login pengguna untuk membuka (luksOpen) dan melakukan mounting partisi home internal terenkripsi secara transparan saat login, serta menutupnya kembali saat pengguna logout.

```bash
nvim /etc/security/pam_mount.conf.xml
nvim /etc/pam.d/system-login

```
## Masuk Mode Root
### install dekstop environment 

```bash
pacman -S plasma sddm pavucontrol wereclumber firefox kitty 
```
> description : 
Beralih ke identitas superuser (root) sepenuhnya.

> rationale : 
Memudahkan eksekusi beruntun untuk konfigurasi sistem tingkat rendah (seperti penguncian modul kernel) tanpa interupsi pengetikan sudo.

```bash
sudo su

``` 
## Pemasangan Bootloader & Aktivasi Layanan 
> description : 
Memasang bootloader bawaan systemd (systemd-boot) ke partisi EFI.

> rationale : 
systemd-boot dipilih karena kodenya jauh lebih ringkas, cepat, dan memiliki attack surface (celah keamanan) yang lebih kecil dibandingkan GRUB.

```bash
bootctl --path=/boot install
```
> description : 
Membangun ulang Initial RAM disk image (initramfs) untuk semua kernel yang terpasang.

> rationale : 
Karena kita menggunakan LVM dan enkripsi LUKS, kernel membutuhkan modul khusus (seperti encrypt dan lvm2) di dalam initramfs agar bisa mengenali dan membuka partisi harddisk saat pertama kali komputer dinyalakan.

```bash 
mkinitcpio -P
```
> description : 
Mengatur agar layanan jaringan (systemd-networkd), pengaturan Wi-Fi (iwd), keamanan dinding api (firewalld), dan akses remot aman (sshd) otomatis berjalan sejak komputer pertama kali dinyalakan.

> rationale : 
Memastikan workstation langsung terlindungi oleh Firewall sejak detik pertama booting.

```bash 
systemctl enable systemd-networkd
systemctl enable iwd
systemctl enable firewalld
systemctl enable sshd
sysytemctl enable sddm 
exit

```
## Prosedur Vendor Lenovo & Restart
> description : 
Melakukan instalasi bootloader ulang dari luar chroot yang diarahkan ke jalur absolut /mnt/boot.

> rationale : 
Beberapa seri NVMe/Firmware UEFI pada laptop Lenovo memiliki standar pembacaan NVRAM yang ketat; perintah dari luar chroot ini memastikan entri booting terdaftar secara permanen pada arsitektur UEFI Lenovo.

```bash
# Jalankan instruksi ini di luar lingkungan chroot (Khusus mesin Lenovo)
bootctl --path=/mnt/boot install
```

# Nyalakan ulang komputer workstation
> description : 
Memulai ulang komputer.

> rationale : 
Mengakhiri sesi instalasi dari Live ISO dan memuat sistem operasi baru yang sudah diperkeras (hardened) dari harddisk utama.

```bash 
reboot

```

