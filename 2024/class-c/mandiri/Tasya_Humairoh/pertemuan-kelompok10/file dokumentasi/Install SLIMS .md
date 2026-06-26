## MENGINSTALL APLIKASI SLIMS

## Menginstall open ssh:

```
sudo packman -S open ssh
```
```
sudo su
```

## Menjalankan sshd

```
sudo systemctl enable sshd
```

```
systemctl start sshd
```

## koneksi dari ip dan user laptop server ke dalam laptop admin

```
ssh force@[ip server]
```

## Menginstall podman compose

```
sudo packman -S podman-compose
```
## Mendowload slims wget

```
cd .config
```
```
cd containers
```

**lalu cek Kembali**

```
ls
```
```
sudo packman
```
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
```
sudo packman -S wget unzip
```

## Mengextract zip

```
unzip master.zip
```

## Lalu di cek Kembali menggunakan command

```
ls
```

```
mv docker-compose-for-slims-master compose
```

```
cd compose
```

## Konfigurasi system

```
sudo nvim /etc/sysctl.d/compose.conf
```
```
sudo nvim /etc/sysctl.d/nebula.conf
```
```
insert
```
```
net.ipv4.ip_unprivileged_port_start=80
```
```
esc :wq
```
```
sudo sysctl –system
```
```
mv docker-compose.yaml docker-compose.yaml.nbl
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```

**kemudian ketik manual dengan mengikuti gambar dibawah ini**
<img width="794" height="529" alt="image" src="https://github.com/user-attachments/assets/9719d410-322b-4e8b-9df5-6de445e394e9" />

 
**Membuat hak akses untuk folder**

```
sudo chmoot -R 777 app/slims
```
```
podman compose -f docker-compose.yaml up-d
```
```
sudo nvim /etc/contaioners/registries.conf
```

## Memasukan port untuk SLIMS

```
sudo firewall-cmd --zone=public –add-port=80/tcp –permanent
```

```
sudo firewall-cmd reload
```

## Melakukan pengecekan IP Server


```
ip a
```
## Dilanjutkan dengan membukan browser untuk mengecek apakah slims sudah masuk atau belum, dengan menggunakan IP server* 

```
https://[IP Server]: 80
```
<img width="1280" height="720" alt="apk slims" src="https://github.com/user-attachments/assets/4e3bb13c-ee44-4582-b021-f58f3321331a" />

