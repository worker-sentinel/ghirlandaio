# Deploy SLiMS di Kubernetes

## Buat Manifest

Buat file manifest menggunakan nvim.

```
nvim slims.yaml
```

Isi file dengan konfigurasi berikut: Service untuk database, Pod MariaDB, Deployment web SLiMS, dan Service untuk expose web.

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

Keterangan tiap objek:

- `slims-db-service` — Service internal (ClusterIP default) yang mengekspos port 3306 ke Pod database.
- `slims-db-pod` — Pod MariaDB dengan database, user, dan password yang ditentukan lewat environment variable.
- `slims-web-deployment` — Deployment aplikasi SLiMS dengan 2 replica, image `slimsofficial/slims:latest`, port container 80.
- `slims-web-service` — Service bertipe NodePort yang expose aplikasi web ke port 30088 pada node.

Simpan dan keluar dari nvim.

```
exit
```
