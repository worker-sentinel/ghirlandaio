
### Konfigurasi Podman 

Konfig podman untuk data, internal, dan publik

Membuka file konfigurasi registry:
```
sudo nvim /etc/containers/registries.conf
```

masuk menu insert (i) lalu setelah itu edit hapus hastag pada bagian 
unquallfied-search-registries = [example.com] 
ganti example.com menjadi:

```
unquallfied-search-registries = [docker.io] 
```

Mengaktifkan service Podman:
```
sudo systemctl enable podman
```

Menjalankan Podman:
```
sudo systemctl start podman
```


### Instalasi K3S

```
curl -sfL https://get.k3s.io | sudo sh -s - agent --server https://10.18.210.197 --token K10d54c6e2ada2adcc9f0c64796e9bd11fdec77c820543b3a75fc4884d7207089e6::server:kubernet
```

Melihat waktu sistem:
```
timedatectl
```

Mengaktifkan sinkronisasi waktu:
```
sudo timedatectl set-ntp true
```
Mengaktifkan Network Time Protocol agar jam server sinkron otomatis.


Mengubah Timezone (Setting zona waktu menjadi WIB):
```
sudo timedatectl set-timezone Asia/Jakarta
```

Lalu lakukan kembali sinkronisasi waktunya:
```
sudo timedatectl set-ntp true
```

Melihat Log K3S :
```
sudo journalctl -xeu k3s-agent.service
```

Mengedit Service K3S:
```
sudo nvim /etc/systemd/system/k3s-agent.service
```

masuk mode insert (i), lalu ganti internal 

agent \ 
    '--internal' \  
    'https://10.18.210.197' \ 
    '--token' \ \'K10e72a47660799a1e48edb10de57efcf87eb5ae859855b5d672573f3cdffa2ca59::server:7b88c4d08dfc3ab438596a203f1c7318' \
    
menjadi:

```
agent \
'--server' \        
'https://10.18.210.197' \                                                         '--token' \ 'K10e72a47660799a1e48edb10de57efcf87eb5ae859855b5d672573f3cdffa2ca59::server:7b88c4d08dfc3ab438596a203f1c7318' \
```

Reload Systemd:
```
sudo systemctl daemon-reload
```
Memberitahu systemd bahwa file service telah berubah dan harus dijalankan setelah mengedit file service.

Restart K3S Agent:
```
sudo systemctl restart k3s-agent
```
Menghentikan kemudian menjalankan ulang K3S Agent agar konfigurasi baru digunakan.

Cek status K3S:
```
sudo systemctl status k3s-agent
```


Terdapat opsi jika ingin menghapus K3S

Menghapus K3S:
```
sudo /usr/local/bin/k3s-agent-uninstall.sh
```

Menghapus Data K3S:
```
sudo rm -fr /etc/rancher/k3s /var/lib/rancher/k3s
```

Kemudian, Reload Systemd:
```
sudo systemctl daemon-reload
```

### Agent

SSH ke Server:
(nama user dan ip disesuaikan dengan keterangan pada masing-masing perangkat)
```
ssh user@ip
```

```
ssh internal@10.50.66.163
```

Export Token:
```
export K3S_TOKEN=kubernet
```


Install Agent Menggunakan Variable:
```
curl -sfL https://get.k3s.io | sh -s - agent --server https://10.50.66.197:6443
```
Masukkan Password

```
 curl -sfL https://get.k3s.io | K3S_URL=https://10.50.66.197:6443 K3S_TOKEN=K10d54c6e2ada2adcc9f0c64796e9bd11fdec77c820543b3a75fc4884d7207089e6::server:kubernet sudo sh -- agent
```
Masukkan Password


Export Token:
```
export K3S_TOKEN=kubernet
```

Install Menggunakan URL dan Token:
```
curl -sfL https://get.k3s.io | sh -s - agent --server https://10.50.66.197:6443
```

```
curl -sfL https://get.k3s.io | sh -s - agent --server https://10.50.66.197:6443 --token K10d54c6e2ada2adcc9f0c64796e9bd11fdec77c820543b3a75fc4884d7207089e6::server:kubernet
```

```
ssh internal@10.153.19.163
```

```
ping 1.1.1.1
```

```
ssh internal@10.50.66.163
```

Masukkan Password
```
curl -sfL https://get.k3s.io | sudo sh -s - agent \ --server https://10.50.66.197 --token K109f1dce7f014b050703103a9b1d6d96853a228994cecdb2c93f63c8a6c2c440d0::server:8ca16c66635a6d7da50fda41a38f14a8
```

Masukkan Password
```
sudo /usr/local/bin/k3s-agent-uninstall.sh
```

```
curl -sfL https://get.k3s.io | sudo sh -s - agent \ --server https://10.50.66.197 --token K10187deb30cff6a2ccbca72725675fda808f3cca95db3abe01f4cf3a3ddddf9d73::server:fc5be170f4b231a84753b04a85dc7a52
```

```
sudo /usr/local/bin/k3s-agent-uninstall.sh
```

Masukkan Password
```
curl -sfL https://get.k3s.io | sudo sh -s - agent \ --server https://10.50.66.197 --token K10187deb30cff6a2ccbca72725675fda808f3cca95db3abe01f4cf3a3ddddf9d73::server:fc5be170f4b231a84753b04a85dc7a52
```

```
Connection to 10.50.66.163 closed.
```


---
### GKE1

Install K3S Server, menginstal node sebagai Server/Master:
```
curl -sfL https://get.K3s.io| sh -
```

Melihat Token Cluster:
```
sudo cat /var/lib/rancher/k3s/server/node-token
```

```
exit
```

### GKE2

```
ssh internal@10.153.19.163
```

```
curl -sfL https://get.k3s.io | sudo sh -s - agent \ --server https://10.153.19.197 --token K109f1dce7f014b050703103a9b1d6d96853a228994cecdb2c93f63c8a6c2c440d0::server:8ca16c66635a6d7da50fda41a38f14a8
```
Masukkan Password



---
# Install-Slims

Membuat File Deployment:
```
nvim slims.yaml
```
wq!

Deploy Manifest:
```
sudo kubectl apply -f slims-multi-node.yaml
```
Masukkan Password

Deploy Multi Node
```
sudo kubectl apply -f slims.yaml
```

Melihat Pod:
```
sudo kubectl get pods -o wide
```

Melihat Detail Pod:
```
sudo kubectl describe pod slims-web-deployment-5f9fdb8fb5-4hj8p
```

Melihat waktu sistem:
```
timedatectl
```

Install Podman
```
sudo pacman -S podman
```

Enable Podman:
```
sudo systemctl enable podman
```

```
sudo systemctl start podman
```

```
nvim s
```

```
nvim slims.yaml
```

```
exit
```


### Master

Ekspor Token:
```
export K3S_TOKEN=kubernet
```

Install Master Menggunakan Variable:
```
curl -sfL https://get.k3s.io | sh -s - server --disable treafik
```

Cek Status K3S
```
systemctl status k3s.service
```

```
sudo kubectl get node
```

```
ls
```

```
mkdir .kube
```

```
sudo cp /etc/rancher/k3s/k3s.yaml .kube/config.yaml
```

```
sudo chown $USER:$GROUP .kube/config.yaml
```

```
export KUBECONFIG=~/.kube/config.yaml
```

```
kubectl get node
```

```
ssh internal@10.50.66.163
```

```
exit
```

### Podman Control

Instal podman:
```
sudo pacman -S podman
```


```
sudo systemctl enable podman
```

```
sudo systemctl start podman
```


### Sharing

```
git clone -b qa/2.x https://github.com/artefactual/atom.git atom
```

```
ls
```

```
cd docker/
```

```
ls
```

```
nvim docker-compose.dev.yml
```

```
volumes:
  percona_data:
services:

  percona:
    image: percona/percona-server:8.4
    env_file: etc/environment 
    volumes:
      - percona_data:/var/lib/mysql:rw
      - ./etc/mysql/mysqld.cnf:/etc/my.cnf.d/mysqld.cnf:ro
    ports:
    - "127.0.0.1:63003:3306"
```


### Install vm dulu 

```
sudo pacman -S qemu-base virt-manager libvirt dnsmasq vde2 openbsd-netcat
```

```
sudo systemctl enable libvirt 
```

```
sudo chmod -aG libvirt [nama user laptop]
```

install iso debian versi yang sesuai dengan Atom
lalu copy iso tersebut biasanya ada di file Downloads

```
sudo mv Downloads/[nama file iso] /var/lib/libvirt/images
```

lalu install secara manual ubuntu nya
# Deploy-Atom

```
sudo su
```

Masukkan Password
```
kubectl label node kel5 workload=vm
```

```
kubectl taint nodes kel5 vm=true:NoSchedule
```

```
kubectl apply -f https://github.com/kubevirt/kubevirt/releases/latest/download/kubevirt-operator.yaml
```

```
kubectl get pods -n kubevirt
```

```
kubectl get pods -n cdi
```

```
kubectl apply -f https://github.com/kubevirt/containerized-data-importer/releases/latest/download/cdi-operator.yaml
```

```
kubectl get crd | grep cdi
```

```
kubectl get pods -n kubevirt
```

```
kubectl get pods -n cdi -w
```





