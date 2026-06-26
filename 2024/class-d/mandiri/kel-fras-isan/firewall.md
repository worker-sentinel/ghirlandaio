# 1. Buka port untuk Docker Swarm & Cockpit
```
firewall-cmd --zone=public --add-service=cockpit --permanent
```
```
firewall-cmd --zone=public --add-port=2377/tcp --permanent
```
```
firewall-cmd --zone=public --add-port=7946/tcp --permanent
```
```
firewall-cmd --zone=public --add-port=7946/udp --permanent
```
```
firewall-cmd --zone=public --add-port=4789/udp --permanent
```

# 2. Muat ulang firewall untuk menerapkan semua perubahan sekaligus
```
firewall-cmd --reload
```
