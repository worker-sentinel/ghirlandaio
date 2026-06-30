# Rich Rule

```
sudo systemctl enable --now firewalld
```

```
sudo firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="20.20.20.20" port port="22" protocol="tcp" accept'
```
> server 1 & 2

```
sudo firewall-cmd --zone=public --permanent --add-rich-rule='rule family="ipv4" source address="18.18.18.18" port port="80" protocol="tcp" accept'
```
> server 1

```
sudo firewall-cmd --zone=public --permanent --add-port="80"/tcp
```
> server 2

```
sudo firewall-cmd --reload
```

```
sudo firewall-cmd --info-zone=public
```
