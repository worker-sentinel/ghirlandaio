# INSTALASI K3S

### Instalasi k3s
```
curl -sfL https://get.k3s.io | sh -
```
---
### Cek Apakah k3s Sudah Berjalan (running) atau Ada Error
```
systemctl status k3s
```
---
### Menampilkan Daftar Node (mesin) yang Menjadi Bagian dari Cluster Kubernetes
```
k3s kubectl get nodes
```
