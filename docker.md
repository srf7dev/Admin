#### Network
>docker network create --subnet=192.168.1.0/24 srf_net

#### GitHub ключ для заливки
    
#### GitHub ключ для чтения
   

#### Postgress
    docker run --name srf_db \
      --restart always \
      --network srf_net \
      --ip 192.168.1.110 \
      -p "0.0.0.0:36000:5432" \
      -e POSTGRES_PASSWORD=ZsWrywPahRpk \
      -e PGDATA=/var/lib/postgresql/data/pgdata \
      -v /home/volumes/pg_data:/var/lib/postgresql/data \
      -d postgres

#### NATS
    docker run --name srf_nats \
      --restart always \
      --network srf_net \
      --ip 192.168.1.100 \
      -p "0.0.0.0:4222:4222" \
      -p "0.0.0.0:8222:8222" \
      -v /home/volumes/nats:/natsdata \
      -d nats \
      -c /natsdata/natsconfig.cfg

 #### apiGateway
    docker run --name srf_apiGateway \
      --restart always \
      --network srf_net \
      --ip 192.168.1.10 \
      -p "0.0.0.0:5100:8080" \
      -v /home/volumes/logs:/logs \
      -v /home/volumes/wwwroot/images:/wwwroot/images \
      -v /home/volumes/wwwroot/img:/wwwroot/img \
      -v /home/volumes/wwwroot/bot:/wwwroot/bot \
      -v /home/volumes/wwwroot/auth:/wwwroot/auth \
      -v /home/volumes/wwwroot/uploads:/wwwroot/uploads \
      -d ghcr.io/srf7dev/apigateway:latest

  #### user
    docker run --name srf_user \
      --restart always \
      --network srf_net \
      --ip 192.168.1.11 \
      -v /home/volumes/logs:/logs \
      -d ghcr.io/srf7dev/user:latest