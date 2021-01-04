### DOCKER

## build image

`docker build .`

- using tag could be version like github sha
  - `docker build -t name:tag .`

## run container

`docker run -p 8000:80 imageidxxx`

## Run container and removed after close with also container name

`docker run -p 3000:80 -d --rm --name example-container-name imageidxxx`

## Check container

`docker ps -a`

## Check running container

`docker ps`

## Check images

`docker images`

## Remove container

`docker rm containername`

## Remove images

`docker rmi imageid`

## Remove images / conatiner

`docker prune / docker image prune`

## inspect image

`docker image inspect imageid`

## copy files or folders could be inside a container

- `docker cp directory-or-file-to-be-copy destiny`
- `docker cp directory-or-file-to-be-copy container-name:/forder-or-directory`

## name container

`docker run -p expose-port-in-browser:app-port -d --rm --name goalkube imageid`

## name image is a tag

- `name:tag = REPOSITORY:TAG = name:version`
- `e.g FROM node:12`
- `docker build -t name:tag .`
- `docker build -t goals:latest-sergio .`

## you can start a container using name and tag

`docker run -p 8000:80 -d --rm --name surfing goals:latest-sergio`

## rename images

- `docker tag current-REPOSITORY:TAG new-docker-hub-sergiouk10/node-app-server`
- e.g: `docker tag goals:latest-sergio sergiouk10/node-app-server`

# share images in dockerhub

## create image to push push to dockerhub

- `create dockerhub repo`
- `get slug e.g: sergiouk10/node-app-server`
- `build image with -t e.g: docker build -t sergiouk10/node-app-server:version-example .`

## push image to dockerhub

`docker push sergiouk10/node-app-server:version-example`

## pull image from dockerhub

`docker pull sergiouk10/node-app-server`

## add volumes with -v, path and name

`docker run -p 3000:80 -d --rm --name -v volume-name-feedback:/app/feedback feedback-node:volumes`

## volume list

`docker volume ls`

## remove volume

- `docker volume prune`
- `docker volume rm vol_name`

## binding volume path

- `we need to add a relative path and the path inside your container`
- `docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/sergiohernandez/Documents/cursos/data-volumes-01-starting-setup:/app" feedback-node:volumes`
  `we can use ---> -v $(pwd):/app`

- note: we need to add node modules in volume

`docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/sergiohernandez/Documents/cursos/data-volumes-01-starting-setup:/app" -v /app/node_modules feedback-node:volumes`

- we can read only adding extra : at the end eg: `-v "/Users/sergiohernandez/Documents/cursos/data-volumes-01-starting-setup:/app:ro"`

### good example of volume NOTE: needs nodemon to update container

- docker run -p 3000:80 -d --rm --name feedback-app -v "/Users/sergiohernandez/Documents/cursos/docker-2-first-image:/app:ro" -v /app/node_modules node-server:v1

## env to run docker

`docker run -p 3000:8000 --env PORT=8000 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/sergiohernandez/Documents/cursos/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules -v /app/temp feedback:env`

## with env file

- with env file->

  - `docker run -p 3000:8000 --env-file ./.env -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/sergiohernandez/Documents/cursos/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules -v /app/temp feedback:env`

- without env file->
  - `docker run -p 3000:8000 --env PORT=8000 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/sergiohernandez/Documents/cursos/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules -v /app/temp feedback:env`

e.g.

- FROM node:14

- WORKDIR /app

- COPY package.json /app

- RUN npm install

- COPY . /app

- ENV PORT 80

- EXPOSE $PORT

- CMD ["npm", "start"]

## arguments art

## get sha

`git rev-parse --short HEAD`

## push images with tag sha

`docker push sergiouk10/node-app-server:$(git rev-parse --short HEAD)`

# Networking

## Between containers

To comunicate containers we need to create a network adding the --network tag
use name of the container name address with db as domain also you can inspect container and add the IP address

steps:

- create image for your containers
  - `docker run mongo`
  - `docker build -t f-node:latest .`
- create network
  - `docker network --help`
  - `docker network create favorites-net`
  - `docker network ls`
- add network to your container
  - `docker run -d --name mongodb --network favorites-net mongo`
  - `docker run --name favorites --rm -p 3000:3000 -d --network favorites-net f-node:latest`
- add name or your network to your host in http request

NOTE: you can inspect containers

- `docker container inspect mongodb`

## Between external db or your host machine

we can just use host.docker.interal in your http request

### NOTE: docker-compose up with example in root

## utility container

1. create image for initial container Dockerfile
2. run image with volumen binding and (npm init in iteractive mode in this case)

- display your container in the root dir
  - e.g: `docker run -it -v /Users/sergiohernandez/Documents/cursos/docker-2-first-image:/app name-of-rnning-image npm init`

# Deployment EC2 instance

## AWS must display the image using the dockerhub image, simple way with EC2 instance

### Steps

- create EC2 server with privileges could be type:HTTP and slecting port:80 or type:alltraffic port:all protocol:all
- set config for this servwr with docker inside and uese the dockerhub image

### to refrest changes simple way with EC2 instance

### Steps

- Rebuild image and update image tagged
  - `docker build -t image-to-be-build .`
  - `docker tag image-to-be-build dockerhub-account/repo-name`
- Push to dockerhub
  - `docker push dockerhub-account/repo-name`
- Make sure we use the latest image in the server pulling latest version NOTE: image must be stopped a removed first
  - `docker pull dockerhub-account/repo-name`
  - `docker pulrun -d --rm -p 80:80 dockerhub-account/repo-name`

# Kubernetes

## Architecture Theory

1. Pod: small unit to holds and execute container/s
2. Worker node: Wrap Pod e.g: this could be like EC2 instance in AWS another example is like my local machine

- run the container of your app
- node anr your machines / virtual instances
- managed by master node

3. Proxy: Handle request into the node to the pod for external user
4. Master node / the control Plane: its like a remote machine responsible for interac with nodes

### Worker Node

1. Kubelet: communication between master and worker node
2. Docker: needs to have docker env installed like any other instance
3. Pod: holds or wrap, and execute container/s, volumes or any other container setup or tool
4. Kube-proxy: Managed node and pod network communication handle traffic to your worker node

### Master Node

1. API server: handle communication between worker and master node, counter point por kubelet to communicate
2. Schedule: watches for new pods, select worker nodes to run them on
3. Kube-controller-manager: watch and controls worker nodes, correct number of pods
4. Cloud-controller-manager: translade instructionsto cloud provider

## Minikube instalation

- links:
  - https://kubernetes.io/docs/tasks/tools/install-kubectl/
  - https://kubernetes.io/docs/tasks/tools/install-minikube

### To run and start minikube if you have a hyperkit installed if ins't could be with docker

- `minikube start --driver=hyperkit`
- `minikube start --driver=docker`

### check minikube cluster is up a running

- `minikube status`

### open dashboard in the browser

- `minikube dashboard`

### we use this `kubectl` command to send instructions to the cluster e.g: create deployments

- `kubectl --help`

## deployment pod

### create new deployment cluster

- `kubectl create --help`
- `kubectl create deployment new-deployment-pod-name --image=sergiouk10/kub-first-app`

NOTE: image name must be in dockerhub to be able to get it
remenber steps:

1. build image

- `docker build -t image-name .`

2. create dockerhub repocitory
3. re-tag image to dockerhub repo name

- `docker tag image-name sergiouk10/dockerhub-new-image-name`

4. push to dockerhub with new image name

- `docker push sergiouk10/dockerhub-new-image-name`

### get deployments

- `kubectl get deployments`

### get pods

- `kubectl get pods`

### delete deployments

- `kubectl delete deployment new-deployment-pod-name`

## service

Group pods to shared IP address, we need to expose a deployment

### check services

- `kubectl get services`

### Expose container creating service

1. Include expose in kebectl command

- `kubectl expose deployment`

2. Especify name of deployment

- `kubectl expose deployment first-app`

3. Especify port to expose

- this is the container exposed port eg: 8080
- `kubectl expose deployment first-app --port=8080`

4. Include type of server or exposings

- Posible types

  - ClusterIP: this means will only be reachable inside this cluster (this is the default)

    - `kubectl expose deployment first-app --type=ClusterIP --port=8080`

  - NodePort: this deployment should be expose with help of the IP address of the worker node

    - `kubectl expose deployment first-app --type=NodePort --port=8080`

  - LoadBalancer: utilice loadbalancer which have to exis in the infrastruture on which our cluster runs and this loadbalancer generate a unique IP address for this server and also distribute incoming traffic across our pods part of this services

    - `kubectl expose deployment first-app --type=LoadBalancer --port=8080`

NOTE: loadbalancer can be use only if your cluster or provider suported e.g: AWS and also minikube does support this

- `kubectl expose deployment first-app --type=LoadBalancer --port=8080`

### display in browser

- `kubectl get services`
- `minikube service services-name`
- e.g: `minikube service first-app`
