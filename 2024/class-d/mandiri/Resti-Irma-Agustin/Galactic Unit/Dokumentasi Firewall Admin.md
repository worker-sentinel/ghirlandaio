Firewall admin
Koneksi admin ke server
ssh F123@10.38.202.71
masukkan password server 
1.	Melihat Konfigurasi Firewall Saat Ini
sudo firewall-cmd --list-all-zone
masukkan password server 

2.	Menghapus Service DHCP dari Zona nm-shared
sudo firewall-cmd –zone=nm-shared –permanent –remove-service=dhcp
sudo firewall-cmd –-reload
3.	Memverifikasi Konfigurasi Firewall
sudo firewall-cmd --list-all-zone
4.	Menghapus Service DNS dan SSH dari Zona nm-shared
sudo firewall-cmd --zone=nm-shared --permanent --remove-service=dns
sudo firewall-cmd –zone=nm-shared –permanent –remove-service=ssh
sudo firewall-cmd –-reload

5.	Menambahkan Service HTTP pada Zona Public
sudo firewall-cmd --zone=public --add-service=http –permanent
6.	Membuka Port
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
7.	Keluar sesi 
exit.
