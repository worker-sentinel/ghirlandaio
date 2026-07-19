# Dokumentasi Deploy Slims

## 1. Edit konfigurasi registry container
```
nano /etc/containers/registries.conf
```
yang diedit:
```
# # An array of host[:port] registries to try when pulling an unqualified image, in order.             
# unqualified-search-registries = ["docker.io"]
``` 
Membuka file konfigurasi registry containers (CRI-O/Podman) untuk mengatur dari registry mana image container akan ditarik (misalnya menambahkan docker.io sebagai unqualified-search registry). File ini penting di Arch Linux karena container runtime tidak otomatis tahu registry default seperti di Docker.

## 2. Edit manifest deployment SLiMS
```
nano slims-test.yaml
```
Membuka/menulis file manifest Kubernetes slims-test.yaml yang berisi definisi resource: Service dan Pod untuk database (slims-db), serta Deployment dan Service untuk web (slims-web).

## 3. Apply manifest ke cluster
```
kubectl apply -f slims-test.yaml
```

Output:
```
service/slims-db-service created
pod/slims-db-pod created
deployment.apps/slims-web-deployment created
service/slims-web-service created
```
Menerapkan manifest ke cluster K3s. Empat resource berhasil dibuat: service dan pod untuk database, serta deployment (yang otomatis membuat ReplicaSet + Pod) dan service untuk web.

## 4. Cek pod yang berjalan
```
kubectl get pods -o wide
```
Output:
```
NAME                                    READY   STATUS    RESTARTS   AGE     IP          NODE       NOMINATED NODE   READINESS GATES
slims-db-pod                            1/1     Running   0          5m22s   10.42.2.4   internal   <none>           <none>
slims-web-deployment-5f9fdb8fb5-l9c6n   1/1     Running   0          5m22s   10.42.3.3   amelia     <none>           <none>
slims-web-deployment-5f9fdb8fb5-pc2pd   1/1     Running   0          5m22s   10.42.2.3   internal   <none>           <none>
```
Memverifikasi status pod beserta node penempatannya (-o wide menampilkan kolom IP dan NODE). Terlihat scheduler K3s mendistribusikan 2 replika web ke node berbeda (amelia dan internal), sementara db hanya 1 pod di internal. Semua pod dalam status Running dan READY 1/1.


## 5. Cek konfigurasi jaringan node
```
ip a
```
Menampilkan seluruh interface jaringan pada node lokal

## 6. Cek status pod 
```
kubectl get pods -o wide
```

## 7. Exec ke pod 
```
kubectl exec -it slims-web-deployment-5f9fdb8fb5-pc2pd -- ls -la /var/www/html
```
Output:
```
total 43948
drwxrwxrwx  1 www-data www-data     4096 Feb 27  2020 .
drwxr-xr-x  1 root     root         4096 Feb  1  2020 ..
drwxr-xr-x 19 root     root         4096 Feb 14  2020 slims
-rw-r--r--  1 root     root     44986485 Feb 27  2020 slims.zip
```
berhasil masuk ke container web. Isi /var/www/html menunjukkan image sudah berisi source code SLiMS dalam folder slims/ dan arsip slims.zip, tapi belum diekstrak/dipindah ke root direktori web.

## 8. Cek isi web root di pod database
```
kubectl exec -it slims-db-pod -- ls -la /var/www/html
```
Output:
```
ls: cannot access '/var/www/html': No such file or directory
command terminated with exit code 2
Mengecek apakah pod database juga punya direktori /var/www/html — ternyata tidak ada, karena image database (kemungkinan MariaDB/MySQL) memang tidak menjalankan web server, jadi struktur direktorinya berbeda dari image web (PHP/Apache).
```

## 9. Exec ke pod
```
kubectl exec -it slims-web-deployment-5f9fdb8fb5-pc2pd -- ls -la /var/www/html
```


## 10. Memindahkan isi folder slims/ ke web root
```
kubectl exec -it slims-web-deployment-5f9fdb8fb5-pc2pd -- bash -c "mv /var/www/html/slims/* /var/www/html/ && mv /var/www/html/slims/.[!.]* /var/www/html/ 2>/dev/null"
```
Menjalankan command shell di dalam pod (bash -c) untuk memindahkan seluruh isi folder slims/ ke satu level di atasnya (/var/www/html/), yaitu direktori root web server:
- mv /var/www/html/slims/* /var/www/html/ — memindahkan semua file/folder biasa (non-hidden).
- mv /var/www/html/slims/.[!.]* /var/www/html/ — memindahkan file/folder tersembunyi (diawali titik, seperti .htaccess, .gitignore), dengan pola [!.]* agar . dan .. tidak ikut terpindah.
- 2>/dev/null — menyembunyikan pesan error jika tidak ada file hidden yang cocok.

Tujuannya agar SLiMS bisa diakses langsung dari domain/IP root (http://<ip>/) tanpa perlu subfolder /slims/ di URL.

## 11. Verifikasi hasil pemindahan file
```
kubectl exec -it slims-web-deployment-5f9fdb8fb5-pc2pd -- ls -la /var/www/html
```
Output :
```
.gitignore, .htaccess, LICENSE, README.md, admin/, api/, changes.txt,
chatserver.php, composer.json, composer.lock, config/, css/, files/,
help/, images/, index.php, indexing_engine/, install/, js/, lib/, m/,
oai.php, oai2.php, repository/, sample/, simbio2/, slims/ (kosong),
slims.zip, supports.txt, sysconfig.inc.php, template/, ucnode.inc.php,
upgrade/, webicon.ico
```
Konfirmasi bahwa seluruh file inti SLiMS (index.php, admin/, config/, dll.) sudah berhasil dipindah ke /var/www/html/ langsung. Folder slims/ masih tersisa tapi sudah kosong (bisa dihapus jika perlu). File slims.zip masih ada sebagai arsip asli. Struktur ini menandakan aplikasi sudah siap diakses via browser.

## 12. Keluar
```
exit
```
---

