# Dokumentasi Deployment Redis di Cluster K3s 
Dokumentasi ini mencatat proses deploy Redis ke cluster K3s, termasuk troubleshooting pod yang stuck Pending akibat node selector yang belum terpenuhi, hingga akhirnya berhasil Running di node internal.
## 1. Percobaan install Redis via pacman (dibatalkan)
```
pacman -S redis
```
output:
```
resolving dependencies...
looking for conflicting packages...

Packages (2) jemalloc-1:5.3.1-2  valkey-9.1.0-1

Total Download Size:   1.81 MiB
Total Installed Size:  5.54 MiB

:: Proceed with installation? [Y/n]
```
Mencoba install Redis langsung di OS host (Arch Linux) via package manager pacman. Paket redis di repo Arch sekarang di-redirect ke Valkey. Instalasi dibatalkan karena Redis akan dijalankan sebagai container di dalam cluster K3s, bukan langsung di host.
## 2. Menulis manifest Redis
```
nano redis.yaml
```
Membuat file manifest Kubernetes untuk Redis, berisi definisi Deployment (image redis:7-alpine, port 6379) dan Service.

## 3. Apply manifest Redis
```
kubectl apply -f redis.yaml
```
output:
```
deployment.apps/redis created
service/redis-service created
```
Resource Deployment dan Service untuk Redis berhasil dibuat di cluster

## 4. Cek status deployment 
```
kubectl get deployment
```
output:
```
NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
redis                  0/1     1            0           11s
slims-web-deployment   2/2     2            2           120m
```
Deployment redis menunjukkan READY 0/1 dan AVAILABLE 0 — pod belum siap/berjalan meskipun sudah 11 detik, tanda ada yang perlu dicek lebih lanjut (biasanya masalah scheduling atau image pull).

## 5. Cek ulang status deployment (masih belum ready)
```
kubectl get deployment
```
Diulang lagi setelah 38 detik, status masih sama (0/1). Mengarah ke keputusan untuk investigasi lebih dalam ke level pod.

## 6. Cek detail pod Redis
```
kubectl get pods -o wide
```
output:
```
NAME                                    READY   STATUS    RESTARTS   AGE    IP          NODE       NOMINATED NODE   READINESS GATES
redis-847fc8d5f6-sktpk                  0/1     Pending   0          76s    <none>      <none>     <none>           <none>
slims-db-pod                            1/1     Running   0          121m   10.42.2.4   internal   <none>           <none>
slims-web-deployment-5f9fdb8fb5-l9c6n   1/1     Running   0          121m   10.42.3.3   amelia     <none>           <none>
slims-web-deployment-5f9fdb8fb5-pc2pd   1/1     Running   0          121m   10.42.2.3   internal   <none>           <none>
```
Pod redis-847fc8d5f6-sktpk berstatus Pending dengan NODE: <none> yang artinya scheduler belum berhasil menempatkan pod ini ke node manapun, berbeda dari pod SLiMS lain yang sudah Running normal.

## 7. Describe pod untuk cari akar masalah
```
kubectl describe pod redis-847fc8d5f6-sktpk
```
output:
```
Node-Selectors:              role=internal
...
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  115s  default-scheduler  0/4 nodes are available: 1 node(s) had untolerated taint(s), 3 node(s) didn't match Pod's node affinity/selector...
```
Manifest redis.yaml punya nodeSelector: role=internal, tapi event FailedScheduling menunjukkan dari 4 node yang ada, tidak satupun cocok: 1 node kena taint yang tidak ditoleransi (kemungkinan control-plane), dan 3 node lain tidak punya label role=internal. Kesimpulan: label role=internal yang diminta pod belum ada di node manapun.

## 8. Edit manifes redis lagi
```
nano redis.yaml
```
Mengecek ulang isi nodeSelector guna memastikan bukan typo di YAML sebelum memutuskan solusi akhir.

## 9. Cek label semua node
```
kubectl get nodes --show-labels
```
output:
```
NAME        STATUS     ROLES           AGE   VERSION        LABELS
amelia      Ready      public          24h   v1.36.2+k3s1   ...node-role.kubernetes.io/public=...
archlinux   Ready      control-plane   26h   v1.36.2+k3s1   ...node-role.kubernetes.io/control-plane=true...
assyifa     NotReady   data            25h   v1.36.2+k3s1   ...node-role.kubernetes.io/data=...
internal    Ready      internal        25h   v1.36.2+k3s1   ...node-role.kubernetes.io/internal=...
```
Menampilkan seluruh label tiap node untuk membandingkan dengan nodeSelector: role=internal yang diminta pod Redis. Ditemukan bahwa node internal punya role node-role.kubernetes.io/internal=, tapi tidak punya label custom role=internal — label bawaan K3s berbeda dari label custom yang dipakai di nodeSelector. Perlu ditambahkan manual. Terlihat juga node assyifa berstatus NotReady.

## 10. Menambahkan label custom ke node
```
kubectl label node internal role=internal
```
output:
```
node/internal labeled
```
Menambahkan label custom role=internal ke node internal secara manual, agar cocok dengan nodeSelector di manifest Redis.

## 11. Verifikasi label sudah terpasang
```
kubectl get nodes --show-labels
```
Mengecek ulang label node internal yang hasilnya sekarang mencantumkan tambahan role=internal di kolom LABELS

## 12. Verifikasi akhir: pod Redis berhasil running
```
kubectl get pods -o wide
```
output:
```
NAME                                    READY   STATUS    RESTARTS   AGE     IP          NODE       NOMINATED NODE   READINESS GATES
redis-847fc8d5f6-sktpk                  1/1     Running   0          6m50s   10.42.2.5   internal   <none>           <none>
slims-db-pod                            1/1     Running   0          127m    10.42.2.4   internal   <none>           <none>
slims-web-deployment-5f9fdb8fb5-l9c6n   1/1     Running   0          127m    10.42.3.3   amelia     <none>           <none>
slims-web-deployment-5f9fdb8fb5-pc2pd   1/1     Running   0          127m    10.42.2.3   internal   <none>           <none>
```
Setelah label ditambahkan, K3s langsung menempatkan pod Redis ke node internal (sesuai nodeSelector) dan statusnya berubah dari Pending menuju Running dengan READY 1/1, IP 10.42.2.5. Masalah scheduling berhasil diselesaikan.

## 13. Keluar 
```
exit
```
