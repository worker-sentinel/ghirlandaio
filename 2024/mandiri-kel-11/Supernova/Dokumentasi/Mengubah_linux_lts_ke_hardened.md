## Mengubah linux lts ke hardened
```
sudo su
```
```
pacman -S linux-hardened linux-hardened-headers
```
```
ls -l /etc/mkinitcpio.d/*.conf
```
<img width="1280" height="720" alt="WhatsApp Image 2026-06-25 at 22 15 27" src="https://github.com/user-attachments/assets/10408c04-6ffb-4e65-9c5c-38938e4a23ef" />

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```
<img width="1280" height="720" alt="WhatsApp Image 2026-06-25 at 22 15 42" src="https://github.com/user-attachments/assets/55dc78d3-22e1-4b60-974f-cbe259ae182c" />

```
mkinitcpio -P
```
```
reboot
```
```
uname -r
```
