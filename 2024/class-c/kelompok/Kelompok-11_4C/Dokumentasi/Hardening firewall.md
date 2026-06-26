## HARDENING FIREWALL

## masuk mode root

```
sudo su
```

## cek status firewall

```
systemctl status firewalld
```
**Pastikan statusnya harus `active` dan `enabled`**
**Jika disable maka menggunakan command**
```
systemctl enable –now firewall
```

## Melihat semua zona yang ada diperangkat

```
firewall-cmd --list-all-zone
```

## Menghapus service yang tidak diperlukan

```
firewall-cmd --zone=sesuaikanzonanya --remove-service={service yang mau dihapus} --permanent
```
**Contoh zona= work, public, external, internal, nm-shared,dll yang ada di perangkat kalian**

**Sisakan service=ssh dibagian zona public**
*Jika service yang ada lebih dari satu maka gunakan {} jika hanya satu maka tidak perlu menggunakan {}*

## Reload firewall

```
firewall-cmd --reload`
```
**di-reload dulu biar konfigurasinya terpakai**

## Cek ulang

```
firewall-cmd --list-all-zone
```
**untuk mengecek apakah service disetiap zona yang tidak diperlukan sudah terhapus atau belum**

## Selesai
