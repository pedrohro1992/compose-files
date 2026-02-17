# compose-files
Docker Compose code to config homelab infra

# How to use:
## Create docker network
````bash 
docker network create --subnet=172.19.0.0/16 infra
`````
## PowerDNS
### Create the storage
````bash
mkdir -p ~/docker-data/powerdns/data/mysql

docker volume create --driver local \
  --opt type=none \
  --opt device=$HOME/docker-data/powerdns/data/mysql \
  --opt o=bind \
  pdns-mysql-volume

sudo chown -R 999:999 ~/docker-data/powerdns/data/mysql
````
### Run the stack
````bash
docker compose -f ./power-dns-compose/docker-compose.yaml up -d
````
