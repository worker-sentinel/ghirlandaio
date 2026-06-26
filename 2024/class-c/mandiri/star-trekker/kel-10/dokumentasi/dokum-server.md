## bersihkan service di firewall

```
systemctl enable --now firewalld
```
```
sudo firewall-cmd --list-all-zone
```
```
sudo firewall-cmd --permanent --remove-service=ssh --zone=work
```
```
sudo firewall-cmd --permanent --remove-service=dhcpv6-client --zone=work
```
```
sudo firewall-cmd --permanent --remove-service=dhcpv6-client --zone=public
```
```
sudo firewall-cmd --permanent --remove-service=dhcpv6-client --zone=internal
```
```
sudo firewall-cmd --permanent --remove-service=mdns --zone=internal
```
```
sudo firewall-cmd --permanent --remove-service=samba-client --zone=internal
```
```
sudo firewall-cmd --permanent --remove-service=ssh --zone=internal
```
```
sudo firewall-cmd --permanent --remove-service=ssh --zone=home
```
```
sudo firewall-cmd --permanent --remove-service=samba-client --zone=home
```
```
sudo firewall-cmd --permanent --remove-service=mdns --zone=home
```
```
sudo firewall-cmd --permanent --remove-service=dhcpv6-client --zone=home
```
```
sudo firewall-cmd --permanent --remove-service=ssh --zone=external
```
```
sudo firewall-cmd --permanent --remove-service=ssh --zone=dmz
```
```
sudo firewall-cmd --reload
```

## setup podman rootless

> pastikan di /etc/subuid

```
nvim /etc/subuid
```
```
username:100000:65536
```
```
nvim /etc/subgid
```
```
username:100000:65536
```
```
sudo nvim /etc/containers/registries.conf.d/10-unqualified-search-registries.conf
```
isi
```
unqualified-search-registries = ["docker.io"]
```

## setup slims

```
mkdir -p .config/containers
```
```
nvim .config/containers/storage.conf
```
isi
```
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = ""
mountopt = "userxattr"
```
```
cd .config/containers
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
```
mv docker-compose-for-slims-master compose
```
```
cd compose
```
```
sudo nvim /etc/sysctl.d/69-custom.conf
```
isi
```
net.ipv4.ip_unprivileged_port_start=80
```
```
sudo systctl --system
```
```
mv docker-compose.yaml docker-compose.yaml.bck
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```
```
nvim docker-compose.yaml
```
> pastikan isinya sama dengan
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
        command: --sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION --max_allowed_packet=1024M
        volumes:
            - "./dbdata:/var/lib/mysql"
        ports:
            - "127.0.0.1:3306:3306"
    redis:
        image: redis:latest
        restart: always
        networks: 
            - slims-net
        container_name: redis
        ports:
            - "127.0.0.1:6379:6379"
    app01:
        image: slimsofficial/slims:latest
        restart: always
        networks: 
            - slims-net
        container_name: slims-app
        ports:
            - "80:80"
            - "443:443"
        volumes:
            - "./app:/var/www/html"
            - "./conf/php/php.ini:/usr/local/etc/php/conf.d/php.ini"
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
podman-compose up -d
```
```
sudo firewall-cmd --zone=public --add-port=80 --permanent
```
```
sudo firewall-cmd --reload
```


## disable kernel module

```
lsmod
```
```
sudo nvim /etc/modprobe.d/hardenings.conf
```
```
install usb_storage /bin/false
```
```
sudo mkinitcpio -P
```

