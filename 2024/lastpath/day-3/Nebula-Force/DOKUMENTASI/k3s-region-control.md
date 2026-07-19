# Kurbenetes

## Masuk mode root
Masuk sebagai admin utama(root) dengan menggunakan command

```
Sudo su
```
## Lalu masukkan password 

## Download Script Installer K3S
Untuk menginstall K3S menggunakan command

```
Curl -sfL https://get.k3s.io | sh –
```

## Lalu melihat token
Untuk melihat isi dari sebuah file teks yaitu node-token bisa menggunakan command

```
Sudo cat /var/lib/rancher/k3s/server/node-token
```

## Lalu tampilkan alamat IP
Untuk melihat beberapa alamat IP bisa menggunakan command

```
Ip a
```
