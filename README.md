DOCKER INFO

# build image

docker build .

# run container

docker run -p 8000:80 imageidxxx

# Run container and removed after close

docker run -p 3000:80 -d --rm imageidxxx

# Check container

docker ps -a

# Check running container

docker ps

# Check images

docker images

# Remove container

docker rm containername

# Remove images

docker rmi imageid

# Remove images / conatiner

docker prune / docker image prune

# inspect image

docker image inspect imageid

# copy files or folders could be inside a container

docker cp directory-or-file-to-be-copy destiny
docker cp directory-or-file-to-be-copy container-name:/forder-or-directory

# name container

docker run -p expose-port-in-browser:app-port -d --rm --name goalkube imageid

# name image is a tag

name:tag = REPOSITORY:TAG = name:version
e.g FROM node:12
docker build -t name:tag .
docker build -t goals:latest-sergio .

# you can start a container using name and tag

docker run -p 8000:80 -d --rm --name surfing goals:latest-sergio
