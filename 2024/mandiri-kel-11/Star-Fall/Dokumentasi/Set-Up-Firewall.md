## Di server 1

Menentukan bahwa port tersebut untuk ip address tertentu 
```bash
Sudo firewall-cmd --zone=public --add-rich-rule=’rule family=”ipv4” source address=”ipadmin” port port=”22” protocol=”tcp” accept’
```

```bash 
sudo firewall-cmd --reload
```

## Di Server 2

Lakukan langkah yang sama di server 2

```bash
Sudo firewall-cmd --zone=public --add-rich-rule=’family=”ipv4” source address=”ipadmin” port port=”80” protocol=”tcp” accept’
```

```bash 
sudo firewall-cmd --reload
```
