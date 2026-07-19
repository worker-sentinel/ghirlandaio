###### bab binding 
lakukan binding menggunakan kubernetes engine namun sebelum dapat melakukan bidning set up terlebih dahulu waktu menggunakan command timedatectl untuk cek sinkronisasi waktu 
```sh
timedatectl
```

enable resinkronisasi waktu dengan command 
```sh
timedatectl set-ntp true 
```


kemudia set timezone ke asia jakarta 
```sh
timedatectl set-timezone Asia/Jakarta
```

untuk menginstall sekaligus mendeploy kubernetes engine kalian command ; 

```sh
curl -sfL https://get.k3s.io | sh -
```
kemudian panggil token atau cek ke token untuk deploy dengan 
```sh
cat /var/lib/rancher/k3s/server/node-token
```

trus user lain menggunakan token yang sudah ada dan menyesuaikan dengan IP dari milik control (admin); 
```sh
curl -sfL https://get.k3s.io | sudo sh - agent --server https://[masukan ip milik control di sini] --token [masukan token yang telah di generate control di sini ]
```
setelahnya binding berhasil di lakukan 

