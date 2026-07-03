
![[Tipologi Jaringan.jpg]]

Topologi jaringan dalam proyek ini menggunakan konsep segmentasi jaringan dengan WLAN (Wireless Local Area Network) untuk memisahkan berbagai layanan berdasarkan fungsi dan tingkat keamanan yang berbeda. Infrastruktur terbagi menjadi beberapa area utama, yaitu Data, Internal, Public, dan Client, yang saling terhubung menggunakan perangkat jaringan. Selain itu, ada Administrator yang memiliki akses untuk mengelola semua layanan yang berjalan di dalam sistem. Pemisahan jaringan ini bertujuan untuk membuat lebih aman, memudahkan pengelolaan server, mengurangi kemungkinan akses yang tidak diizinkan, serta memastikan setiap layanan berjalan di lingkungan yang cocok dengan perannya.

Dalam topologi, WLAN digunakan untuk memisahkan lalu lintas jaringan berdasarkan jenis layanan yang digunakan.  
  
**WLAN 1 (Public)  
  
WLAN 1 digunakan untuk layanan yang berhubungan langsung dengan pengguna atau klien. Semua akses pengguna akan masuk melalui jaringan ini.  Penggunaan WLAN 1 adalah untuk memisahkan trafik pengguna dari trafik server internal, mempermudah pengelolaan akses pengguna, dan mengurangi kemungkinan pengguna mengakses jaringan backend secara langsung.

**WLAN 0 (Internal)  

WLAN 0 digunakan untuk komunikasi antar server secara internal dan juga untuk kegiatan administrasi sistem.  Penggunaan WLAN 0 adalah untuk memisahkan trafik administrasi dari trafik pengguna, meningkatkan keamanan jaringan, dan mempermudah monitoring dan pengelolaan server.

## Alur Komunikasi Jaringan

Dalam topologi ini, komunikasi jaringan tidak terjadi langsung dari klien ke data. Semua orang harus menggunakan jalur yang sudah ditetapkan untuk mengakses sesuatu.

Alur Akses Pengguna : Client → Public. Pengguna mengakses aplikasi melalui jaringan Public.

Alur Komunikasi Server: Public → Internal. Saat aplikasi membutuhkan layanan di bagian belakang, komunikasi dilanjutkan ke jaringan internal.

Alur Akses Data: Internal → Data. Server internal kemudian berkomunikasi dengan server data untuk mengambil atau menyimpan informasi.

Alur Balik Data: Data → Internal → Public → Client. Hasil yang sudah diproses dikirim kembali kepada pengguna melalui jalur yang sama.

Akses Control. Administrator bisa mengakses semua bagian jaringan untuk mengelola dan merawat sistem.
Admin → Data
Admin → Internal
Admin → Public

Akses ini diperlukan untuk konfigurasi server, monitoring layanan, backup dan pemulihan data, dan troubleshooting jaringan.

Topologi jaringan ini menerapkan struktur berlapis dengan membagi layanan menurut fungsinya dan tingkat keamanannya. Data regional berperan sebagai tempat menyimpan informasi, internal berfungsi sebagai penghubung dan pengelola layanan di bagian belakang sistem, public memberikan layanan yang bisa diakses oleh pengguna, dan client adalah pengguna akhir dari sistem tersebut. Administrator dapat mengakses semua wilayah untuk mengelola dan merawat infrastruktur. Dengan memanfaatkan WLAN dan membagi area secara regional, sistem menjadi lebih aman, lebih mudah dikelola, lebih stabil, serta mendukung pertumbuhan layanan perpustakaan digital berbasis SLiMS dan K3s di masa depan.