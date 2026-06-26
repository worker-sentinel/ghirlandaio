# Disable Module Kernel (Asciinema 1)

## Apaya
```
systemctl status networkmanager
sudo systemctl enable NetworkManager
Masukin password kel10
```

```
sudo systemctl start NetworksManager
nmcli device status
-enp0s31f6 (connected)
-lo (connected)
-wlan0 (disconnected)
-p2p-dev-wlan0 (disconnected)
Ip a
```
## Apaya
```
sudo nmcli connection add type ethernet ifname enp0s31f6 con-name novaria-admin ipv4.method manual ipv4.address 9.9.9.2/24 ipv4.gateway 9.9.9.1 ipv4.dns 8.8.8.8
(successfully added)
echo “nmcli connection up Novaria-admin” >> /home/kel10.bash_profile
ls
Desktop   	  app         	dbdata           	  installslims.cast	slims.cast          	test
Downloads   | conf    |   desablekernel.cast  | setup-ip-admin.cast |slims_lanjutan.cast                                   	
akses_slims.cast | db_default.env | docker-compose.yaml | slims-compose | slims_semoga_bisa.cast
```
## LAST STEPS
```
ctrl+d
asciinema upload desablekernel.cast
```
