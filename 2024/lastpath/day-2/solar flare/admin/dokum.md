# Dokumentasi Instalasi K3s Server — Kelompok 20 Solar Flare

## 1. Install K3s Server

```bash
curl -sfL https://get.k3s.io | sh -
```

### Output

```text
[INFO]  Finding release for channel stable
[INFO]  Using v1.36.2+k3s1 as release
[INFO]  Downloading hash ...
[INFO]  Downloading binary ...
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Creating /usr/local/bin/ctr symlink to k3s
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink '/etc/systemd/system/multi-user.target.wants/k3s.service' → '/etc/systemd/system/k3s.service'.
[INFO]  systemd: Starting k3s
```

### Penjelasan

Perintah tersebut digunakan untuk mengunduh sekaligus memasang **K3s Server** sebagai **control plane** Kubernetes. Setelah instalasi selesai, sistem akan membuat service `k3s`, symlink untuk `kubectl`, `crictl`, dan `ctr`, serta menjalankan service secara otomatis sehingga server siap mengelola cluster.

---

## 2. Melihat Alamat IP Server

```bash
ip a
```

### Output

```text
(menyesuaikan ip address)
```

### Penjelasan

Perintah `ip a` digunakan untuk menampilkan seluruh informasi antarmuka jaringan pada server. Dari hasil tersebut diketahui bahwa alamat IP server adalah **10.186.198.212**, yang nantinya digunakan oleh node agent untuk terhubung ke control plane.

---

## 3. Memastikan Instalasi K3s

```bash
curl -sfL https://get.k3s.io | sh -
```

### Output

```text
[INFO]  Skipping binary downloaded, installed k3s matches hash
[INFO]  Skipping /usr/local/bin/kubectl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/crictl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, already exists
[INFO]  No change detected so skipping service start
```

### Penjelasan

Perintah dijalankan kembali untuk memastikan K3s telah terpasang dengan benar. Karena instalasi sudah ada, sistem tidak mengunduh ulang file maupun memulai ulang service. Hal ini menunjukkan bahwa proses instalasi sebelumnya telah berhasil.

---

## 4. Melihat Node Token

```bash
(menyesuaikan token)
```

### Output

```text
K10c9477ff966057063401d22029a1dfd36cfe730aed46ffcc5d913c14d29f723ad::server:177a41888fbf4ec30fdaa028be4e03af
```

### Penjelasan

Perintah ini digunakan untuk menampilkan **node token** yang berada pada server K3s. Token tersebut diperlukan ketika node worker (agent) ingin bergabung ke dalam cluster sebagai proses autentikasi agar koneksi ke control plane dapat dilakukan dengan aman.

---

## 5. Exit

```bash
exit
```

### Penjelasan

Perintah `exit` digunakan untuk mengakhiri sesi terminal setelah seluruh proses konfigurasi dan pengecekan selesai dilakukan.

