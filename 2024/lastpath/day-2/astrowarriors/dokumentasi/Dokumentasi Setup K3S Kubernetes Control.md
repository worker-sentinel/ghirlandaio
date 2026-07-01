# Dokumentasi Setup Cluster K3s

## Control Plane / Admin

### 1. Install K3s Server

```bash
curl -sfL https://get.k3s.io | sh -
```

Menginstal K3s sebagai **control plane** yang bertugas mengelola seluruh cluster Kubernetes.

Mengunduh dan menjalankan skrip instalasi resmi K3s untuk membuat control plane Kubernetes. Control plane bertanggung jawab mengelola seluruh cluster, seperti penjadwalan Pod, penyimpanan status cluster (etcd/SQLite), API Server, serta komunikasi dengan worker node. Penggunaan installer resmi juga membantu memastikan komponen Kubernetes terpasang dengan konfigurasi yang konsisten dan mudah dipelihara, sesuai praktik deployment server yang direkomendasikan dalam panduan keamanan sistem.

(masukkan password setelah command yang pertama)

(lalu cek ip a)

---

### 2. Menampilkan Node Token

```bash
sudo cat /var/lib/rancher/k3s/server/node-token
```

Menampilkan token yang digunakan worker node untuk bergabung ke cluster.

Menampilkan node token yang digunakan sebagai mekanisme autentikasi ketika worker node ingin bergabung ke dalam cluster. Token ini memastikan hanya node yang memiliki kredensial yang valid yang dapat melakukan proses join, sehingga membantu mencegah perangkat yang tidak berwenang menjadi bagian dari cluster Kubernetes. Prinsip ini sejalan dengan rekomendasi CIS mengenai pengendalian akses dan autentikasi pada sistem.

---

### 3. Memastikan Worker Berhasil Join

```bash
kubectl get nodes
```

Menampilkan seluruh node yang terdaftar pada cluster. Apabila worker berhasil bergabung, statusnya akan **Ready**.

Menampilkan daftar seluruh node yang telah terdaftar pada cluster beserta status, peran (role), dan versi Kubernetes yang digunakan. Pemeriksaan ini merupakan proses verifikasi untuk memastikan worker node telah berhasil bergabung dan berada pada status Ready, sehingga dapat menerima penjadwalan workload dari control plane. Verifikasi konfigurasi setelah proses deployment merupakan bagian dari praktik audit yang dianjurkan oleh CIS sebelum sistem digunakan dalam lingkungan produksi.

Contoh output:

```
NAME        STATUS   ROLES           VERSION
backlink    Ready    control-plane   v1.36.x
archlinux   Ready    <none>          v1.36.x
```

---

### 4. Install kubectl

```bash
pacman -S kubectl
```

Menginstal utilitas **kubectl** untuk mengelola resource Kubernetes.

Menginstal kubectl, yaitu utilitas command line resmi Kubernetes yang digunakan administrator untuk berkomunikasi dengan Kubernetes API Server. Melalui kubectl, administrator dapat membuat, mengubah, menghapus, maupun memantau seluruh resource dalam cluster secara terpusat. Pemisahan utilitas administrasi dan penggunaan alat resmi membantu menjaga konsistensi pengelolaan sistem sesuai praktik administrasi yang baik.

---

### 5. Membuat Manifest Deployment

```bash
nvim slims-multi-node.yaml
```

Membuat file manifest Kubernetes yang berisi konfigurasi deployment dan service aplikasi SLiMS beserta database.

Membuat file manifest Kubernetes dalam format YAML yang berisi deklarasi konfigurasi resource, seperti Deployment, Service, maupun komponen lain yang diperlukan aplikasi SLiMS dan database. Pendekatan deklaratif ini memudahkan proses audit, dokumentasi, serta reproduksi konfigurasi karena seluruh pengaturan cluster tersimpan dalam sebuah berkas yang dapat ditinjau maupun dikelola menggunakan sistem version control. Praktik dokumentasi konfigurasi seperti ini sejalan dengan pendekatan konfigurasi yang direkomendasikan dalam CIS Benchmark.

---

### 6. Deploy Manifest

```bash
kubectl apply -f slims-multi-node.yaml
```

Menerapkan seluruh resource yang terdapat pada file manifest ke dalam cluster Kubernetes.

Menerapkan seluruh konfigurasi yang terdapat pada file manifest ke dalam cluster Kubernetes. Kubernetes akan membandingkan kondisi aktual cluster dengan konfigurasi yang didefinisikan pada manifest, kemudian membuat atau memperbarui resource agar sesuai dengan keadaan yang diinginkan (desired state). Pendekatan ini mendukung pengelolaan infrastruktur secara konsisten dan memudahkan proses pemeliharaan maupun audit konfigurasi.

---

### 7. Verifikasi Pod

```bash
kubectl get pods -o wide
```

Menampilkan status Pod beserta node tempat Pod dijalankan untuk memastikan deployment berhasil.

Menampilkan daftar Pod beserta informasi tambahan seperti alamat IP, status, dan node tempat Pod dijalankan. Informasi ini digunakan untuk memastikan seluruh Pod berhasil dibuat, berada pada status Running, serta telah terdistribusi ke node yang sesuai. Verifikasi hasil deployment merupakan bagian dari proses monitoring konfigurasi yang direkomendasikan oleh CIS agar administrator dapat memastikan layanan berjalan sesuai konfigurasi yang telah diterapkan.

Contoh hasil:

```
slims-db-pod                 Running
slims-web-deployment-xxxxx   Running
slims-web-deployment-yyyyy   Running
```

Output juga menunjukkan salah satu Pod berjalan pada node archlinux, yang menandakan mekanisme penjadwalan (scheduler) Kubernetes berhasil mendistribusikan workload ke worker node dalam cluster.
