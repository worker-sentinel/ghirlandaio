# Resume Konsep Availability dalam Keamanan Siber Perpustakaan Digital
## Nama: Amanda Dwi Rahayu
## NIM: 12402051030100
## Kelas: 4D
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum
---
### Pendahuluan
Resume ini disusun sebagai tindak lanjut atas pertanyaan yang diajukan dalam sesi diskusi kelompok 4 mengenai presentasi berjudul “Etika Keamanan Siber Pada Perpustakaan dan Lembaga Arsip Digital: Tantangan dan Strategis.” Pertanyaan yang diajukan adalah: “Apa yang dimaksud dengan availability?” Adapun jawaban yang diberikan adalah: “Availability (Ketersediaan) adalah jaminan bahwa sistem dan data perpustakaan digital selalu bisa diakses oleh pengguna yang berwenang kapan pun mereka butuhkan, tanpa ada gangguan dari pihak yang tidak bertanggung jawab.”

Resume ini dibuat karena jawaban yang diberikan masih terlalu singkat dan belum menjelaskan secara mendalam. Konsep availability sebenarnya tidak sesederhana definisi saja, tapi juga berkaitan dengan bagaimana sistem bisa tetap berjalan dan diakses dengan baik. Oleh karena itu, jawaban tersebut perlu dikembangkan dalam bentuk resume agar lebih jelas, lebih lengkap, dan mudah dipahami dalam konteks perpustakaan digital.

### Pembahasan
#### Konteks
Availability (Ketersediaan) merupakan salah satu dari tiga pilar utama dalam keamanan informasi yang dikenal sebagai Triad CIA (Confidentiality, Integrity, Availability). Dalam materi presentasi kelompok 4, availability didefinisikan sebagai “memastikan bahwa sistem perpustakaan dan datanya tetap dapat diakses oleh pengguna yang berwenang saat dibutuhkan tanpa gangguan yang tidak sah.” Definisi ini menempatkan availability sebagai tanggung jawab aktif institusi, bukan kondisi yang terjadi dengan sendirinya.

Dalam konteks keamanan siber perpustakaan digital, ancaman nyata yang dapat mengganggu availability antara lain adalah serangan Denial of Service (DoS) dan Distributed Denial of Service (DDoS) yang dapat melumpuhkan server perpustakaan dengan membanjirinya menggunakan lalu lintas data berbahaya, serangan ransomware yang mengenkripsi koleksi digital sehingga layanan tidak dapat beroperasi, hingga kegagalan infrastruktur akibat perangkat lunak usang atau konfigurasi jaringan yang keliru (Azibaev et al., 2026).

Penelitian Azibaev et al. (2026) menunjukkan bahwa terminal OPAC memiliki Skor Risiko Keamanan Siber (CRS) tertinggi sebesar 8,7 justru karena sifatnya yang terbuka untuk publik sebuah kondisi yang dirancang by design untuk mendukung availability. Ini membuktikan adanya paradoks: semakin mudah sistem diakses (tinggi availability), semakin besar pula permukaan serangannya. Oleh karena itu, memahami availability tanpa mempertimbangkan manajemen risiko merupakan pemahaman yang tidak lengkap.

#### Opini
Saya setuju dengan jawaban saya yang mengatkan bahwa availability adalah jaminan akses bagi pengguna berwenang tanpa gangguan.  Namun, jawaban tersebut perlu diperkuat dengan tiga argumen ilmiah berikut:
1. Perspektif Filosofi Perpustakaan: Lima Hukum Ranganathan

Hukum-hukum Ranganathan memberikan fondasi filosofis yang kuat untuk memahami mengapa availability bukan sekadar kebutuhan teknis, melainkan kewajiban etis institusional perpustakaan.

- Hukum Pertama: "Buku adalah untuk digunakan" Jika sistem perpustakaan digital tidak tersedia (down), maka informasi yang tersimpan di dalamnya tidak memiliki nilai guna. Ranganathan menegaskan bahwa tujuan utama aktivitas preservasi adalah untuk mendorong penggunaan, bukan sekadar penyimpanan. Sistem yang tidak available secara langsung melanggar semangat Hukum Pertama ini.
- Hukum Keempat: "Hemat waktu pembaca" Hukum ini menuntut tidak adanya jeda waktu antara permintaan pengguna dan penyediaan dokumen. Serangan siber yang mengganggu availability seperti ransomware yang pernah dilaporkan menyebabkan hilangnya hari-hari layanan pada perpustakaan yang memiliki koleksi digital secara langsung bertentangan dengan hukum ini.
- Hukum Kelima: "Perpustakaan adalah organisme yang berkembang" Di era digital, WEBOPAC memungkinkan pengguna mengakses koleksi perpustakaan dari mana saja, kapan saja, 24 jam sehari. Ini merupakan perwujudan Hukum Kelima yang paling nyata: perpustakaan "mendatangi" pengguna, bukan sebaliknya. Gangguan terhadap availability menghentikan pertumbuhan dan adaptasi organisme ini.

2. Perspektif Keamanan Siber: Nilai Instrumental Availability

Dalam kajian etika keamanan siber, availability dipandang sebagai nilai instrumental (instrumental value) artinya, ketersediaan sistem diperlukan agar nilai-nilai lain yang lebih tinggi dapat tercapai. Dalam konteks kesehatan, availability sistem informasi medis diperlukan agar nyawa pasien dapat diselamatkan. Dalam konteks perpustakaan, availability diperlukan agar pengetahuan dapat diakses dan pendidikan dapat berlangsung.

Lebih jauh, ancaman terhadap availability seperti serangan DoS atau ransomware tidak hanya berdampak teknis, tetapi juga berdampak pada kepercayaan institusional. Hilangnya kepercayaan pengguna terhadap sistem perpustakaan merupakan kerugian jangka panjang yang sulit dipulihkan.

3. Perspektif Pemodelan Risiko: Konsentrasi Risiko pada Aset High-Availability

Hasil penelitian empiris menunjukkan bahwa terminal OPAC publik memiliki Skor Risiko Keamanan Siber (CRS) tertinggi (CRS = 8,7) dibandingkan komponen sistem perpustakaan lainnya, justru karena aksesibilitasnya yang tinggi dan ambang batas otentikasi yang rendah. Ini adalah paradoks availability: komponen yang paling "tersedia" untuk pengguna adalah komponen yang paling rentan terhadap ancaman yang dapat menghancurkan ketersediaannya.

Temuan ini memperkuat argumen bahwa mempertahankan availability tidak cukup dengan sekadar "menyalakan sistem," tetapi memerlukan strategi keamanan berlapis yang mencakup segmentasi jaringan, otentikasi multi-faktor (MFA), kontrol akses berbasis peran (RBAC), dan pemantauan real-time melalui sistem SIEM.

Berdasarkan uraian di atas, availability dalam ekosistem keamanan perpustakaan digital mencakup tiga lapisan yang saling berkaitan:
- Lapisan Teknis: mencakup redundansi server, sistem pencadangan data (backup), pembaruan perangkat lunak secara berkala, serta perlindungan titik akhir (endpoint protection) untuk mencegah kegagalan layanan.
- Lapisan Manusia: mencakup pelatihan literasi digital bagi pustakawan dan pemustaka, serta peningkatan kesadaran terhadap ancaman seperti phishing dan rekayasa sosial yang dapat memanfaatkan celah akses publik.
- Lapisan Kebijakan: mencakup penerapan standar internasional (ISO/IEC 27001, NIST), kebijakan kontrol akses berbasis peran (RBAC), serta protokol respons insiden yang terstruktur.

Ketiga lapisan ini sejalan dengan tiga faktor pengembangan yang disebutkan dalam materi presentasi kelompok 4, yaitu faktor infrastruktur teknologi, sumber daya manusia (SDM), dan tata kelola serta kebijakan. Artinya, availability bukan hanya masalah teknis, melainkan juga masalah organisasi dan etika secara bersamaan.

### Kesimpulan
Availability atau ketersediaan adalah jaminan bahwa infrastruktur informasi perpustakaan tetap berfungsi sebagaimana mestinya untuk melayani kebutuhan pemustaka kapan pun dan di mana pun. Konsep ini merupakan titik temu antara etika pelayanan perpustakaan yang mengutamakan akses universal terhadap pengetahuan sebagaimana diabadikan dalam Lima Hukum Ranganathan dengan teknik keamanan siber yang melindungi sistem dari gangguan ilegal dan ancaman digital.

Tanpa jaminan availability, sebuah perpustakaan digital gagal memenuhi tujuan dasarnya. Lebih dari sekadar "sistem yang menyala," availability adalah komitmen institusional untuk memastikan bahwa pengetahuan selalu dapat dijangkau oleh mereka yang membutuhkannya sebuah prinsip yang sekaligus mencerminkan nilai etis perpustakaan sebagai institusi publik yang bertanggung jawab di era digital.

### Referensi
1. Azibaev, A. G., Madrahimova, G., Mubarakova, D., AbdulJaleel, H., Mullabaeva, N., Mavlyanova, U., & Davlatova, R. (2026). Pemodelan risiko keamanan siber dalam infrastruktur informasi perpustakaan. Indian Journal of Information Sources and Services, 16(1), 30–41.
2. Christen, M., Gordijn, B., & Loi, M. (Eds.). (2020). The ethics of cybersecurity. Springer. Sebagaimana dikutip dalam materi presentasi Kelompok 4: Etika Keamanan Siber Pada Perpustakaan dan Lembaga Arsip Digital.
3. Ranganathan, S. R. (1931). Five laws of library science. Madras Library Association. Sebagaimana dikutip dalam modul Library and Information Science, Module 1. The National Institute of Open Schooling (NIOS).
