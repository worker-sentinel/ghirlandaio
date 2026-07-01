# Dokumentasi kubernet data

## Install & sambung ke Server
```
curl -sfL https://get.k3s.io | K3S_URL="https://192.168.000.00:6443" K3S_TOKEN="K10ed98ed7890da492906e905df5164e43be9fc7e84761ab95a60a0f84624ffdc40::server:4a0b533ab175331bc1f29f9fdda011a7" sh -s - agent
```
Penjelasan:
192.168.000.00=IP a dari control,
6443= port yang dipakai kubernetes

---

## Menyalakan Firewalld
```
systemctl start firewalld
```
```
systemctl status firewalld
```

Penjelasan:
Firewalld dipastikan aktif agar port komunikasi  tetap terkontrol tapi service tetap bisa jalan normal.

---

## Restart Service
```
systemctl restart k3s-agent.service
```
```
systemctl status k3s-agent.service
```
## Exit
```
exit
```
