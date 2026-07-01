## Join Worker Node 
```
curl -sfL https://get.k3s.io | sudo sh -s - agent \  --server https://192.168.137.169:6443 \  --token K10fela20dcedccad34aa3d918160e4bb7ba65102d939a8efc9c57b4ee40b6837c0::server:18afdd28bab82473047711ab74bclcfi
```
Berfungsi untuk menghubungkan node internal, data, dan public ke dalam cluster Kubernetes sebagai worker node. Instalasi dilakukan menggunakan K3s Agent dengan menentukan alamat control plane melalui parameter --server dan melakukan autentikasi menggunakan --token. Setelah berhasil, node akan bergabung ke cluster dan siap menjalankan workload yang dikelola oleh control plane.
