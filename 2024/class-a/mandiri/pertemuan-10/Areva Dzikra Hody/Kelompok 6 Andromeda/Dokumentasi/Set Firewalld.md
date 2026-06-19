# Set Up Firewalld

## Menyambungkan SSH server ke admin
menghubungkan ke satu jaringan koneksi internet yang sama antara admin dan server.

iwctl


device list


station wlan0 get-networks


station wlan0 connect oooal


exit

### Menghubungkan ssh admin dan server
Lakukan ini di admin dan server

systemctl status sshd


systemctl start sshd


systemctl enable sshd


ip a


Lakukan ini di Server

ssh [nama root server]@ip server


### Set Up Firewalld

sudo su


firewall-cmd --list-all-zone

Bersihkan all zone pada port service, kecuali pada zona publik

firewwall-cmd --zone=[nama zona] --remove-service=[portnya] --permanent


firewall-cmd --reload

Setelah itu tambahkan pada zona publik port 

firewwall-cmd --zone=public --add-port=http --permanent


firewwall-cmd --zone=public --add-port=3306/tcp --permanent


firewwall-cmd --zone=public --add-port=443/tcp --permanent


firewwall-cmd --zone=public --add-port=80/tcp --permanent


firewall-cmd --reload


# Set Up Firewalld

## Menyambungkan SSH server ke admin
menghubungkan ke satu jaringan koneksi internet yang sama antara admin dan server.

iwctl


device list


station wlan0 get-networks


station wlan0 connect oooal


exit

### Menghubungkan ssh admin dan server
Lakukan ini di admin dan server

systemctl status sshd


systemctl start sshd


systemctl enable sshd


ip a


Lakukan ini di Server

ssh [nama root server]@ip server


### Set Up Firewalld

sudo su


firewall-cmd --list-all-zone

Bersihkan all zone pada port service, kecuali pada zona publik

firewwall-cmd --zone=[nama zona] --remove-service=[portnya] --permanent


firewall-cmd --reload

Setelah itu tambahkan pada zona publik port 

firewwall-cmd --zone=public --add-port=http --permanent


firewwall-cmd --zone=public --add-port=3306/tcp --permanent


firewwall-cmd --zone=public --add-port=443/tcp --permanent


firewwall-cmd --zone=public --add-port=80/tcp --permanent


firewall-cmd --reload
