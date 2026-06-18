```
sudo docker pull openkm/openkm-ce:latest
```
```
sudo nvim docker-compose.yml
```
isi
```
services:
  openkm-db:
    image: mariadb:10.11
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: openkm
      MYSQL_USER: openkm
      MYSQL_PASSWORD: openkmpassword
    volumes:
      - openkm_db_data:/var/lib/mysql
    networks:
      - openkm_net
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.role == manager

  openkm:
    image: openkm/openkm-ce:latest
    environment:
      DB_DRIVER: com.mysql.cj.jdbc.Driver
      DB_URL: jdbc:mysql://openkm-db:3306/openkm?autoReconnect=true&useUnicode=true&characterEncoding=UTF-8
      DB_USER: openkm
      DB_PASSWORD: openkmpassword

      TZ: Asia/Jakarta

    ports:
      - target: 8080
        published: 8080
        protocol: tcp
        mode: ingress

    volumes:
      - openkm_data:/opt/tomcat/repository
      - openkm_logs:/opt/tomcat/logs

    networks:
      - openkm_net

    depends_on:
      - openkm-db

    deploy:
      replicas: 2
      restart_policy:
        condition: on-failure

networks:
  openkm_net:
    driver: overlay

volumes:
  openkm_db_data:
  openkm_data:
  openkm_logs:
```
```
sudo docker stack deploy -c docker-compose.yml [nama_container]
```
