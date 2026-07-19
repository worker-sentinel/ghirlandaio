# Dokumentasi Join K3s Agent — kubernet public

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
[INFO]  systemd: Starting k3s-agent

```
Penjelasan:
- curl -sfL https://get.k3s.io | sh -s - agent — download dan jalankan install script resmi K3s, dengan argumen agent supaya node ini di-install sebagai *worker node*, bukan server/control-plane.
- K3S_URL="https://192.168.100.63:6443" — alamat API server dari K3s server yang mau di-join, port 6443 adalah default port API server Kubernetes.
- K3S_TOKEN="..." — token cluster yang diambil dari server (biasanya di /var/lib/rancher/k3s/server/node-token), dipakai agent untuk autentikasi join ke cluster.
- Script otomatis membuat binary k3s, symlink kubectl & crictl ke binary tersebut, serta service systemd k3s-agent.service yang langsung di-enable dan dijalankan.

---

---

## 2. Restart Service K3s Agent
```
sudo systemctl restart k3s-agent.service
```
Penjelasan:
- sudo memberi hak akses root sehingga systemd mengizinkan restart service k3s-agent.

### 3. Cek Status Service
```
sudo systemctl status k3s-agent.service
```
Output:
```
● k3s-agent.service - Lightweight Kubernetes
     Loaded: loaded (/etc/systemd/system/k3s-agent.service; enabled; preset: disabled)
     Active: active (running) since Thu 2026-07-02 02:11:50 WIB; 40s ago
   Main PID: 3734 (k3s-agent)
      Tasks: 33
     Memory: 375.7M (peak: 389M)
        CPU: 3.188s



Jul 02 02:11:50 amelia k3s[3734]: time="2026-07-02T02:11:50+07:00" level=info msg="k3s agent is up and running"
Jul 02 02:11:50 amelia systemd[1]: Started Lightweight Kubernetes.
```
Penjelasan:
- Active: active (running) — konfirmasi bahwa service k3s-agent sedang berjalan normal.
- Main PID: 3734 — process ID utama dari k3s agent.
- Log terakhir "k3s agent is up and running" dan network policy controller yang start menandakan agent sudah berhasil connect ke server dan siap menerima workload (pod CIDR 10.42.3.0/24 sudah di-assign ke node ini).
 
 ## Exit
```
exit
```
  
