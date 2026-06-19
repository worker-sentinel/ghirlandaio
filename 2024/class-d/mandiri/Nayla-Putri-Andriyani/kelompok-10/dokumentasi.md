## Bagian 1 : INSTALASI OS UNTUK SERVER DAN OPERATOR

Keduanya menggunakan tahapan yang sama, yang berbeda hanyalah operator akan menginstall dekstop juga di tahap akhir. 

### 1. Cek Partisi 

```bash
lsblk
```

### 2. Enkripsi partisi dengan LUKS

```bash
cryptsetup luksFormat --sector-size=4096 /dev/[partisi root]
```

Buka partisi dan beri nama

```bash
cryptsetup luksOpen /dev/[partisi root] [nama]
```

### 3. Buat LVM

Buat physical volume 
```bash
pvcreate /dev/mapper/[nama]
```

Buat volume grup lalu verifikasi apakah vg sudah muncul
```bash
vgcreate [namavg] /dev/mapper/[nama]
vgs
```

Buat Logical Volume untuk root, vars, vlog, vaud, vtmp, home, dan podman 

```bash
lvcreate -L 10G fall -n root  
lvcreate -L 8G fall -n vars
lvcreate -L 1G fall -n vlog
lvcreate -L 1G fall -n vtmp
lvcreate -L 1G fall -n vaud
lvcreate -L 5G fall -n home
lvcreate -L 5G fall -n podman 
```

### 4. Lakukan format 

Format BOOT 
```bash
mkfs.vfat -F32 -n BOOT /dev/[partisiboot]
```

Format sisa logical volume
```bash
mkfs.ext4 /dev/fall/root
mkfs.ext4 /dev/fall/vars
mkfs.ext4 /dev/fall/vtmp
mkfs.ext4 /dev/fall/vaud
mkfs.ext4 /dev/fall/home
mkfs.ext4 /dev/fall/podman
```

### 5. Lakukan mounting

Cek terlebih dahulu dengan `lsblk` sebelum mounting.

1. Mount root terlebih dahulu
```bash
mount /dev/fall/root /mnt
```

2. Mount boot
```bash
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6
```

3. Mount sisa logical volume
```bash 
mount --mkdir -o rw,nodev,nosuid,relatime /dev/fall/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/fall/vtmp /mnt/tmp
mount --mkdir -o rw,nosuid,noexec,relatime /dev/fall/vlog /mnt/var/log
mount --mkdir -o rw,nosuid,noexec,relatime /dev/fall/vaud /mnt/var/log/audit
mount --mkdir -o rw,nosuid,relatime /dev/fall/home /mnt/home
mount --mkdir -o rw,nosuid,noexec,relatime /dev/fall/podman /mnt/var/lib/containers
```
Setelah itu cek dengan `lsblk`. 

### 6. Install package dasar yang dibutuhkan 

```bash
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers linux-firmware mkinitcpio lvm2 sudo curl neovim iwd firewalld pacman podman
```

### 7. Generate fstab dan copy konfigurasi network

```bash
genfstab -U /mnt > /mnt/etc/fstab 
cp /etc/systemd/network/* /mnt/etc/systemd/network   
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```

Cek apakah sudah berhasil dengan command `cat`
```bash
cat /mnt/etc/fstab 
```

### 8. Konfigurasi di dalam arch-chroot

Masuk ke dalam root
```bash 
arch-chroot /mnt                                                                                                                                      
```

Set localtime
```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime     
```

Set jam 
```bash
hwclock --systohc
```

Konfigurasi
```bash
nvim /etc/locale.gen   
```

Cari `en_US` dan hapus tanda tagar di depan keduanya. Setelah itu keluar `esc` + `:wq`

Generate locale
```bash
locale-gen
```

Masukkan locale ke dalam konfigurasi
```bash
locale > /etc/locale.conf
```

Masuk ke dalam /etc/locale.conf
```bash
nvim /etc/locale.conf
```

Ubah sehingga menjadi seperti berikut : 
```bash 
LANG=en_US.UTF-8                                                                                                                                                          LC_CTYPE="C.UTF-8"                                                                                                                                                        LC_NUMERIC="C.UTF-8"                                                                                                                                                      LC_TIME="C.UTF-8"                                                                                                                                                         LC_COLLATE="C.UTF-8"                                                                                                                                                      LC_MONETARY="C.UTF-8"                                                                                                                                                     LC_MESSAGES=                                                                                                                                                              LC_PAPER="C.UTF-8"                                                                                                                                                        LC_NAME="C.UTF-8"                                                                                                                                                         LC_ADDRESS="C.UTF-8"                                                                                                                                                      LC_TELEPHONE="C.UTF-8"                                                                                                                                                    LC_MEASUREMENT="C.UTF-8"                                                                                                                                                  LC_IDENTIFICATION="C.UTF-8"                                                                                                                                               LC_ALL=en_US.UTF-8     
```

Menambahkan User dan password
```bash
useradd -m starfall
passwd starfall
```

Memberi hak akses ke user sehingga user bisa melakukan sudo su
```bash
echo "starfall ALL=(ALL:ALL) ALL" > /etc/sudoers.d/starfall
```

Buat file directory 
```bash
mkdir /etc/cmdline.d   
```

Membuat file dengan `touch`, memakai tanda kurung kurawal karena kita bikin 2 file langsung.
```bash
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

Masukan konfigurasi ke file boot
```bash
echo "rd.luks.name=$(blkid -s UUID -o value /dev/nvme0n1p7)=star root=/dev/fall/root" > /etc/cmdline.d/01-boot.conf
```

Masukkan file ke conf
```bash
echo "rw" > etc/cmdline.d/02-misc.conf
```

Konfigurasi
```bash
nvim /etc/mkinitcpio.conf
```

Cari hooks yang tidak ada tagarnya lalu tambahkan `sd-encrypt lvm2`. Sehingga menjadi: 
```bash
HOOKS=(base systemd autodetect microcode modconf kms keyboard sd-encrypt lvm2 sd-vconsole block filesystems fsck) 
```

DISABLE KERNEL UNTUK SEMUA (KECUALI CLIENT KARENA MEREKA BEBAS)

FIREWALL 
Di internet itu ada berbagai ancaman, sehingga firewall yang akan memblokir mereka. Firewall benar-benar dipakai di internet, router wifi, dll. Di firewall ada beberapa zona; drop, blok, public, dmz, external, internal, home, dll. 
Drop : zona paling ketat. Setiap yang masuk ke sini bakal ditolak. Misal kalau ngeping ke sini maka ping nya bakal ditolak. Misal kek punya yang 6.6.6.6 nya departemen keamanan US
Blok : paketnya masuk tapi keblokir (analogi), datanya masuk tapi nggak diterima. 
Publik : Cuma orang-orang tertentu yang bisa masuk. Misal, di sebuah tempat publik kita cuman ngomong orang-orang yang mereka tahu.
Eksternal : Mirip-mirip publik, tapi kek baru ketemu sekali. Zona yang diluar zona internet. Misal, kita di kantor dan di dalam kantor itu ada 2 ip address dan beda router. Misal, di gedung fah kita kenal 1 orang dari lantai bsa. 
DMZ : demilitarized zone. Zona netral. 
Internal : Deket aja. 
Work : 
Home : Jaringan yang udah kita percaya banget. Misal, teman satu tim
Trusted : Paling percaya banget deh pokoknya. 
Ada list di zone itu service-service yang diizinin. Secara default diblok, namun diberikan izin oleh firewall.

COMMAND 

## Bagian 2 : Disable Kernel 

### 2.1 : Set Firewall 

#### 1. Cek zona-zona yang ada 

```bash
Firewall-cmd --list-all-zone
```

Jika tidak muncul zona, maka beri izin dan mulai firewalld dulu
```bash
Systemctl enable firewalld 
Systemctl start firewalld
```

#### 2. Menuju ke zona spesifik

Lalu ulang command pertama, nanti akan muncul zone-zone yang tersedia.
Jika ingin menuju ke zona yang spesiifik, maka gunakan command berikut. Kali ini kami akan menuju zona publik 

```bash
firewall-cmd --info-zone=public || (ini masuk ke public)
```

#### 3. Hapus service yang tidak terpakai

Hapus service yang tidak dipakai karena rawan menjadi celah keamanan.
```bash
	‘firewall-cmd --zone=public --remove-service=dhcpd6-client --permanent’ || Hapus semua selain ssh.
```

#### 4. Cek apa service sudah terhapus 

```bash
firewall-cmd --info-zone=public || cek dulu
```

Jika masih ada, reload firewall
```bash
firewall-cmd --reload 
```

Cek lag
```bashi 
firewall-cmd --info-zone=public
```

#### 5. Cek Zona lainnya

Lakukan langkah yang sama dengan zona lainnya yang tersedia. Setelah ini akan mengecek zona home

```bash
firewall-cmd --info-zone=home
```

Jika servicenya banyak dan ada lebih dari satu sert akan dihapus bersamaan, maka gunakan tanda kurung kurawal seperti berikut: 
```bash
firewall-cmd --zone=home --remove-service={dhcpd6-client,mdns,samba-client} --permanent
```
Ikuti cara yang sama untuk zona yang lainnya, seperti internal, eksternal, dmz, dll. Lalu reload firewall.

SET UP MODULE KERNEL 

### 2.2 : Set Up Module Kernel

Nonaktifkan module kernel yang tidak dipakai untuk memperkecil celah keamanan. 

#### 1. Buat file konfigurasi 
```bash
	sudo nvim /etc/modprobe.d/01-hard.conf
```

Isi dengan: 
```
install usb-storage /bin/false
blacklist usb-storage
```

Lalu esc :wq

```bash
sudo modprobe -r usb-storage
````

Lalu rebuild initramfs

```bash
mkinitcpio -P
```

Ctrl + d asciinema 
Lakukan reboot untuk semua cluster

Setelah ini fokus khusus ke server 
Instalasi Podman untuk Cluster Server 

Kalau mau tahu cara baterai di server 
	cat /sys/class/power_supply/BAT0/capacity || BAT nya beda-beda,  bisa BAT1 
 
## Bagian 3 : APLIKASI

### 3.1 : SET UP ROOTLESS PODMAN 

Agar tidak sudo su untuk memakai podman.
Pertama, aktifkan dulu rekaman.
```bash
Asciinema rec [nama].cast 
```
#### 1. Masuk ke root dan lakukan pengecekan

Masuk ke root
```bash
Sudo su
```
 
Verifikasi bahwa ada user dengan subuid 100000:65536
```bash
nvim /etc/subuid
```

Keluar esc :wq

Verifikasi grup 
```bash
nvim /etc/subguid
```

Keluar

#### 2. Berikan izin kepada podman

```bash
systemctl enable --global podman
```

Exit dari root
#### 3. Buat Konfigurasi Storage untuk Container 

Membuat direktori untuk folder dalam folder yang hanya ada di user. Untuk membuat hidden folders di linux tinggal ditambahkann titik di sebelumnya. Dijadikan hidden karena berkaitan dengan keamanan.

```bash
Mkdir -p .config/containers
```

Lakukan verifikasi 
```bash
Ls -la 
```

Verifikasi untuk semua yang ada di config
```bash
ls .config 
```

Membuat file konfigurasi storage
```bash
nvim .config/containers/storage.conf
```

Isi seperti ini 
```bash
[storage]
driver = “overlay”

[storage.options.overlay]
mount_program = “”
mountopt = “userxattr”
```

#### 4. Menambahkan registry

Untuk menambahkan repository penyimpanan image. Di dalam podman ada beberapa situs untuk mengunduh image, seperti dockerhub,quay,ghcr. 
```bash
sudo nvim /etc/containers/registries
```
 
Scroll paling bawah, lalu buat line baru
```bash
unqualified-search-registries = [“docker.io”]
```

Keluar esc :wq

Cek status Podman
```bash
	Systemctl status podman
	Sudo systemctl start podman
```

Ctrl + d untuk asciinema 

Reboot lalu cek status lagi 
```bash
Systemctl status podman
```

Kalau tidak aktif diulang saja 
```bash
Sudo systemctl enable podman
Systemctl status podman
Sudo systemctl status podman
```

### 3.2 : INSTALASI SLIMS DI SERVER TAPI REMOTE OLEH ADMIN
