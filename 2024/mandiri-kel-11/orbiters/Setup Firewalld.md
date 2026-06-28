## Cek status apakah firewalld sudah aktif
```
systemctl status firewalld
```

## Cek semua list zone firewall yang ada
```
firewall-cmd --list-all-zone
```

## Hapus service pada zone work
```
firewall-cmd --zone=work --remove-service={dhcpv6-client,ssh} --permanent
```

## Reload firewall
```
firewall-cmd --reload
```

## Cek kembali 
```
firewall-cmd --list-all-zone
```

## Hapus service zone public
```
firewall-cmd --zone=public --remove-service={dhcpv6-client,http,https} --permanent
```

## Reload firewall
```
firewall-cmd --reload
```
## Cek kembali 
```
firewall-cmd --list-all-zone
```

## Menghapus service pada zone Internal
```
firewall-cmd --zone=internal --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```

## Reload firewall
```
firewall-cmd --reload
```

## Cek kembali 
```
firewall-cmd --list-all-zone
```

## Menghapus service home zone
```
firewall-cmd --zone=home --remove-service={dhcpv6-client,mdns,samba-client,ssh} --permanent
```
## Reload firewall
```
firewall-cmd --reload
```

## Cek kembali 
```
firewall-cmd --list-all-zone
```

## Menghapus SSH dari zone External
```
firewall-cmd --zone=external --remove-service=ssh --permanent
```

## Menghapus SSH dari zone DMZ
```
firewall-cmd --zone=dmz --remove-service=ssh --permanent
```

## Reload firewall
```
firewall-cmd --reload
```
## Menghapus service pada NM-Shared
```
firewall-cmd --zone=nm-shared --remove-service={dhcp,dns,ssh} --permanent
```
## Reload firewall
```
firewall-cmd --reload
```

## Cek kembali apakah semua list yang tadi dihapus masih ada atau tidak
```
firewall-cmd --list-all-zone
```












