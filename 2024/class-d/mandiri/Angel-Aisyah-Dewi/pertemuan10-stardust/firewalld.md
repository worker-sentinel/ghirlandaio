## Instalasi Firewalld

```bash
pacman -S firewalld
```

## Manajemen Service Firewalld

```bash
systemctl enable firewalld
```

```bash
systemctl start firewalld
```

## Memeriksa Status Service

```bash
systemctl status firewalld
```

## Melihat Semua Zona Firewall

```bash
firewall-cmd --list-all-zones
```
