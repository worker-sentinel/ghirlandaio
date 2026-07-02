# DOKUMENTASI DEPLOY SLIMS

## CEK REGIONAL DATA, INTERNAL, DAN PUBLIC SUDAH TERSAMBUNG KE ADMIN ATAU BELUM
```
kubectl get nodes
```
Jika sudah tersambung maka Status per-regional akan menunjukkan Ready

<img width="1600" height="333" alt="WhatsApp Image 2026-07-02 at 21 49 45(1)" src="https://github.com/user-attachments/assets/19c7355a-17c3-4522-b16c-1d2d3ab01f88" />

## BIKIN FILE MANIFES DALAM KUBERNETES
```
nvim slims-multi-node.yaml
```
isikan seperti gambar dibawah:
<img width="945" height="1280" alt="WhatsApp Image 2026-07-02 at 21 33 54" src="https://github.com/user-attachments/assets/41953167-d28e-4c33-a2d8-35d9d7cf01cb" />
<img width="950" height="1280" alt="WhatsApp Image 2026-07-02 at 21 33 54(1)" src="https://github.com/user-attachments/assets/42d457da-5b34-4173-a3a2-2d8910aac434" />
<img width="917" height="1280" alt="WhatsApp Image 2026-07-02 at 21 33 54(2)" src="https://github.com/user-attachments/assets/13711f7d-270d-46aa-941f-e885b2df31ba" />

didalam file tersebut terdapat file untuk slims, mariadb, redis, dan nginx

## LALU TERAPKAN KE REGIONAL LAINNYA
```
kubectl apply -f slims-multi-node.yaml
```
syantx ini digunakan untuk men-deploy atau menjalankan aplikasi slims pada cluster kubernetes


## MENGECEK POD SUDAH BERJALAN DAN BERADA DI NODE YANG BENAR
```
kubectl get pods -o wide
```

## LALU AKSES SLIMS DI BROWSER
pastikan dalam koneksi yang sama lalu buka browser, dan tambahkan port 300088
```
http://10.237.160.246:300088
```
<img width="1365" height="636" alt="WhatsApp Image 2026-07-02 at 21 18 43" src="https://github.com/user-attachments/assets/5bafc94c-ee29-4ebe-8d6f-3756f83e375c" />







