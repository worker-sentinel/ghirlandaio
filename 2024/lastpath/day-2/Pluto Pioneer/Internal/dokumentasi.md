# Dokumentasi regional internal 
**pertama-tama, kita akan singkronisasi waktu**
```
sudo timedatectl set-ntp true
```
Command ini tuh berfungsi untuk mengaktifkan sinkronisasi waktu secara otomatis menggunakan NTP (Network Time Protocol). Kenapa ini penting? Karena semua node di dalam cluster harus punya waktu yang sinkron. Kalau waktunya beda-beda, nanti bisa muncul masalah pas komunikasi antar node atau bahkan gagal join ke cluster.

**habis itu, kitaa**
```
curl -sfL https://get.k3s.io | K3S_URL="https://(ip punya server):6443" K3S_TOKEN="(paste token dari admin/control)" sh -s - pluto
```
Nah, command yang ini kita gunakan untuk menghubungkan server kita ke cluster yang udah dibuat sebelumnya.
Kalau command ini berhasil dijalankan, berarti node kalian udah berhasil bergabung ke dalam cluster.

**kalau node kalian sudah berhasil bergabung di dalam cluster, beri label yang sudah disesuaikan dengan peran yang temen-temen gunakan. semisal disini aku adalah internal, maka commandnya adalah:**
```
sudo kubectl label node pluto role=internal
```
Kalau role kalian bukan internal, tinggal ganti aja tuh bagian internal sesuai role yang dipakai, misalnya data, public, atau role lainnya. Jadi nanti Kubernetes tuh bakalan bisa membedakan fungsi dari masing-masing node di dalam cluster itu sendiri
