# Connect SSH

konek satu jaringan yang sama
```
ssh [nama root]@ip a
```

Instal K3s
```
ip a
```
```
curl -sfl https://get.k3s.io | sh -
```

Melihat General Token
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
Melihat PC se;anjutnya nyambung atau tidak
```
kubectl get nodes
```

Install kubectl
```
pacman -S kubectl
```
