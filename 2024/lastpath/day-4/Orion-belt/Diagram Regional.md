# Diagram Regional

<img width="341" height="291" alt="Diagram Regional" src="https://github.com/user-attachments/assets/69d928f3-f233-44fa-b312-55b739056a5d" />


Diagram regional dibuat agar dapat menunjukkan bagaimana fungsi sistem dibagi ke dalam berbagai wilayah atau segmen layanan. Setiap daerah punya peran yang berbeda, sehingga sistem lebih teratur, aman, dan mudah dikembangkan. Dengan pemisahan ini, masalah di satu area tidak akan langsung mengganggu seluruh sistem.
## 1. Regional Control
Regional Control adalah pusat pengelolaan semua cluster Kubernetes atau K3s. Wilayah ini bisa dianggap sebagai "otak" dari sistem karena bertugas mengatur semua kegiatan yang terjadi di dalam kluster. Control bertugas menentukan di mana aplikasi dijalankan, memantau keadaan setiap node, mengatur komunikasi antar layanan, serta memastikan semua bagian sistem selalu berjalan dengan baik.    
Fungsi Regional Control yaitu mengelola cluster Kubernetes/K3s, menjadwalkan pod ke node yang tersedia, mengelola konfigurasi cluster, mengontrol komunikasi antar node, dan mengawasi kesehatan layanan.
## 2. Regional Data
Regional Data adalah tempat penyimpanan semua informasi yang digunakan oleh aplikasi. Wilayah ini menyimpan informasi penting, jadi harus memiliki tingkat perlindungan dan ketepatan yang sangat tinggi. Data yang disimpan bisa berupa database, file pengguna, dokumen, cadian sistem, atau data aplikasi lainnya. Fungsi Regional Data adalah menyimpan data aplikasi, menyediakan database untuk layanan, menyimpan file dan dokumen, dan menyimpan backup dan arsip data.
## 3. Regional Internal
Layanan dalam Regional Internal hanya bisa digunakan oleh administrator dan tidak bisa diakses langsung oleh pengguna biasa. Wilayah ini berperan dalam mendukung jalannya sistem operasional dan membantu dalam proses pengawasan. Biasa digunakan untuk memantau, mencatat log, observasi, serta pengelolaan infrastruktur. Fungsi Regional Internal adalah monitoring kondisi server, mengumpulkan log aplikasi, menyediakan dashboard administrasi, membantu proses troubleshooting, dan mendukung keamanan sistem.
## 4. Regional Public
Regional Public berperan sebagai pintu utama yang menghubungkan pengguna dari luar jaringan dengan sistem yang ada di dalam. Semua koneksi dari internet harus melewati wilayah ini terlebih dahulu sebelum sampai ke layanan yang ada di dalam kelompok tersebut.  Layanan di tingkat regional umumnya mencakup Nginx, HAProxy, Reverse Proxy, Load Balancer, atau Ingress Controller yang bertugas mengelola aliran jaringan.  
Fungsi Regional Public  adalah unutk menerima seluruh akses dari pengguna, menyediakan alamat atau domain aplikasi, melakukan load balancing terhadap trafik yang masuk, dan mencegah akses langsung ke layanan internal agar tetap aman.
## 5. Regional Client
Regional Client merupakan titik awal pengguna memulai interaksi dengan sistem. Wilayah ini mencakup perangkat yang digunakan oleh pengguna maupun administrator untuk mengakses layanan yang tersedia. Perangkat client bisa berupa laptop, komputer meja, ponsel, atau tablet yang terhubung ke jaringan. Pengguna tidak berkomunikasi langsung dengan server, melainkan mengirimkan permintaan melalui antarmuka aplikasi atau browser.  
Fungsi Regional Client dapat mengakses layanan yang tersedia pada sistem, mengirimkan permintaan (request) ke server, menampilkan hasil pemrosesan data kepada pengguna,dan menjadi media interaksi antara pengguna dan aplikasi.

Diagram daerah menunjukkan bagaimana sistem dibagi menjadi beberapa daerah, yaitu Data, Internal, Public, dan Client, masing-masing berdasarkan fungsi mereka. Pembagian ini dilakukan agar keamanan meningkat, pengelolaan layanan lebih mudah, dan komunikasi antar bagian sistem menjadi lebih rapi dan terorganisir. Dengan desain seperti ini, setiap area memiliki tugas yang jelas, sehingga sistem menjadi lebih kuat, cepat, dan mudah diperbaiki di masa depan.
