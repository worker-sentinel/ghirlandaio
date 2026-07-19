## HARDENING FIREWALL

## masuk mode root
Agar diizinkan untuk mengubah system keamanan komputer dan diterima perintahnya dengan menggunakan command
```
sudo su
```

## cek status firewall
Untuk mengecek status firewall sedang aktif atau tidak dengan menggunakan command

```
systemctl status firewalld
```
**Pastikan statusnya harus `active` dan `enabled`**
**Jika disable maka menggunakan command**
```
systemctl enable –now firewall
```

## Melihat semua zona yang ada diperangkat
Untuk melihat wilayah keamanan yang ada di komputer dengan menggunakan command

```
firewall-cmd --list-all-zone
```

## Menghapus service yang tidak diperlukan
Supaya orang asing tidak bisa masuk ke service yang tidak diperlukan menggunakan command

```
firewall-cmd --zone=sesuaikanzonanya --remove-service={service yang mau dihapus} --permanent
```
**Contoh zona= work, public, external, internal, nm-shared,dll yang ada di perangkat kalian**

**Sisakan service=ssh dibagian zona public**
*Jika service yang ada lebih dari satu maka gunakan {} jika hanya satu maka tidak perlu menggunakan {}*

## Reload firewall
Untuk menerapkan aturan keamanan baru dengan menggunakan command

```
firewall-cmd --reload`
```
**di-reload dulu biar konfigurasinya terpakai**

## Cek ulang
Untuk menampilkan Kembali status dan daftar aturan pada seluruh zona firewall dengan menggunakan command
```
firewall-cmd --list-all-zone
```
**untuk mengecek apakah service disetiap zona yang tidak diperlukan sudah terhapus atau belum**

## Selesai
