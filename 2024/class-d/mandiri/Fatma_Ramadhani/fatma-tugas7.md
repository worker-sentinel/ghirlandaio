## Nama: Fatma Ramadhani
## NIM: 12402051050157
## Mata Kuliah: Perpustakaan dan Arsip Digital
## Dosen Pengampu: Al Muhdil Karim, S. IP., M.Hum
## Penjelasan bersumber dari dosen Perpustakaan dan Arsip Digital

## Boot (Booster): 
Aplikasi ini fungsinya sebagai generator atau kernel/initramfs. Jadi pas kita pencet tombol power, setelah lewat BIOS dan bootloader, masuklah ke proses ini. Tugas utamanya itu buat ngerubah bahasa manusia (huruf/angka) jadi bahasa mesin atau biner (0 dan 1) supaya bisa dibaca sama prosesor.
## Disk Encryption (LUKS on LVM): 
LUKS itu aplikasi buat enkripsi di layer hardware, jadi hard disk-nya yang dikunci, bukan cuma datanya. Kalau hard disk-nya dicabut terus dipasang di laptop lain, datanya nggak bakal bisa dibaca selama nggak tahu password-nya. Nah, kalau LUKS on LVM, ini strategi yang cocok buat sistem kearsipan. Kenapa? Karena di arsip itu nggak semua data harus dikunci. Kita bisa milih partisi mana aja yang rahasia buat di-enkripsi, jadi lebih fleksibel.
## Desktop (XFCE): 
Ini salah satu pilihan Desktop Environment atau "muka" buat komputer kita. Kalau mau yang simpel pakainya GNOME, kalau mau yang agak fancy pakainya Plasma. Nah, XFCE ini alternatif lainnya kalau teman-teman mau ganti tampilan kalau sudah bosan.
## File Manager (Superfile): 
Ini itu kayak Windows Explorer-nya terminal. Karena nanti kita bakal ngurusin server yang nggak ada tampilannya (CLI), kita butuh alat ini buat navigasi file di terminal tanpa harus pakai UI yang ribet.
## Disk Layout (CIS): 
CIS itu adalah protokol atau standar keamanan. Isinya aturan-aturan kayak partisi /var, /var/log, dan /tmp itu harus dipisah. Tujuannya buat menjaga stabilitas sistem dan memaksa pengamanan maksimal supaya kalau ada malware yang ter-download, dia nggak bisa langsung bekerja. Selain itu, CIS ini penting buat standarisasi; jadi mau punya ribuan server pun, konfigurasinya seragam dan gampang diaudit.
## Container (Podman, Podman Desktop): 
Podman aslinya itu jalan lewat CLI (perintah teks). Nah, Podman Desktop itu dikeluarin supaya ada tampilannya (UI), jadi teman-teman tinggal klik-klik aja buat konfigurasi dan nggak harus hafal semua perintah satu-satu.
## Firewall (IP Tables): 
Anggap aja komputer itu rumah yang punya 65.000 pintu (port). IP Tables ini penjaga pintunya. Dia yang ngatur pintu mana yang boleh dibuka dan mana yang harus ditutup. Misalnya kita cuma butuh akses web (port 443), ya sisanya ditutup aja semua supaya nggak ada celah buat orang luar masuk ke sistem kita.
## Multimedia (MPD, MPC, MPV): 
Ini alat buat multimedia di terminal. Fungsinya keren, teman-teman bisa nonton YouTube tanpa iklan dan itu gratis. Jadi kalau lagi malas buka browser, bisa langsung jalanin dari terminal aja.
## Password (KeePass XC): 
Ini buat manajemen password server-server kita. Di dalamnya kita bisa simpan database password yang terenkripsi. Malah algoritmanya sudah dukung buat jadi autentikator (kayak Google Authenticator), jadi bisa buat simpan kode OTP juga.
## Key (Secret): 
Fungsinya mirip sama KeePass, tapi lebih buat otomasi SSH key. Jadi pas kita login user, kuncinya langsung di-load otomatis. Pas kita mau push data ke server, kita nggak perlu input password atau kunci secara manual lagi karena sudah diambil otomatis sama aplikasi ini.
## Accsess (OpenSSH): 
Ini standar industri buat ngendaliin atau ngeremote komputer lain dari jarak jauh secara aman. Jadi kita bisa connect dari laptop kerja kita ke server di tempat lain tanpa harus pakai metode password yang nggak aman.

Intinya aplikasi-aplikasi di atas dipakai buat mastiin standar keamanan kita kuat. Seperti yang kita tahu, dalam dunia security itu, prinsipnya lebih baik data itu hancur atau hilang daripada bocor ke tangan orang yang nggak berwenang.

