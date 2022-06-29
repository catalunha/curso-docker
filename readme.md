# repo
https://github.com/cod3rcursos/curso-docker

# docker docs
https://docs.docker.com/
hub.docker.com




# install 
https://docs.docker.com/engine/install/ubuntu/
```
sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo usermod -a -G docker $USER

```

# tutoriais
https://www.youtube.com/watch?v=AVNADGzXrrQ

# teste

sudo usermod -a -G docker $USER

docker container run hello-world

# criacao de containers
* ~$ docker container run debian bash --version
  * pull na imagem se ela nao existe localmente
  * create o container sempre
  * start o container
  * exec em modo iterativo
* ~$ docker container ls
  * list todos os container ativos
* ~$ docker container ls -a
  * list todos os container criados
* ~$ docker container run --rm debian bash --version
  * executa e depois remove o container
* ~$ docker container run -it  debian bash
  * -i inicia o container no modo iterativo
  * -t conecta terminal de entrada como no host
* ~$ docker container run --name mydebian -it debian bash
  * --name cria um container com o nome mydebian
  * -i Keep STDIN open even if not attached
  * -t Allocate a pseudo-TTY
* ~$ docker container start -ai mydebian
  * start inicia um container com certo nome
  * -a Attach to STDIN, STDOUT or STDERR
  * -i Attach container's STDIN
* ~$ docker container rm aa bb
  * rm remove container com o nome  aa e bb

# compartilhamento
* ~$ docker container run -p 8080:80 nginx
  * faz um run
  * -p Publish a container's port(s) to the host
  * -p PortaHost:PortaContainer
* ~$ docker container run -p 8080:80 -v $(pwd)/not-found:/usr/share/nginx/html nginx
  * run faz um run
  * -p mapeia a porta
  * -v mapeia o path no host e o path no ctnr
* catalunha@pop-os:~/dockers/ex-volume$ docker container run -d --name ex-daemon-basic -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html nginx
  * -d Run container in background and print container ID
* catalunha@pop-os:~/dockers/ex-volume$ docker container start ex-daemon-basic
* catalunha@pop-os:~/dockers/ex-volume$ docker container restart ex-daemon-basic
* catalunha@pop-os:~/dockers/ex-volume$ docker container stop ex-daemon-basic
* catalunha@pop-os:~/dockers/ex-volume$ docker container  logs ex-daemon-basic
  * acompanhar os logs do container
* catalunha@pop-os:~/dockers/ex-volume$ docker container  inspect ex-daemon-basic
  * mostra todas as configs de um container
* catalunha@pop-os:~/dockers/ex-volume$ docker container  exec ex-daemon-basic uname -or
  * envia um comando para o cntnr
* catalunha@pop-os:~/dockers/primeiro-build$ docker container run -p 80:80 ex-simple-build
* catalunha@pop-os:~/dockers/build-with-arg$ docker container run ex-build-arg bash -c 'echo $S3_BUCKET'
  * run container com arg

# images
* catalunha@pop-os:~/dockers/ex-volume$ docker image ls
  * listar as imagens
* catalunha@pop-os:~/dockers$ docker image pull redis:latest
  * apenas baixa a imagens para depois gerar os containers
* catalunha@pop-os:~/dockers$ docker image tag redis:latest myredis
  * criar uma nova tag com novo nome e tag
* catalunha@pop-os:~/dockers$ docker image rm myredis
  * remover uma image
* catalunha@pop-os:~/dockers/primeiro-build$ docker image build -t ex-simple-build .
  * build para construir uma image
  * -t com o seguinte tag
  * . do dockerfile local
* catalunha@pop-os:~/dockers/build-with-arg$ docker image build --build-arg S3_BUCKET=myapp -t ex-build-arg .
  * build com argumentos

# volumes
* catalunha@pop-os:~/dockers/ex-volume$ docker volume ls
  * listas os volumes


# build com args
* catalunha@pop-os:~/dockers/build-with-arg$ docker image build -t ex-build-arg .
* catalunha@pop-os:~/dockers/build-with-arg$ docker container run ex-build-arg bash -c 'echo $S3_BUCKET'
  * run container com arg
* catalunha@pop-os:~/dockers/build-with-arg$ docker image build --build-arg S3_BUCKET=myapp -t ex-build-arg .
  * build com argumentos
* catalunha@pop-os:~/dockers/build-with-arg$ docker container run ex-build-arg bash -c 'echo $S3_BUCKET'
  * run container com arg


# build com copy
* catalunha@pop-os:~/dockers/build-with-copy$ docker image build -t ex-build-copy .
* catalunha@pop-os:~/dockers/build-with-copy$ docker container run -p 80:80 ex-build-copy


# build dev
* catalunha@pop-os:~/dockers/build-dev$ docker image build -t ex-build-dev . 
  * criando image
* catalunha@pop-os:~/dockers/build-dev$ docker container run -it -v $(pwd):/app -p 80:8000 --name python-server ex-build-dev
  * executando image para criar container
* catalunha@pop-os:~/dockers$ docker container run -it --volumes-from=python-server debian cat /log/http-server.log
  * subindo outro container para escular os logs do build.dev


# push image docker hub
* catalunha@pop-os:~/dockers/build-dev$ docker image tag ex-simple-build catalunha/simple-build:1.0
  * renomear a tag
* catalunha@pop-os:~/dockers/build-dev$ docker login --username=catalunha
Password: 
* catalunha@pop-os:~/dockers/build-dev$ docker image push catalunha/simple-build:1.0


# network
* catalunha@pop-os:~/dockers/build-dev$ docker container run --rm alpine ash -c 'ifconfig'
  * container com acesso normal liberado para a internnet
* catalunha@pop-os:~/dockers$ docker container run --rm --net none alpine ash -c 'ifconfig'
  * container com acesso bloqueado a internet
* catalunha@pop-os:~/dockers$ docker container run -d --name container1 alpine sleep 1000
  * ativando container1 em -d ,daemon, com --name , container1 da image alpine dormindo por 1000 segs
catalunha@pop-os:~/dockers$ docker container exec -it container1 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02  
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0

* catalunha@pop-os:~/dockers$ docker container run -d --name container2 alpine sleep 1000
catalunha@pop-os:~/dockers$ docker container exec -it container2 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:03  
          inet addr:172.17.0.3  Bcast:172.17.255.255  Mask:255.255.0.0

* catalunha@pop-os:~/dockers$ docker container exec -it container1 ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.097 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.380 ms
64 bytes from 172.17.0.3: seq=2 ttl=64 time=0.339 ms

A partir do container1 consigo pingar no container2 pela bridge de rede.

* catalunha@pop-os:~/dockers$ docker network create --driver bridge rede-nova
  * criando nova rede

catalunha@pop-os:~/dockers$ docker network ls
NETWORK ID     NAME        DRIVER    SCOPE
866955a1b826   bridge      bridge    local
72c09f9f5ac4   host        host      local
d07259e2fa03   none        null      local
fd1352938bc1   rede-nova   bridge    local

* catalunha@pop-os:~/dockers$ docker container run -d --name container3 --net rede-nova alpine sleep 1000
criando container com a nova rede, se nao for especificado vai de padrao bridge. sleep mata o container em 1000 segs

catalunha@pop-os:~/dockers$ docker container exec -it container3 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:12:00:02  
          inet addr:172.18.0.2  Bcast:172.18.255.255  Mask:255.255.0.0

catalunha@pop-os:~/dockers$ docker container exec -it container3 ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes

nao consegue pinkar pois estao em reder diferentes e nao tem conexao entre elas

* catalunha@pop-os:~/dockers$ docker network connect bridge container3
faz com que o container3 tenha acesso a rede bridge

* catalunha@pop-os:~/dockers$ docker container exec -it container3 ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:12:00:02  
          inet addr:172.18.0.2  Bcast:172.18.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:90 errors:0 dropped:0 overruns:0 frame:0
          TX packets:37 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:14098 (13.7 KiB)  TX bytes:3514 (3.4 KiB)

eth1      Link encap:Ethernet  HWaddr 02:42:AC:11:00:04  
          inet addr:172.17.0.4  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:30 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:4268 (4.1 KiB)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

agora com a nova config o container3 de uma nova rede pode acessar a rede do container1  ou container2 

* catalunha@pop-os:~/dockers$ docker container exec -it container3 ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.112 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.346 ms

* catalunha@pop-os:~/dockers$ docker network disconnect bridge container3

modo de rede host
* catalunha@pop-os:~/dockers$ docker container run -d --name container4 --net host alpine sleep 1000


# aula sobre node-mongo-nginx
após criar as aplicações e o docker compose
* catalunha@pop-os:~/dockers/node-mongo-nginx$ docker-compose up

# Seção 9 - Email com workers
## 1
Após criar o arquivo docker-compose.yml
executar com
catalunha@pop-os:~/dockers/email-worker$ docker-compose up -d
o -d é modo daemon
listando processos no compose
catalunha@pop-os:~/dockers/email-worker$ docker-compose ps
      Name                     Command              State    Ports  
--------------------------------------------------------------------
email-worker_db_1   docker-entrypoint.sh postgres   Up      5432/tcp

listando tables

catalunha@pop-os:~/dockers/email-worker$ docker-compose exec db psql -U postgres -c '\l'
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)

catalunha@pop-os:~/dockers/email-worker$ docker compose down
[+] Running 2/2
 ⠿ Container email-worker_db_1   Removed                                                                                         0.4s
 ⠿ Network email-worker_default  Removed                                                                                         0.2s

## 2
catalunha@pop-os:~/dockers/email-worker$ docker-compose ps
Name   Command   State   Ports
------------------------------

catalunha@pop-os:~/dockers/email-worker$ docker-compose up -d
Creating network "email-worker_default" with the default driver
Creating volume "email-worker_dados" with default driver
Creating email-worker_db_1 ... done

catalunha@pop-os:~/dockers/email-worker$ docker-compose exec db psql -U postgres -f /scripts/check.sql
                                  List of databases
     Name     |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
--------------+----------+----------+------------+------------+-----------------------
 email_sender | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 postgres     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
              |          |          |            |            | postgres=CTc/postgres
 template1    | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
              |          |          |            |            | postgres=CTc/postgres
(4 rows)

You are now connected to database "email_sender" as user "postgres".
                                    Table "public.emails"
  Column  |            Type             |                      Modifiers                      
----------+-----------------------------+-----------------------------------------------------
 id       | integer                     | not null default nextval('emails_id_seq'::regclass)
 data     | timestamp without time zone | not null default now()
 assunto  | character varying(100)      | not null
 mensagem | character varying(250)      | not null

## 3
catalunha@pop-os:~/dockers/email-worker$ docker-compose down
Stopping email-worker_db_1 ... done
Removing email-worker_db_1 ... done
Removing network email-worker_default
catalunha@pop-os:~/dockers/email-worker$ docker-compose up -d
Creating network "email-worker_default" with the default driver
Creating email-worker_db_1       ... done
Creating email-worker_frontend_1 ... done
catalunha@pop-os:~/dockers/email-worker$ 

## 4
catalunha@pop-os:~/dockers/email-worker$ docker-compose up -d
Creating network "email-worker_default" with the default driver
Creating email-worker_db_1       ... done
Creating email-worker_frontend_1 ... done
Creating email-worker_app_1      ... done
catalunha@pop-os:~/dockers/email-worker$ docker-compose logs -f -t

# 5
catalunha@pop-os:~/dockers/email-worker$ docker-compose exec db psql -U postgres -d email_sender -c 'select * from emails'

# 6
catalunha@pop-os:~/dockers/email-worker$ docker-compose up -d --scale worker=3
catalunha@pop-os:~/dockers/email-worker$ docker-compose logs -f -t worker


# Acessando o container via terminal
catalunha@pop-os:~$ docker exec -t -i email-worker_db_1 /bin/bash
root@b5b224eee2a2:/# ls
bin   docker-entrypoint-initdb.d  home	 media	proc  sbin     sys  var
boot  docker-entrypoint.sh	  lib	 mnt	root  scripts  tmp
dev   etc			  lib64  opt	run   srv      usr
root@b5b224eee2a2:/# psql
psql: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  role "root" does not exist
root@b5b224eee2a2:/# psql -U postgres -d email_sender
psql (9.6.24)
Type "help" for help.

email_sender=# \c
You are now connected to database "email_sender" as user "postgres".
email_sender=# \dt
         List of relations
 Schema |  Name  | Type  |  Owner   
--------+--------+-------+----------
 public | emails | table | postgres
(1 row)

email_sender=# 
