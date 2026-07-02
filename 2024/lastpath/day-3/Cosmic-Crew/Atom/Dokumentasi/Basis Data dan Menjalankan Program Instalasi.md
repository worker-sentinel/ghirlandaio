# Buat Basis Data

Buat database atom dengan charset utf8mb4, menggunakan kata sandi yang dibuat saat [instalasi MySQL](https://www.accesstomemory.org/en/docs/2.10/admin-manual/installation/ubuntu/#installation-ubuntu-dependencies-mysql).

```
sudo mysql -h localhost -u root -p -e "CREATE DATABASE atom CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;"
```

Jika password tidak disertakan setelah `-p`, akan diminta saat perintah dijalankan. Jika disertakan, tulis tanpa spasi (`-pPASSWORD`), meski cara ini kurang aman karena bisa tersimpan di `.bash_history`. Pastikan database dalam kondisi kosong; database lama bisa dihapus dulu dengan `DROP DATABASE` sebelum dibuat ulang.

Buat user MySQL khusus untuk AtoM.

```
sudo mysql -h localhost -u root -p -e "CREATE USER 'atom'@'localhost' IDENTIFIED BY '12345';"
```

Berikan hak akses penuh user tersebut ke database atom.

```
sudo mysql -h localhost -u root -p -e "GRANT ALL PRIVILEGES ON atom.* TO 'atom'@'localhost';"
```

Hak akses `INDEX`, `CREATE`, dan `ALTER` hanya diperlukan saat instalasi/upgrade, dan bisa dicabut setelahnya atau diganti user lain di [config.php](https://www.accesstomemory.org/en/docs/2.10/admin-manual/customization/config-files/#config-config-php).

## Jalankan Installer

Masuk ke direktori AtoM.

```
cd /usr/share/nginx/atom
```

Jalankan installer AtoM.

```
sudo php symfony tools:install
```

Installer akan meminta konfigurasi database dan pencarian. Isi sesuai setup di atas:

- Host basis data: `localhost`
- Port basis data: `3306`
- Nama basis data: `atom`
- Pengguna basis data: `atom`
- Kata sandi basis data: `12345`
- Cari host: `localhost`
- Port pencarian: `9200`
- Indeks pencarian: `atom`

Nilai-nilai ini akan berbeda jika MySQL/Elasticsearch berjalan di server terpisah. Konfigurasi lain (judul situs, deskripsi situs, URL dasar, email admin, username admin, password admin) bisa diisi bebas dan diubah kembali kapan saja lewat Admin > Pengaturan > [Informasi situs](https://www.accesstomemory.org/en/docs/2.10/user-manual/administer/settings/#site-information) atau [edit user](https://www.accesstomemory.org/en/docs/2.10/user-manual/administer/manage-user-accounts/#edit-user). Detail lengkap dan cara otomatisasi ada di [halaman CLI tools](https://www.accesstomemory.org/en/docs/2.10/admin-manual/maintenance/cli-tools/#cli-installer).
