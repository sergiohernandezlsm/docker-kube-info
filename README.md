Readme file info

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
