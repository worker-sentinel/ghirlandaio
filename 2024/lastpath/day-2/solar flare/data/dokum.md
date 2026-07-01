## 1. Install & Join K3s Agent ke Server

```bash
curl -sfL (menyesuaikan ip address) | K3S_URL="https://(menyesuaikan token):6443" K3S_TOKEN="K10ed98ed6904da492906e905df5164e43be9fc7e84761ab95a60a0f84624ffdc40::server:4a0b522ab175331bc1f29f9fdda012a7" sh -s - agent
```

### Output

```text
[INFO]  Using v1.36.2+k3s1 as release
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  systemd: Creating service file /etc/systemd/system/k3s-agent.service
[INFO]  systemd: Enabling k3s-agent unit
Created symlink '/etc/systemd/system/multi-user.target.wants/k3s-agent.service' → '/etc/systemd/system/k3s-agent.service'.
```

### Penjelasan

Perintah di atas digunakan untuk menginstal K3s dalam mode **agent (worker)** sekaligus menghubungkannya ke server Kubernetes.

- `K3S_URL` berisi alamat API Server Kubernetes.
- `K3S_TOKEN` merupakan token autentikasi agar node dapat bergabung ke cluster.
- `sh -s - agent` menjalankan proses instalasi sebagai node worker.

Setelah proses selesai, sistem akan:
- Menginstal binary `k3s`.
- Membuat symlink `kubectl` dan `crictl`.
- Membuat service `k3s-agent.service`.
- Mengaktifkan service agar otomatis berjalan saat sistem dinyalakan.

Keterangan:
- **192.168.100.63** = IP Address Control Plane.
- **6443** = Port API Server Kubernetes.

---

## 2. Menjalankan Firewalld

```bash
systemctl start firewalld
```

```bash
systemctl status firewalld
```

### Output

```text
● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: disabled)
     Active: active (running) since Thu 2026-07-02 00:21:25 WIB
```

### Penjelasan

Firewalld dijalankan untuk mengelola aturan firewall sehingga komunikasi antar node Kubernetes tetap aman.

Status **active (running)** menunjukkan bahwa firewall telah aktif dan siap mengatur akses terhadap port yang diperlukan, seperti:
- 6443/TCP (Kubernetes API Server)
- 8472/UDP (Flannel VXLAN)
- Port lain yang digunakan oleh K3s.

---

## 3. Restart Service K3s Agent

```bash
systemctl restart k3s-agent.service
```

```bash
systemctl status k3s-agent.service
```

### Output

```text
● k3s-agent.service - Lightweight Kubernetes
     Active: active (running) since Thu 2026-07-02 00:38:54 WIB
...
time="2026-07-02T00:38:54+07:00" level=info msg="k3s agent is up and running"
Started Lightweight Kubernetes.
```

### Penjelasan

Service `k3s-agent` direstart agar seluruh konfigurasi terbaru diterapkan.

Status:

- `active (running)` menunjukkan service berhasil berjalan.
- Log `k3s agent is up and running` menandakan node worker telah berhasil terhubung ke cluster Kubernetes dan siap menerima workload dari control plane.

---

## 4. Keluar dari Session

```bash
exit
```

### Penjelasan

Perintah `exit` digunakan untuk mengakhiri sesi terminal atau keluar dari shell yang sedang digunakan.
