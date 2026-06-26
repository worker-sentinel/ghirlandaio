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
```bash
Ssh [namauserserver]@[ip address server]
```
Install podman compose
Untuk menjalankan container dengan menggunakan file docker compose. Untuk menyatukan file-file yang dibutuhkan instalasi slims di satu folder. 
```bash
sudo pacman -S podman-compose 
```
Kalau ditanyain password, masukkan password server.
```bash
Cd .config/containers
```
Download link zip yang disiapkan oleh developer slims menggunakan wget
```bash
Sudo pacman -S wget 
‘wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip’
```

Cek apakah master.zip ada atau tidak
```bash
ls
```

Unzip file dengan unzip
```bash
sudo pacman -S unzip
unzip master.zip
```

Cek lagi
```bash
ls
```
(biru artinya folder)

Masuk ke folder 
```bash
cd docker-compose-for-slims-master
```

Cek isinya 
```bash
ls
```
 
Di arch linux ada port, jalur-jalur yang digunakan aplikasi untuk berinteraksi. Batasan untuk user biasa adalah 1024 ke atas, port ke bawahnya digunakan untuk aplikasi-aplikasi krusial. 
	
Mengatur bahwa port 80 adalah milik user 
```bash
Sudo nvim /etc/sysctl.d/99-custom.conf
```

Isi seperti di bawah 
```bash
net.ipv4.ip_unprivileged_port_start=80
```

Keluar esc :wq

Restart konfigurasi yang dibuat
```bash
Sudo sysctl --system
```

Cek 
```bash
	Ls
```
Gunakan file docker-compose-redis.yaml 
Pada defaultnya baca yang docker compose dulu, jadi harus direname
```bash
mv docker-compose.yaml docker-compose.yaml.back
```
Ubah docker-compose-redis.yaml menjadi docker-compose.yaml
```bash
Mv docker-compose-redis.yaml docker-compose.yaml
```
Buat file konfigurasi
```bash
	Nvim docker-compose.yaml
```
Hapus tagar yang terletak di sebelum ports, hapus juga tagar di line setelahnya
Lalu tambahkan hingga “127.0.0.1:3306:3306”
Lakukan langkah yang sama di port redis.
Hapus line yang ada ipv4 || pencet saja esc lalu dd 

### 3.1 : BERI HAK AKSES WRITE, READ, EXECUTE
READ ditandai dengan angka (4), WRITE (2), EXECUTE (1) Jadilah 4+2+1 = 7

USER punya akses WRITE, READ, EXECUTE
GRUP punya akses WRITE, READ, EXECUTE
OTHERS punya akses WRITE, READ, EXECUTE

```bash
sudo chmod -R 777 app/slims
podman compose up -d
```

…
```bash
Sudo Firewall-cmd –zone=public –add-port=80/tcp –permanent
Sudo Firewall-cmd reload
```

Lalu masuk ke firefox > masuk ke slims
Caranya harus tahu masuk ip servernya 
Buka browser
http://(ip servernya):80

