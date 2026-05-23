download ISO Arch Linux berikut linknya

masuk ke live environment
<img width="4080" height="2296" alt="20260518_123135" src="https://github.com/user-attachments/assets/b99fa777-fffe-40ae-86cf-de6e1e72859e" />

verify boot mode
```
cat /sys/firmware/efi/fw_platform_size
```

cek koneksi internet
<img width="4080" height="2296" alt="20260518_123610" src="https://github.com/user-attachments/assets/3a2ca08b-ab25-48db-afa9-ed527cadd3e4" />
```
iwctl
```
untuk mencari nama list wireless device
```
device list
```
inisiasi scan untuk jaringan
```
station wlan0 scan
```
list seluruh jariangan yang tersedia
```
station wlan0 get-networks
```
konek ke jaringan
```
station wlan0 connect ssid
```
update system jam
```
timedatectl
```
partisi disk

<img width="2296" height="4080" alt="20260518_123652" src="https://github.com/user-attachments/assets/8b34e0ae-ca39-408b-b90c-8efe68b3626a" />
```
cfdisk /dev/sda
```
<img width="4080" height="2296" alt="20260518_124124 (1)" src="https://github.com/user-attachments/assets/0d86693c-2acf-4c48-adad-13885766d740" />
ubah type disk dengan menekan....
<img width="4080" height="2296" alt="20260518_124119 (1)" src="https://github.com/user-attachments/assets/188841dd-9e8f-4cb0-bd50-71fc21d2a185" />

ubah sesuai layout ini

ketik lsblk -l untuk melihat apakah sudah benar

format partisi

untuk boot
<img width="4080" height="2296" alt="20260518_130012" src="https://github.com/user-attachments/assets/0d649137-1399-4a9d-8035-228931baa725" />

untuk root
<img width="4080" height="2296" alt="20260518_130209" src="https://github.com/user-attachments/assets/9a8ab672-71b2-40e4-bb59-21d3151d057c" />

untuk swap

mount



