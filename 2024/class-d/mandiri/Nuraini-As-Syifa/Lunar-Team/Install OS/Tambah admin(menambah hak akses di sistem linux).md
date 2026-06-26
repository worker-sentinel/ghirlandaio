## ​1. Konfigurasi Hak Akses Sudo (Sudoers)
Membuat file konfigurasi khusus untuk memberikan hak penuh kepada pengguna admin (menggunakan perintah echo untuk menulis langsung ke dalam direktori sudoers.d).
```bash
echo "admin ALL=(ALL:ALL) ALL" > /etc/sudoers.d/admin

```
## 2. Membuat Pengguna Baru (operator)
Membuat akun pengguna baru bernama operator beserta direktori rumahnya (-m).
```bash
useradd -m operator

```
## 3. Mengatur Kata Sandi Pengguna
Mengatur atau mengubah kata sandi untuk kedua pengguna yang telah dibuat agar bisa digunakan untuk masuk ke sistem.
 * **Untuk pengguna admin:**
   ```bash
   passwd admin
   
   ```
 * **Untuk pengguna operator:**
   ```bash
   passwd operator
   
   ```
## 4. Menambahkan Pengguna ke Grup wheel
Memasukkan pengguna operator ke dalam grup wheel agar memiliki jalur untuk mendapatkan hak akses administratif tambahan di kemudian hari jika diperlukan.
```bash
usermod -aG wheel operator

```

```bash
exit

```
