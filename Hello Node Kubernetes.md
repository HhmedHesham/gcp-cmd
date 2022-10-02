############################ CREATE Node.js application ########################################

// open vim to write the server.js 
vi server.js

// enter i to edit on the vim
i

var http = require('http');                           // import http module 
var handleRequest = function(request, response) {     // create a function to handle the request 
  response.writeHead(200);                            // return status code 200 
  response.end("Hello World!");                       // return "Hello World!" 
}
var www = http.createServer(handleRequest);           // create a server
www.listen(8080);                                     // listen to port 8080

// press esc to exit the edit mode and enter :wq to save and quit the vim
:wq

// run and start the node server
node server.js 


############################ CREATE a Docker container image ########################################

// open Dockerfile to write the dockerfile
vi Dockerfile

// enter i to edit on the vim
i

// this code will create a docker image that will import node and run the server.js on port 8080 and copy the server.js to the image
FROM node:6.9.2     // import node image
EXPOSE 8080        // expose port 8080 
COPY server.js .   // copy the server.js to the image 
CMD node server.js // run the server.js


:wq // press esc to exit the edit mode and enter :wq to save and quit the vim 

docker build -t gcr.io/PROJECT_ID/hello-node:v1 .  // build the docker image and tag it with the project id and the version number 

// run the docker image with the name "hello-node" and tag "1.0" here we test the image locally by running a Docker container from the image we just built 
docker run -d -p 8080:8080 gcr.io/PROJECT_ID/hello-node:v1 // -d means run in background and -p means port mapping 


curl http://localhost:8080 // should return "Hello World!" 


docker ps // to see the running container

docker stop [CONTAINER ID] // to stop the container

gcloud auth configure-docker // to configure docker to use gcloud so we can push the image to the container registry (gcr.io)

docker push gcr.io/PROJECT_ID/hello-node:v1 // push the docker image to gcr.io for deployment to google cloud platform 


############################ CREATE A CLUSTER ########################################

gcloud config set project PROJECT_ID // to set the project id

gcloud container clusters create hello-world \    // to create a cluster with the name "hello-world"
                --num-nodes 2 \                   // with 2 nodes
                --machine-type n1-standard-1 \    // with the machine type "n1-standard-1"
                --zone us-central1-a              // in the zone "us-central1-a" 

############################ CREATE A POD ########################################

kubectl create deployment hello-node \         // to create a deployment with the name "hello-node"
    --image=gcr.io/PROJECT_ID/hello-node:v1    // with the image "gcr.io/PROJECT_ID/hello-node:v1"

kubectl get deployments // to see the deployment

kubectl cluster-info // to see the cluster info which contains the ip address of the cluster

kubectl config view // to see the config which is the cluster info

kubectl get events // to see the events which is useful for debugging 
 
kubectl logs <pod-name> // to see the logs 

############################ CREATE A SERVICE (allow external traffic) ########################################

kubectl expose deployment hello-node --type="LoadBalancer" --port=8080 // to expose the deployment to the internet 

kubectl get services // to see the services 

############################ SCALE THE CLUSTER ########################################

kubectl scale deployment hello-node --replicas=4 // to scale the deployment to 4 replicas 

kubectl get deployment  // to see the deployment 

kubectl get pods // to see the pods 

############################  Roll out an upgrade to your application ########################################

vi server.js // to edit the server.js 

i // to enter the edit mode 

response.end("Hello Kubernetes World!"); // to change the response 

:wq // to save and quit the vim 

docker build -t gcr.io/PROJECT_ID/hello-node:v2 . // to build the docker image with the name "hello-node" and tag "2.0" so we can push it to the container registry

docker push gcr.io/PROJECT_ID/hello-node:v2 // to push the docker image to gcr.io for the new version of the application for deployment

kubectl edit deployment hello-node // to edit the deployment so you can change your containers image to the new version, 
//Look for Spec > containers > image and change the version number to v2:


:wq // to save and quit the vim 

kubectl get deployments // now new pods will be created with the new version of the application and the old pods will be deleted 




