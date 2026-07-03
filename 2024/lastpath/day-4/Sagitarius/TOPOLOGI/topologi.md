**Kelompok Sagitarius**
1. Maryam Nurul Jaatsiyah
2. Hana Maulida
   
<img width="1169" height="827" alt="TOPOLOGI drawio" src="https://github.com/user-attachments/assets/2b00d2a0-6f64-46e3-9637-d6c0c99e3bdf" />

### Cluster

| Device | Role | Fungsi |
|---------|------|--------|
| Control | K3s Server / Master | Mengelola seluruh cluster Kubernetes |
| Data | Worker Node | Menjalankan MariaDB sebagai Database |
| Internal | Worker Node | Menjalankan Redis/Valkey sebagai Cache & Session |
| Public | Worker Node | Menjalankan aplikasi SLiMS dan Web Server |
| Client | User | Mengakses aplikasi melalui browser |

### Alur Komunikasi
```
Client
   │
HTTP/HTTPS
   │
   ▼
Public (SLiMS)
   ├────────► MariaDB
   └────────► Redis / Valkey

Control
   ├────────► Data
   ├────────► Internal
   └────────► Public
```
 ### Network

| Device | IP Address | Fungsi |
|---------|------------|--------|
| Control | 10.171.181.168 | Cluster Management |
| Data | 10.171.181.53 | Database |
| Internal | 10.171.181.22 | Cache |
| Public | 10.171.181.224 | Web Server |
| Client | | Browser |
```
   Control (K3s)
    ├────────► Data (MariaDB)
    ├────────► Internal (Redis)
    └────────► Public (SLiMS)
                        ▲
                        │
                 HTTP / HTTPS
                        │
                     Client
```
