# Dokumentasi Join K3s Agent — kubernet internal
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

## 2. Restart Service K3s Agent
```
systemctl restart k3s-agent.service
```
```
systemctl status k3s-agent.service
```
Output:
```
● kIni seluruh output dari command `systemctl status k3s-agent.service` sampai bawah:

``
● k3s-agent.service - Lightweight Kubernetes
     Loaded: loaded (/etc/systemd/system/k3s-agent.service; enabled; preset: disabled)
     Active: active (running) since Wed 2026-07-01 11:06:01 UTC; 11s ago
 Invocation: 6eb5ec64eeab4df29430c8758ebf8311
       Docs: https://k3s.io
    Process: 7813 ExecStartPre=/sbin/modprobe br_netfilter (code=exited, status=0/SUCCESS)
    Process: 7815 ExecStartPre=/sbin/modprobe overlay (code=exited, status=0/SUCCESS)
   Main PID: 7817 (k3s-agent)
      Tasks: 33
     Memory: 355M (peak: 372.5M)
        CPU: 2.570s
     CGroup: /system.slice/k3s-agent.service
             ├─7590 /var/lib/rancher/k3s/data/25bd5c205b3a990305155ad3b8e899582026d2d802b257105310b41a262941c1/bin/containerd-shim-runc-v2 -namespace k8s.io -id 300f4d31b85c4a26cbf6a3557174cd58f8bd3c90aa5f143e21efb41facac28c0 -address /run/k3s/containerd/containerd.sock
             ├─7817 "/usr/local/bin/k3s agent"
             └─7833 "containerd "

Jul 01 11:06:00 internal k3s[7817]: I0701 11:06:00.200264    7817 vxlan_network.go:265] Received Subnet Event with VxLan: BackendType: vxlan, PublicIP: 192.168.100.63, PublicIPv6: (nil), BackendData: {"VNI":1,"VtepMAC":"62:0e:dd:14:92:c2"}, BackendV6Data: (nil)
Jul 01 11:06:00 internal k3s[7817]: I0701 11:06:00.233634    7817 iptables.go:358] bootstrap done
Jul 01 11:06:00 internal k3s[7817]: I0701 11:06:00.252586    7817 iptables.go:358] bootstrap done
Jul 01 11:06:00 internal k3s[7817]: I0701 11:06:00.636864    7817 apiserver.go:51] "Watching apiserver"
Jul 01 11:06:01 internal k3s[7817]: I0701 11:06:01.059750    7817 desired_state_of_world_populator.go:154] "Finished populating initial desired state of world"
Jul 01 11:06:01 internal k3s[7817]: time="2026-07-01T11:06:01Z" level=info msg="Starting network policy controller version v2.6.3-k3s1, built on 2026-06-24T22:52:31Z, go1.26.4"
Jul 01 11:06:01 internal k3s[7817]: time="2026-07-01T11:06:01Z" level=info msg="k3s agent is up and running"
Jul 01 11:06:01 internal k3s[7817]: I0701 11:06:01.227051    7817 network_policy_controller.go:164] Starting network policy controller
Jul 01 11:06:01 internal systemd[1]: Started Lightweight Kubernetes.
Jul 01 11:06:01 internal k3s[7817]: I0701 11:06:01.301012    7817 network_policy_controller.go:179] Starting network policy controller full sync goroutine
```


Penjelasan:
Setelah restart, service `k3s-agent` berhasil aktif (`active (running)`) dan log mengonfirmasi `k3s agent is up and running` — node `internal` resmi bergabung sebagai agent ke cluster.

---

### 3. Exit
```
exit
```
