# Install Aplikasi SLiMS
**Kelompok 11 Pluto Pioneer**
1. Silvi Nur Aini
2. Fatma Ramadhani
3. Salfa Firyal Hasanah
4. Muhammad Fauzan Azhiimi
5. Aditya Pangruwating Dhiyu
6. Ahmad Hafiz Baihaqi

## Running Compose
```
podman compose up -d
```
```
sudo nvim /etc/containers/registries.conf
```
**isi dengan**
```
unqualified-search-registries = ["docker.io"]
```
**lalu kita akan,**
```
nvim slims-test.yaml
```
```
apiVersion: v1
kind: Service
metadata:
  name: slims-db-service
spec:
  ports:
  - port: 3306
  selector:
    app: slims-db
---
apiVersion: v1
kind: Pod
metadata:
  name: slims-db-pod
  labels:
    app: slims-db
spec:
  containers:
  - name: mariadb
    image: mariadb:10.6
    env:
    - name: MARIADB_ROOT_PASSWORD
      value: "slimsrootpass"
    - name: MARIADB_DATABASE
      value: "slims_db"
    - name: MARIADB_USER
      value: "slims_user"
    - name: MARIADB_PASSWORD
      value: "slimspassword"
    ports:
    - containerPort: 3306
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: slims-web-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: slims-web
  template:
    metadata:
      labels:
        app: slims-web
    spec:
      containers:
      - name: slims
        image: slimsofficial/slims:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: slims-web-service
spec:
  type: NodePort
  selector:
    app: slims-web
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30088
```

## Cek containers
```
podman ps
```
```
podman ps -a
```
## Masukan port SliMS
```
firewall-cmd --zone=public --add-port=80/tcp –permanent
```
```
firewall-cmd –reload
```
## 
Cek IP a
```
ip a
```
## Kembali ke user 
```
exit
```
## Keluar dari SSH server
```
logout
```
## Keluar dari root PC admin
```
exit
```
## Berhentikan asciinema
```
Ctrl + d
```
```
asciinema upload nama file.cast
```
## Buka di browser buat cek apakah SLiMS sudah masuk atau belum
```
http://[IP Server]:80
```
# Tampilan Slims dari kacamata Client
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/1dc7144a-cc22-45d3-9464-f8e718bf3b93" />

**Tampilan dari hp client**

<img width="717" height="1600" alt="image" src="https://github.com/user-attachments/assets/6d55fab1-e1c3-4331-a58c-d7db791bc5eb" />
