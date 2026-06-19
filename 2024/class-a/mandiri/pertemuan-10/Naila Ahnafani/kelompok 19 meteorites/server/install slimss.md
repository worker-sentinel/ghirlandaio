## install slims
````
sudo pacman -S podman-compose
mkdir -p .config/kel19
cd .config/kel19
sudo pacman -S wget
unzip master.zip
mv docker-compose-for-slims-master compose
cd compose
sudo nvim /etc/sysctl.d/13-custom.conf
masukkan:
net.ipv4.ip_unprivlegend_port_start=80

sudo sysctl --system
mv docker-compose.yaml docker-compose.yaml.lvm
mv docker-compose-redis.yaml docker-compose.yaml
ls
lalu cek apakah ada:
app             docker-compose-redis-elasticsearch.yaml          docker-compose.yaml.lvm
conf            docker-compose-redis-load_balanced-haproxy.yaml  phpmyadmin.env
db_default.env  docker-compose.yaml                              README.md

nvim docker-compose.yaml
lalu hapus tanda "#" pada bagian dibawah ini:
volumes:
    - "./dbdata:/var/lib/mysql"
#ports:
#    - "3306:3306"
redis:
    image: redis:latest
    restart: always
    networks:
        slims-net:
            ipv4_address: 172.18.1.4
   container_name: redis
   #ports:
   #    - "6379:6379"
sudo systemctl enable --global podman
nvim -/.convig/kel19/storage.conf
lalu masukkan:
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"

nvim /etc/containers/registries.conf
hapus "#" di bagian #unqualified-search-registries = ["example.com"] dan ubah example.com menjadi docker.io hingga menjadi seperti dibawah ini

# # An array of host[:port] registries to try when pulling an unqualified image, in order.
unqualified-search-registries = ["docker.io"]
````
## grant permission
````
sudo chmod -R 777 app/slims
````
````
podman compose up -d
podman ps
ls
cat docker-compose.yaml
nvim docker-compose.yaml
pada port 80:80 dan port 443:443 ubah menjadi port 8080:80 dan 8443:443
````
````
podman compose down
podman compose up -d
````
````
cat db_default.env
ss -tulpn | grep :80
exit
