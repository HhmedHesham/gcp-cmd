docker run hello-world // run the hello-world image

( image pulled from the Docker Hub public registry  When the docker daemon can't find an image locally, it will by default search the public registry for the image. )

docker images // list all images

docker ps // list all running containers 

docker ps -a // list all containers (running and stopped) 

// create DockerFile 
cat > Dockerfile <<EOF
# Use an official Node runtime as the parent image
FROM node:lts
# Set the working directory in the container to /app
WORKDIR /app
# Copy the current directory contents into the container at /app
ADD . /app
# Make the container's port 80 available to the outside world
EXPOSE 80
# Run app.js using node when the container launches
CMD ["node", "app.js"]
EOF

// This is a simple HTTP server that listens on port 80 and returns "Hello World".
cat > app.js <<EOF
const http = require('http');
const hostname = '0.0.0.0';
const port = 80;
const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello World\n');
});
server.listen(port, hostname, () => {
    console.log('Server running at http://%s:%s/', hostname, port);
});
process.on('SIGINT', function() {
    console.log('Caught interrupt signal and will exit');
    process.exit();
});
EOF

docker build -t node-app:0.1 . // build the image from the Dockerfile in the current directory and tag it as node-app:0.1 
(-t is to name and tag an image with the name:tag syntax)
( the tag will default to latest ) 

docker images // list all images 

docker run -p 4000:80 --name my-app node-app:0.1 // run the image in a container named my-app and map port 4000 on the host to port 80 in the container 
(-p is to map a port from the host to the container)
(--name is to name the container)

curl http://localhost:4000 // test the app 

docker stop my-app && docker rm my-app // stop and remove the container 

docker run -p 4000:80 --name my-app -d node-app:0.1 // run the image in a container named my-app and map port 4000 on the host to port 80 in the container in detached mode 
(-d is to run the container in detached mode)

docker ps // list all running containers 

docker logs [container_id] // view the logs of a container 

docker run -p 8080:80 --name my-app-2 -d node-app:0.2 // run the image in a container named my-app-2 and map port 8080 on the host to port 80 in the container in detached mode
docker ps // list all running containers\

curl http://localhost:8080 // test the sec app 
curl http://localhost:4000 // test the first app

docker exec -it [container_id] bash // run bash in the container 
( -it is to run the container in interactive mode )

docker inspect [container_id] // inspect the container SO YOU CAN SEE THE IP ADDRESS OF THE CONTAINER 

docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [container_id] // inspect the container and get the IP address of the container 

docker tag node-app:0.2 gcr.io/[project-id]/node-app:0.2 // tag the image with the Google Container Registry (GCR) repository URL 
( gcr.io/[project-id]/node-app:0.2 is the GCR repository URL )


