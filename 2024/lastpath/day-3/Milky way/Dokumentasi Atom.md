# Dokumentasi Docker Compose Atom

## Cek Docker
```
docker ps
```
Penjelasan:
Mastiin Docker udah aktif dan bisa diakses dari terminal

—

## Clone Source Code
```
cd atom
```
Penjelasan: Masuk ke folder atom hasil clone

—

## Set Compose File (bash)
```
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml"
```
Penjelasan: Sama kaya di atas, cuma beda sintaks karena pakai shell fish

—

## Jalankan Semua Container
```
docker compose up -d
```
Penjelasan: Build dan jalanin semua container sekaligus di background (-d = detached)

—

## Init Database
```
docker compose exec atom php -d memory_limit=-1 symfony tools:purge --demo
```
Penjelasan: Jalanin perintah di dalam container atom buat bersihin lalu isi ulang database pakai data demo -d

—

## Build Theme Bootstrap 5
```
docker compose exec atom npm install
```
Penjelasan: Install semua dependencies Node.js di dalam container atom

```
docker compose exec atom npm run build
```
Penjelasan: Compile CSS/JS tema Bootstrap 5 biar tampilan situs jadi final

—

## Build Theme Bootstrap 2 (versi lama)
```
docker compose exec atom make -C plugins/arDominionPlugin
```
Penjelasan: Build stylesheet buat plugin tema lama yang pakai Bootstrap 2

—

## Restart Worker
```
docker compose restart atom_worker
```
Penjelasan: Stop lalu jalanin ulang container atom_worker aja, tanpa ganggu container lain

—

## Lihat Log
```
docker compose logs -f atom atom_worker nginx
```
Penjelasan: Nampilin log dari beberapa container sekaligus. -f = follow, log update terus real-time kaya tail -f

—

## Scaling Worker
```
docker compose up -d --scale atom_worker=2
```
Penjelasan: Jalanin 2 instance atom_worker sekaligus, buat percepat proses antrian/job besar (misal import data banyak)

—

## Cek Status Worker Gearman
```
docker compose exec atom bash -c "nc gearmand 4730"
```
Penjelasan: Connect manual ke Gearman (job queue server) di port 4730 dari dalam container atom

—

## Stop Semua Container
```
docker compose stop
```
Penjelasan: Stop semua container tanpa hapus data. Jalanin lagi pakai docker compose up -d

—

## Cek Status & Port
```
docker compose ps
```
Penjelasan: Nampilin tabel nama container, status, dan port yang dipetakan ke komputer host

—

## Monitoring PMM
```
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml;$PWD/docker/docker-compose.pmm.yml"
```
Penjelasan: Nambahin config PMM (Percona Monitoring and Management) biar service monitoring MySQL ikut aktif
Kalau pmm_client error "app already is running";

```
docker compose rm pmm_client
```
Penjelasan: Hapus container pmm_client yang bermasalah

```
docker compose up -d
```
Penjelasan: Bikin ulang container dari awal, nyelesain error tadi

—

## Varnish Cache (simulasi mode read-only)
```
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml:$PWD/docker/docker-compose.varnish.yml"
```
Penjelasan: Nambahin config Varnish (reverse-proxy cache) ke daftar compose file

```
docker compose exec varnish varnishlog
```
Penjelasan: Lihat log real-time dari Varnish, buat cek request mana yang di-cache

—

## Debug PHP dengan Xdebug (VS Code)
```
Install extension PHP Debug di VS Code
Buat file .vscode/launch.json
```
```
{
   "version": "0.2.0",
   "configurations": [
      {
         "name": "Listen for Xdebug",
         "type": "php",
         "request": "launch",
         "port": 9003,
         "pathMappings": {
            "/atom/src": "${workspaceFolder}"
         }
      }
   ]
}
```
Penjelasan: Config ini biar VS Code "dengerin" koneksi debug Xdebug di port 9003, dan mapping folder di container ke folder project lokal

—

## Exit
```
exit
```

