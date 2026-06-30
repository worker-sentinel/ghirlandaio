# Set Firewall

```
sudo systemctl status firewalld
```
> Mengecek apakah service firewalld lagi jalan (active) atau mati. ```systemctl status``` adalah command untuk mengecek status service apapun di sistem.

```
sudo firewall-cmd --list-all-zone
```
> Menampilkan semua zone yang ada di firewalld beserta service apa saja yang diizinkan di setiap zone. Firewalld mempunya beberapa zone, yaitu:
> 1. drop
> 2. block
> 3. public
> 4. external
> 5. dmz
> 6. work
> 7. home
> 8. internal
> 9. trusted

```
sudo firewall-cmd --zone=[nama_zone] --remove-service={service1,service2} --permanent
```
> Untuk menghapus service yang tidak perlu dari zona tertentu
> 1. ```--zone=``` untuk memilih zone mana yang mau kita edit
> 2. ```--remove-service=``` service mana yang mau dihapus dari zone tersebut
> 3. ```{}``` digunakan apabila service yang ingin dihapus lebih dari satu (misal: ```{dhcvpv6-client,samba,ssh}```, namun kalau cuman satu tidak perlu kurung kurawal
> 4. ```--permanent``` agar perubahannya menjadi permanent di config, bukan hanya sementara

> Setelah melakukan perubahan, maka perlu dilakukan reload dengan cara mengetik command:
```
sudo firewall-cmd --reload
```

> Untuk mengecek satu zona yang spesifik
```
sudo firewall-cmd --info-zone=[nama zone]
```

> Untuk mengecek keseluruhan apakah perubahan yang kita lakukan sudah berhasil, maka cek kembali seluruh zone dengan command:
```
sudo firewall-cmd --list-all-zone
```
