# ssh nayla@10.219.56.6
masukin passwd

# sudo pacman -S podman-compose
masukin passwd

tahap install

# mkdir vit
cd vit

pacman -S wget (error)

sudo pacman -S wget (berhasil)

wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip


# sudo pacman -S unzip
Masukin passwd

unzip master.zip

ls

[docker-compose-for-slims-master] master.zip (muncul)

Mv docker-compose-for-slims-master golda

ls

[golda] master.zip

cd golda

sudo nvim /etc/sysctl.d/nike.conf

Masukin passwd

Klik i

net.ipv4.ip_unprivileged_port_start=80 

Klik esc kemudian ketik :wq

# Sudo sysctl –system
Sudah ada “net.ipv4.ip_unprivileged_port_start=80” di bagian paling bawah

Ls

Conf 

Docker-compose-redis-elasticsearch.yaml | docker-compose-redis.yaml  | phpmyadmin.env

App

db_default.env | docker-compose-redis-load_balanced-haproxy.yaml | docker-compose.yaml 

# mv docker-compose.yaml doker-compose.yaml.golda 
nvim docker-compose-redis.yaml docker-compose.yaml

Ketik :q!

mv docker-compose-redis.yaml doker-compose.yaml 

Nvim do

docker-compose-redis-elasticsearch.yaml | doker-compose.yaml  

docker-compose-redis-load_balanced-haproxy.yaml | doker-compose.yaml.golda

nvim docker-compose.yaml

Klik i

Hapus hashtag pada bagian tulisan ports yang tadinya warna abu-abu berubah menjadi warna biru dan angka bawahnya juga hapus hastag sehingga angkanya berubah menjadi wana hijau

:wq

# Sudo chmod -R 777 app/slims
Msukin passwd

Podman compose up -d (error)

ls

nvim docker-compose.yaml

Scroll sampe networks klik i kemudian hapus config dan subnet :wq

mv doker-compose.yaml docker-compose.yaml                                                                    podman compose up -d (masih error)

sudo nvim /etc/containers/registries.conf 

Msukin passwd

Bagian # unqualified-search-registries = ["example.com"] hapus hastagnya dan diganti dengan unqualified-search-registries = ["docker.io"] tulisannya jd berwarna biru dan hijau stelah itu :wq

# podman compose up -d (msih error)
nvim docker-compose.yaml

Hapus bagian db, redis dan app01 ipv4_address, kemudian slims-net ditambahin - di dpnnya menjadi - slims-net

Tambahkan - dan angka 127.0.0.1:

Bagian db: 

ports:                                                                                                                                    - "127.0.0.1:3306:3306" 

Bagian redis:

ports:                                 	                                                                                               - "127.0.0.1:6379:6379"  

:wq

# podman compose up -d (berhasil)
podman ps -a

sudo nvim /etc/containers/registries.conf                                                                    password for nayla:                                                                                                              nvim docker-compose.yaml

:q

sudo nvim /etc/subuid

nayla:100000:65536 

:wq

sudo nvim /etc/subgit                                                                                       

sudo nvim /etc/subgid                                                                                      

 cd ..                                                                                                    

 nvim storage.conf

# ip a
sudo pacman -S flatpak

sudo pacman -S firefox

sudo pacman -S firefox

Msukin psswd  kel10

ls

ls -la

cd .config/

ls

Thunar  asciinema  containers  dconf  kelompok10  mozilla  pulse  xfce4 (muncul)

cd ..

ssh nayla@10.219.56.6

Mskin passwd

# Exit
