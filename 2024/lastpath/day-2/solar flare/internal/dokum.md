# Dokumentasi Join K3s Agent — Kelompok 20 Solar Flare

*(Join node ke cluster K3s)*

---

## 1. Install & Join K3s Agent ke Server

```bash
curl -sfL https://get.k3s.io | K3S_URL="https://(Menyesuaikan IP Address):6443" K3S_TOKEN="(Menyesuaikan Token)" sh -s - agent
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

Perintah di atas digunakan untuk menginstal **K3s Agent** sekaligus menghubungkannya ke server K3s (control plane).

- **K3S_URL** merupakan alamat API Server Kubernetes yang digunakan sebagai tujuan koneksi agent. Alamat IP disesuaikan dengan server yang digunakan masing-masing kelompok.
- **6443** adalah port bawaan Kubernetes API Server.
- **K3S_TOKEN** merupakan token autentikasi yang dihasilkan oleh server K3s dan harus disesuaikan dengan token pada server masing-masing.
- Opsi `sh -s - agent` menjalankan proses instalasi dalam mode **agent (worker node)** sehingga node dapat bergabung ke dalam cluster.

---

## 2. Restart Service K3s Agent

```bash
systemctl restart k3s-agent.service
```

Kemudian cek status servicenya menggunakan:

```bash
systemctl status k3s-agent.service
```

### Output

```text
● k3s-agent.service - Lightweight Kubernetes
     Loaded: loaded (/etc/systemd/system/k3s-agent.service; enabled)
     Active: active (running)
     Docs: https://k3s.io
     Main PID: xxxx (k3s-agent)

...

time="YYYY-MM-DDTHH:MM:SSZ" level=info msg="k3s agent is up and running"
```

### Penjelasan

Perintah `systemctl restart k3s-agent.service` digunakan untuk menjalankan ulang service K3s Agent agar konfigurasi terbaru diterapkan.

Selanjutnya, `systemctl status k3s-agent.service` digunakan untuk memastikan service berjalan dengan normal. Apabila status menunjukkan **active (running)** dan muncul pesan **"k3s agent is up and running"**, berarti proses join ke cluster berhasil dan node telah terhubung ke server Kubernetes.

---

## 3. Exit

```bash
exit
```

### Penjelasan

Perintah `exit` digunakan untuk mengakhiri sesi terminal setelah proses instalasi dan verifikasi K3s Agent selesai dilakukan.
