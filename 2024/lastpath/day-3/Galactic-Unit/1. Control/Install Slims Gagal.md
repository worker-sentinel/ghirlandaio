### 1. Konfigurasi Registry Container

```
sudo nvim /etc/containers/registries.conf
```

> Membuka file konfigurasi registry container menggunakan editor **Neovim** 

---

### 2. Membuat File Deployment SLiMS

```
sudo nvim slims-test.yaml
```

> Membuat file konfigurasi Kubernetes (`YAML`) untuk menjalankan database MariaDB, aplikasi SLiMS, dan service.

> Isi file:

```yaml
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

---

### 3. Menerapkan Konfigurasi Kubernetes

```
kubectl apply -f slims-test.yaml
```

> Menjalankan seluruh resource yang terdapat pada file `slims-test.yaml` ke dalam cluster Kubernetes.

---

### 4. Mengecek Pod

```
kubectl get pods -o wide
```

> Berisi informasi status, IP pod, node yang digunakan, dan informasi lainnya.

---
### 5. NOTES

> kita gagal karena masih belum bisa connect kubernetes dan sudah berusaha uninstall-install berkali-kali, tapi tetap tidak bisa connect.





