# HOW TO INSTALL SLiMS
```
systemctl status podman
systemctl enable podman
systemctl status podman
systemctl enable podman
systemctl start podman
systemctl status podman
```
```
mkdir aril
ls
unzip master.zip
docker-compose-for-slims-master
ls
cd .config/
ls
d ..
ls
cd aril/
unzip master.zip
ls
mv docker-compose-for-slims-master/ compose
ls
ls
cd compose
cd aril
```
```
sudo nvim /etc/systcl.d/aril.conf

(ketik manual)
net.ipv4.ip_unprivilaged_port_start=80                                                                                                                                                        
sudo sysctl --system
:q!
```
```
sudo nvim /etc/sysctl.d/aril.conf

(pastikan seperti di bawah ini)
net.ipv4.ip_unprivilaged_port_start
80                                                                                                                                                        
sudo sysctl --system
:q!
```
```
ls
mv docker-compose-redis.yaml docker-compose.yaml
nvim docker-compose.yaml
:wq
```
```
sudo chmod -R 777 app/slims
podman compose up -d
odman compose up -d
nvim /etc/con
nvim /etc/containers/registries.conf
(Lihat dibagian ini, persis)
# unqualified-search-registries = ["example.com"]                                                                                                                                             
#                                                                                                                                                                                             
 [[registry]]

(ubah menjadi..)
unqualified-search-registries = ["docker.io"]
:wq
```
```
podman compose up -d
sudo nvim /etc/subgid
:wq
```
```
cd ..
nvim storage.conf
:wq
exit
```
