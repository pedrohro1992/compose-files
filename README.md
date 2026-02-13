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
## zot (Registry)
### Create the storage
````bash
mkdir -p ~/docker-data/zot/storage

sudo chown -R 999:999 ~/docker-data/zot/storage

docker volume create --driver local \
  --opt type=none \
  --opt device=$HOME/docker-data/zot/storage \
  --opt o=bind \
  zot-storage-volume
````
### Create TLS Certificate
The Zot Registry and Crossplane needs to talt to eath other over https,
````bash 
openssl req -x509 -newkey rsa:4096 -sha256 -days 365 -nodes \
  -keyout domain.key -out domain.crt \
  -subj "/CN=zot-registry.cacetinho.internal.infra" \
  -addext "subjectAltName = DNS:zot-registry.cacetinho.internal.infra"
````



