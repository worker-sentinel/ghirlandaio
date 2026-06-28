# Firewall

```
sudo systemctl restart systemd-networkd
sudo systemctl restart systemd-resolved
sudo systemctl restart iwd
sudo pacman -S firewalld
```

```
sudo systemctl start firewalld
```

```
firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.1.27" port port="22" protocol=tcp accept'
```

```
sudo firewall-cmd --reload
```
