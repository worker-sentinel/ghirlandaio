# MENGINSTALL APLIKASI SLIMS

## Menginstall open ssh:

```
sudo packman -S open ssh
```
```
sudo su
```

**Menjalankan sshd**

```
sudo systemctl enable sshd
```

```
systemctl start sshd
```

```
ssh force@[ip server]
```

## Menginstall aplikasi slims
## Menginstall podman compose
```
sudo packman -S podman-compose
```
## Mendowload widget
```
cd --config
```

```
mkdir containers
```
## lalu cek Kembali
```
ls
```

```
sudo packman
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip
```
## Melakukan instalasi wget

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
sudo sysctl –system
```
```
mv docker-compose.yaml docker-compose.yaml.nbl
```
```
mv docker-compose-redis.yaml docker-compose.yaml
```
**kemudian ketik manual dengan mengikuti gambar dibawah ini**
<img width="1600" height="900" alt="cc961f8d-8b58-4098-b17e-a9d6b7282d0b" src="https://github.com/user-attachments/assets/ad919a8a-a5fd-4f1b-a4a9-de0469dff21f" />



## Membuat hak akses untuk folder
```
sudo chmoot -R 777 app/slims
```
```
podman compose -f docker-compose.yaml up-d
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
## Dilanjutkan dengan membukan browser untuk mengecek apakah slims sudah masuk atau belum, dengan menggunakan IP server 
```
https://[IP Server]: 80
