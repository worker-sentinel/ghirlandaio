## INSTAL SERVER APLIKASI
```
sudo pacman -S podman compose
```

```
mkdir -p .config/containers
```

```
ls .config
```

```
ls .config/
```

```
ls -la
```

```
cd .config/containers/
```

```
sudo pacman -S wget
```

```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```

```
bsdtar -xzf master.zip
```

```
ls
```

```
mv docker-compose-for-slims-master slims
```

```
ls
```

```
cd slims
```

```
ls
```

```
nvim docker-compose-redis-elasticsearch.yaml
```
add
```
version: "3.7" 
services: 
  db: 
      image: mysql:5.7 
      restart: always 
      networks: 
           - slims-net
      container_name: slims-db 
      env_file: 
            - db_default.env
      command: --sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION--max_allowed_packet-1024M
      volumes: 
             - "./dbdata:/var/lib/mysql" 
      ports: 
             - "127.0.1.3306:3306" 
   app01: 
       image: slimsofficial/slims:latest 
       restart: always 
       networks: 
             - slims-net
       container_name: slims-app 
       ports: 
             - "80808:80" 
       #     - "443:443"
networks:
    slims-net:
        name: slims-net
        ipam:
              driver: default

```

```
sudo chmod -R 777 app/slims
```

```
sudo nvim /etc/sysctl.d/99-custome.conf
```
Tambahkan konfigurasi di bawah ini
```
kernel.unprivileged_userns_clone=1
```

```
sudo sysctl --system
```

```
nvim ~/.config/containers/storage.conf
```
Tambahkan konfig di bawah ini
```
[storage]
driver = "overlay"
[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```

```
sudo nvim /etc/containers/registries.conf
```
Tambahkan konfig di bawah ini

uncomenting and "example.com" change to "docker.io"
```
unqualified-search-registries = ["docker.io"]
```

```
sudo su
```

```
podman compose up -d
```

```
podman PS
```
