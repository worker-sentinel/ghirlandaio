## Bagian kesalahan 
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-05 at 11 31 46" src="https://github.com/user-attachments/assets/6cd0a447-5f23-4d21-99da-ba6544f438c1" />

pada saat selesai install dan ingin reboot, tapi linux nya malah tidak muncul 

## Penyelesaian 
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-05 at 11 31 47" src="https://github.com/user-attachments/assets/0e49a90c-645e-4975-ae64-527adc72e880" />

pada gambar diatas, saya melakukan kesalahan pada saat mounting 
```
mount /dev/wardah /mnt/boot
```
```
lsblk
```

kemudian
<img width="1600" height="1200" alt="WhatsApp Image 2026-06-05 at 11 31 46 (1)" src="https://github.com/user-attachments/assets/16e2d5de-7abb-414f-91b5-f00dbb18e6b8" />

```
bootctl --path=/mnt/boot install
```
lalu setelah itu reboot dan tinggal login seperti gambar dibawah ini 
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-05 at 11 31 46 (2)" src="https://github.com/user-attachments/assets/511d02fd-b63a-4196-8d9c-5309942e2d7b" />

dan muncul gambar seperti ini 
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-05 at 11 48 26" src="https://github.com/user-attachments/assets/964ef3d6-876f-4e51-99f6-37bf66e7bca3" />

## SELESAI 
