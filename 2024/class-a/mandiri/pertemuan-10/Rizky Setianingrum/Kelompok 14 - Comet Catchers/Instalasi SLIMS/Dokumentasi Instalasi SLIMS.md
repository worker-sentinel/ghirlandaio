# Dokumentasi Instalasi SLIMS

## Download Source SLiMs
```
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip 
unzip master.zip
```

## Rename folder
```
mv docker-compose-for-slims-master compose
cd compose
```

## Kernel
```
sudo nvim /etc/sysctl.d/99-custom.conf   
sudo chmod -R 777 app/slims  
sudo nvim /etcsubid   
```

Masukkan 
```
[user]:100000:65536      
sudo nvim /etcsugid   

dan

[user]:100000:65536
```

## configure storage
```
nvim ~/.config/cleo/storage.conf

[storage]  
driver = "overlay"  
[storage.options.overlay]                                                                                                                                                                 mount_program = ""   
mountopt = "userxattr"

sudo nvim /etc/containers
```

```
podman pull docker.io/mysql:5.7

podman compose up -d

podman ps -a

exit
```
