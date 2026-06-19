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
	Asciinema rec [nama].cast 
Cari ip di server dulu
	Ip a 

Di keduanya : 
	Sudo pacman -S openssh
	Sudo systemctl enable sshd
	Systemctl start sshd

Di admin : 
	Ssh [namauserserver]@[ip address server]
Install podman compose
Kenapa? Untuk menjalankan container dengan menggunakan file docker compose. buat satuin file-file yang dibutuhkan instalasi slims di satu file. 
	Sudo pacman -S podman-compose 
Kalau ditanyain password, masukin password si server
	Cd .config/containers
Download link zip yang disiapkan oleh developer slims 
	Sudo pacman -S wget 
	‘wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip’
Cek apakah master.zip ada atau tidak
	Ls
Unzip file
	Sudo pacman -S unzip
	unzip master.zip
Cek lagi 
	Ls 
(biru artinya folder)
Masuk ke folder 
	Cd docker-compose-for-slims-master
Cek isinya 
	Ls 
Di arch linux ada port, jalur-jalur yang digunakan aplikasi untuk berinteraksi. Batasan untuk user biasa adalah 1024 ke atas, port ke bawahnya digunakan untuk aplikasi-aplikasi krusial. 
	
Mengatur bahwa port 80 adalah milik user 
	Sudo nvim /etc/sysctl.d/99-custom.conf
Isi seperti di bawah 
	net.ipv4.ip_unprivileged_port_start=80
Keluar esc :wq
Restart konfigurasi yang dibuat
	Sudo sysctl --system
Cek 
	Ls
Gunakan file docker-compose-redis.yaml 
Pada defaultnya baca yang docker compose dulu, jadi harus direname
	Mv docker-compose.yaml docker-compose.yaml.back
Ubah docker-compose-redis.yaml menjadi docker-compose.yaml
	Mv docker-compose-redis.yaml docker-compose.yaml

	Nvim docker-compose.yaml
Hapus tagar yang terletak di sebelum ports, hapus juga tagar di line setelahnya
Lalu tambahkan hingga “127.0.0.1:3306:3306”
Lakukan langkah yang sama di port redis.
Hapus line yang ada ipv4 || pencet saja esc lalu dd 
||Shortcut :  kalau mau undo u || kalau copy itu yy || paste p || kalau blok v || 


HAK AKSES WRITE, READ, EXECUTE
### 3.1 :
READ ditandai dengan angka (4), WRITE (2), EXECUTE (1) Jadilah 4+2+1 = 7

USER punya akses WRITE, READ, EXECUTE
GRUP punya akses WRITE, READ, EXECUTE
OTHERS punya akses WRITE, READ, EXECUTE

sudo chmod -R 777 app/slims
podman compose up -d

…
Sudo Firewall-cmd –zone=public –add-port=80/tcp –permanent
Sudo Firewall-cmd reload

Lalu masuk ke firefox > masuk ke slims
Caranya harus tahu masuk ip servernya 
Buka browser
http://(ip servernya):80

NETWORK
Client data itu buat mariadb
Redis buat cache 
===

## Bagian 4 : Network 

### 4.1 : Tentukan IP

Buka terminal admin > kita tentukan dua user. Satu user admin, satu user operator. 
IP adress admin, operator, server, wifi (wifi buat sendiri di laptop server)

IP adress admin: 199.19.9.7
IP address operator: 199.19.9.9
IP address server: 199.19.9.8
IP address wifi: 199.19.10.1
IP address gateway: 199.19.9.1

### 4.2 FOKUS UNTUK USER ADMIN
	Useradd -m startop
	Sudo passwd startop :1234

#### 1. BIKIN KONEKSI UNTUK ADMIN
Mencari interface:

```bash
nmcli device status
sudo nmcli connection add type ethernet if name eno1 con-name ”admin” ipv4.method manual ipv4.adress 199.19.9.7/24 ipv4.gateway 199.19.9.1 ipv4.dns 8.8.8.8
```
	
#### 2. BIKIN KONEKSI UNTUK OPERATOR
```bash
sudo nmcli connection add type ethernet if name eno1 con-name ”operator” ipv4.method manual ipv4.adress 199.19.9.9/24 ipv4.gateway 199.19.9.1 ipv4.dns 8.8.8.8
```

#### 3. USER ADMIN OPERATOR

```bash
	sudo su
sudo echo
	echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user operator]/.bash_profile
	echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user operator]/.bash_profile
```

```bash
echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user admin]/.bash_profile
echo “nmcli connection up [nama koneksi yang dibuat]” >> /home/[nama user admin]/.bash_profile
cat /home/starfall/.bash
```

#### 4. SAMBUNGKAN KABEL LAN DARI LAPTOP ADMIN KE LAPTOP SERVER
(lihat asciinema di laptop admin)

#### 5. SETUP SSH SERVER 
(lihat asciinema di laptop admin)

```bash
systemctl stop firewalld
systemctl restart iwd
systemctl start firewalld
```

[di laptop admin]
```bash
sudo su 
[masukkan password] pw: 123
nvim /etc/systemd/network/20-ethernet [pencet tab aja biar cepet]
Systemctl restart sytemd-networkd 
```

[colok kabel lan]
Cari ip 

```bash
Ip a
```

Masuk ke nvim lagi 
```bash
nvim /etc/systemd/network/20-ethernet [pencet tab aja biar cepet]
```

Konfigurasi Set Up Router

```bash
ssh [namauserserver]@[ip address server]
sudo su 
pacman -S hostapd
nvim /etc/hostapd/hostapd.conf
```

Search menuju ke line 3200, scroll lagi sampai ke line terbawah. Masuk ke inspection (i)
Isi : 

```bash
interface=wlan0
driver=nl80211
ssid=jatoh [nama wifi]
hw_mode=g
channel=7
auth_algs=1
wpa=2
wpa_passphrase=[password/bintangjatuh]
wpa_key_mgmt=WPA_PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

Keluar esc :wq

Buat file konfigurasi 

```bash
nvim /etc/systemd/network/02-wireless-ap-network
```

Isi :

```bash 
[Match]
Name=wlan0

[Network]
Address=[ip wifi]
DHCPServer=yes
```
Keluar esc :wq

Restart systemd-networkd

```bash
systemctl restart systemd-networkd
```

Buat file konfigurasi 

```bash
Nvim  /etc/sysctl.d/30-ipforward.conf
```

ISI
```bash
net.ipv4.ip_forward=1
```

Esc: wq

```bash
Sudo sysctl–system
Systemctl enable hostapd
```

Ctrld sampai selesai asciinemanya terus reboot di server
	
## Bagian 5 : Konfigurasi Akhir Firewall

Kami menggunakan command di bawah, sebagai berikut :

systemctl enable firewalld

system start firewalld

firewall-cmd –list-all-zone

firewall-cmd –info-zone=public

firewall-cmd --zone=public –remove-service=dhcpv6-client –permanent

firewall-cmd –reload

firewall-cmd –info-zone=trusted

firewall-cmd –info-zone=home
firewall-cmd –zone=home –remove-service=dhcpv6-client –permanent
firewall-cmd –reload
firewall-cmd –info-zone=home
firewall-cmd –info-zone=work
firewall-cmd –zone=work –remove-service=dhcpv6-client –permanent
firewall-cmd –reload
firewall-cmd –info-zone=trusted
firewall-cmd –info-zone=internal
firewall-cmd –zone=internal –remove-service=dhcpv6-client –permanent
firewall-cmd –reload
firewall-cmd –info-zone=trusted
firewall-cmd –info-zone=dmz
firewall-cmd –info-zone=external
firewall-cmd –info-zone=block
firewall-cmd –info-zone=drop
firewall-cmd –reload
firewall-cmd –list-all-zone
firewall-cmd –zone=internal –remove-service={mdns,samba-client} –permanent
firewall-cmd –reload
exit







