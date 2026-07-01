# Dokumentasi Join K3s Agent — kubernetes data
(Join node ke cluster k3s)

## 1. Install & Join K3s Agent ke Server
```
curl -sfL https://get.k3s.io | K3S_URL="https://192.168.100.63:6443" K3S_TOKEN="K10ed98ed6904da492906e905df5164e43be9fc7e84761ab95a60a0f84624ffdc40::server:4a0b522ab175331bc1f29f9fdda012a7" sh -s - agent
```

Output:
```
[INFO]  Using v1.36.2+k3s1 as release
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  systemd: Creating service file /etc/systemd/system/k3s-agent.service
[INFO]  systemd: Enabling k3s-agent unit
Created symlink '/etc/systemd/system/multi-user.target.wants/k3s-agent.service' → '/etc/systemd/system/k3s-agent.service'.
```
Penjelasan:
K3S_URL mengarah ke API server cluster, K3S_TOKEN adalah token join. sh -s - agent menjalankan install script dalam mode agent (worker), sehingga binary k3s, symlink kubectl/crictl, dan service k3s-agent.service terbentuk.

192.168.100.63=IP adress dari controlnya,
6443= port yang dipakai kubernetes

---

## 2. Nyalakan Firewalld
```
systemctl start firewalld
```
```
systemctl status firewalld
```

Output:
```
● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: disabled)
     Active: active (running) since Thu 2026-07-02 00:21:25 WIB
```
Penjelasan:
Firewalld dipastikan aktif agar port komunikasi antar node K3s (6443, 8472/UDP, dsb) tetap terkontrol tapi service tetap bisa jalan normal.

---

## 3. Restart Service K3s Agent
```
systemctl restart k3s-agent.service
```
```
systemctl status k3s-agent.service
```
Output:
```
● k3s-agent.service - Lightweight Kubernetes
     Active: active (running) since Thu 2026-07-02 00:38:54 WIB
...
time="2026-07-02T00:38:54+07:00" level=info msg="k3s agent is up and running"
Started Lightweight Kubernetes.

```
Penjelasan:
Setelah restart, service `k3s-agent` berhasil aktif (`active (running)`) dan log mengonfirmasi `k3s agent is up and running` — node `assyifa` resmi bergabung sebagai agent ke cluster.

---

### 4. Exit
```
exit
```
