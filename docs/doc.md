# docker
docker login --username=catalunha

# Container
docker container create imageName
docker container exec -it containerXName commandX
docker container exec -it containerXName ping ipcontainerY
docker container exec containerXName uname -or
docker container inspect containerXName
docker container logs containerXName
docker container ls -a
docker container restart containerXName
docker container rm containerXName
docker container run --name containerXName -it imageName commandX
docker container run --rm --net none imageName commandX
docker container run --rm imageName commandX
docker container run -d --name containerXName --net host imageName sleep 1000
docker container run -d --name containerXName --net networkX imageName sleep 1000
docker container run -d --name containerXName -p portHost:portContainer -v pathHost:pathContainer imageName
docker container run -d --name containerXName imageName sleep 1000
docker container run -it  imageName commandX
docker container run -it --volumes-from=containerYName containerY cat /pathLogs
docker container run -p portHost:portContainer -v pathHost:pathContainer imageName
docker container run -p portHost:portContainer imageName
docker container run imageName
docker container run imageName commandX
docker container start -ai containerXName
docker container start containerXName
docker container stop containerXName

## flags
-i : Keep STDIN open even if not attached
-a : Attach to STDIN, STDOUT or STDERR
-t : Allocate a pseudo-TTY
--rm : Automatically remove the container when it exits
--name string : Assign a name to the container
-d : Run container in background and print container ID
-p portHost:portContainer : Publish a container's port(s) to the host
-v pathHost:pathContainer : Bind mount a volume

# image
docker image build --build-arg argName=argValue -t imageName .
docker image build -t imageName:imageVersion pathToDockerfile
docker image ls
docker image pull imageName:imageVersion
docker image rm imageName
docker image tag imageNameOld:imageVersionOld imageNameNew:imageVersionNew
docker image inspect imageName
docker image push imageX:imageVersion


## flags
-t : Name and optionally a tag in the 'name:tag' format

## Dockerfile
FROM imageName:imageVersion
LABEL <key>=<value>
RUN commandX
ENV
ARG
COPY pathFileHost pathFileContainer
USER
VOLUME
WORKDIR 
EXPOSE
ENTRYPOINT []
CMD []



# volume
docker volume ls

## flags

# network
docker network create --driver bridge networkX
docker network disconnect bridge containerXName
docker network ls

## flags

# compose
docker compose down
docker-compose exec commandX
docker-compose logs -f -t
docker-compose ps
docker-compose up
docker-compose up -d

## flags
-f : Follow log output.
-t : Show timestamps.
-d : Detached mode: Run containers in the background,

# enviar image ao dockerHub
docker image push catalunha/imageX:imageVersion
docker login --username=catalunha


# comandos para cada imagem

## debian
bash
bash --version
bash -c 'echo $S3_BUCKET'
ifconfig

## alpine
ash -c 'ifconfig'
ifconfig
ash

## nginx
echo '<h1>Hello<h1>' > /usr/share/nginx/html/index.html

## postgres
/bin/bash
db psql -U postgres -c '\l'
db psql -U postgres -d databaseName -c 'SQL'
db psql -U postgres -f /scripts.sql
db psql -U postgres -f /scripts/check.sql

# paths importantes em cada imagem


## nginx
* /usr/share/nginx/html
  * Páginas html
* /etc/nginx/conf.d
  * Pasta com config 
