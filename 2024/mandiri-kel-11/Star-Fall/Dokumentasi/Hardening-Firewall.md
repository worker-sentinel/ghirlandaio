# Menghapus service yang tidak diperlukan 

```
sudo systemctl status firewalld
```
```
firewall--cmd --list-all-zone
```
```
firewall-cmd  --zone=(zona yang akan diremove) --remove(elemen yang akan diremove)=(elemen yang akan diremove) --permanent
```
> hapus semua di elemen services dan sisakan 1 elemen service (SSH) di zona public)
```
firewall-cmd--reload
```
```
firewall-cmd --info-zone=work
```
