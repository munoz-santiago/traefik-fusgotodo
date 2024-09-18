
# This guide is used to set up and configure all the neccesary Traefik staff for my Digital Ocean droplet. 


### 1. Copy traefik configuration file inside droplet
```
scp -i ~/.ssh/digital_ocean traefik.yml root@24.199.99.83:/root/traefik.yml
```


### 2. Connecto to droplet
```
ssh -i ~/.ssh/digital_ocean root@24.199.99.83
```

### 3. Create the network for internal communication for all docker containers and Traefik
```
docker network create traefik_proxy
```

### 4. Start the traefik docker container
```
docker stop traefik-service || true
docker rm -f traefik-service || true
docker run -d \
  --name traefik-service \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /root/traefik.yml:/etc/traefik/traefik.yml \
  -v $(pwd)/letsencrypt:/letsencrypt \
  -p 80:80 -p 443:443 \
  --network traefik_proxy \
  traefik:v2.5


docker logs -f traefik-service
``````


## Para los ssl (https) usé certbot para firmar con let's encrypt
