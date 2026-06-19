## Dokumentasi Install Slims

```bash Download Source SLiMs
wget -c https://github.com/slims/docker-compose-for-slims/archive/master.zip 

unzip master.zip

Rename folder
mv docker-compose-for-slims-master compose
cd compose

Kernel
sudo nvim /etc/sysctl.d/99-custom.conf                                                                                                                            sudo chmod -R 777 app/slims  
sudo nvim /etcsubid   

masukin     
[user]:100000:65536      
sudo nvim /etcsugid   
masukin
[user]:100000:65536

configure storage
nvim ~/.config/cleo/storage.conf

[storage]  
driver = "overlay"  
[storage.options.overlay]                                                                                                                                                                    mount_program = ""   
mountopt = "userxattr"

sudo nvim /etc/containers


Lanjutan Install SLiMS
podman pull docker.io/mysql:5.7

podman compose up -d

podman ps -a

exit
```

```bash Connecting admin
sudo nmcli device status  

sudo nmcli device status 

sudo nmcli connection add type ethernet ifname enp3s0f4u2c2
con-name "admin-connection" ipv4.method manual ipv4.addresses 10.10.10.10/24 ipv
4.gateway 10.10.10.1 ipv4.dns 8.8.8.8   

enter password

sudo nmcli connection modify admin-connection connection.autoconnect yes 

enter password
exit
```

## Connecting Operator

```bash nmcli device status

sudo nmcli connection add type ethernet ifname enp3s0f4u2c2 con-name “operator-connection” ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4 gateway 10.10.10.1 ipv4.dns 8.8.8.8

masukin password for operator:......

nmcli connection add type ethernet ifname enp3s0f4u2c2 con-name “operator-connection” ipv4.method manual ipv4.addresses 10.10.10.11/24 ipv4. gateway 10.10.10.1 ipv4.dns 8.8.8.8

nmcli connection up operator-connection

ip a

ip r

ping 8.8.8.8

exit
```

<img width="1599" height="899" alt="d2953748-12f5-435a-a5a5-87563a9d21d3" src="https://github.com/user-attachments/assets/4c5dae19-398d-40ce-b597-f27b87234d08" />
