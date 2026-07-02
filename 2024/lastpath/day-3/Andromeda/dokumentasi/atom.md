# instal docker

docker ps


git clone -b qa/2.x https://github.com/artefactual/atom.git atom
cd atom

# For bash users (most of you)
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml"

# For fish users
set -lx COMPOSE_FILE (pwd)/docker/docker-compose.dev.ym

# For bash users (most of you)
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml:$PWD/docker/docker-compose.override.arm.yml"

# For fish users
set -lx COMPOSE_FILE (pwd)/docker/docker-compose.dev.yml:(pwd)/docker/docker-compose.override.arm.yml


# Create and start containers. This may take a while the first time you run
# it because all the images have to be downloaded (e.g. percona, memcached)
# and the AtoM image has to be built.
docker compose up -d

## Inisialisasi basis data: 

# Execute the purge command in the running container
docker compose exec atom php -d memory_limit=-1 symfony tools:purge --demo

## Mengkompilasi File Tema Bootstrap 5: 

docker compose exec atom npm install


docker compose exec atom npm run build

## Mengkompilasi File Tema Bootstrap 2: 

# Execute another command: build stylesheets
docker compose exec atom make -C plugins/arDominionPlugin


docker compose restart atom_worker


docker compose logs -f atom atom_worker nginx


docker compose up -d --scale atom_worker=2

Mari kita verifikasi bahwa dua pekerja telah berlangganan Gearman:
# Establish a TCP connection to gearmand, port 4730	

docker compose exec atom bash -c "nc gearmand 4730"

# Send STATUS command
STATUS

0a2a58137e05032d1140fdbd0d6dccbb-arInheritRightsJob                0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arFileImportJob                   0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arInformationObjectXmlExportJob   0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arActorXmlExportJob               0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arCalculateDescendantDatesJob     0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arXmlExportSingleFileJob          0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arUpdatePublicationStatusJob      0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arObjectMoveJob                   0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arInformationObjectCsvExportJob   0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arUpdateEsIoDocumentsJob          0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arActorCsvExportJob               0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arRepositoryCsvExportJob          0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arFindingAidJob                   0   0   2
0a2a58137e05032d1140fdbd0d6dccbb-arGenerateReportJob               0   0   2


docker compose down --volumes

## Terhubung ke AtoM 
Anda dapat menjalankan perintah berikut untuk memeriksa status dan informasi lain tentang kontainer:
$ docker compose ps


         Name                       Command               State                  Ports
-----------------------------------------------------------------------------------------------------
docker_atom_1            /atom/src/docker/entrypoin ...   Up      9000/tcp
docker_atom_worker_1     /atom/src/docker/entrypoin ...   Up      9000/tcp
docker_nginx_1           nginx -g daemon off;             Up      0.0.0.0:63001->80/tcp
docker_elasticsearch_1   /bin/bash bin/es-docker          Up      127.0.0.1:63002->9200/tcp, 9300/tcp
docker_percona_1         /docker-entrypoint.sh mysqld     Up      127.0.0.1:63003->3306/tcp
docker_memcached_1       docker-entrypoint.sh -p 11 ...   Up      127.0.0.1:63004->11211/tcp
docker_gearmand_1        docker-entrypoint.sh gearmand    Up      127.0.0.1:63005->4730/tcp


```
tidak lanjut karena waktu sudah tidak memungkinkan :)
