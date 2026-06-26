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


