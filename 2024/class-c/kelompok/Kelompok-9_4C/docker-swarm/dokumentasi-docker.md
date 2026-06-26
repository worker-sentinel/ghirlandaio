# docker swarm
#### di sisi manager
```
docker swarm init --advertise-addr <ip_anda>
```
> nanti akan muncul token, copy token anda
lalu
#### di sisi worker
```
docker swarm --join token124546567687889867565456763 <ip_manager>:2377
```

## deploy

#### di sisi manager

```
mkdir project-docker.yml
```
```
cd project-docker.yml
```
```
nvim docker-compose.yml
```
> isi dengan
```
version: "3.9"

services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: mayan
      POSTGRES_USER: mayan
      POSTGRES_PASSWORD: strongpassword
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - mayan

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    networks:
      - mayan
    command:
      - redis-server
      - --maxmemory
      - "100mb"
  mayan:
    image: mayanedms/mayanedms:latest
    depends_on:
      - postgres
      - redis
    ports:
      - "8000:8000"
    deploy:
      replicas: 2
    environment:
      MAYAN_DATABASE_ENGINE: django.db.backends.postgresql
      MAYAN_DATABASE_NAME: mayan
      MAYAN_DATABASE_USER: mayan
      MAYAN_DATABASE_PASSWORD: strongpassword
      MAYAN_DATABASE_HOST: postgres
      MAYAN_DATABASE_PORT: 5432

      MAYAN_CELERY_BROKER_URL: redis://redis:6379/0
      MAYAN_CELERY_RESULT_BACKEND: redis://redis:6379/0

      MAYAN_ALLOWED_HOSTS: "*"
    volumes:
      - mayan_settings:/var/lib/mayan
      - mayan_media:/var/lib/mayan/media
    networks:
      - mayan
  
volumes:
  postgres_data:
  redis_data:
  mayan_settings:
  mayan_media:

networks:
  mayan:
    driver: overlay
    attachable: true

```
```
docker stack deploy -c docker-compose.yml mayan
```
```
docker node ls
```
> untuk melihat node

```
docker ps -a
```
> untuk melihat kontainer

```
docker service ls
```
> untuk melihat service yaang sedang running

