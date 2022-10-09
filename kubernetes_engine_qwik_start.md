gcloud container clusters create --machine-type=e2-medium --zone= lab-cluster  // create a cluster with 3 nodes of type e2-medium in zone europe-west1-b

gcloud container clusters get-credentials lab-cluster  // Authenticate with the cluster so we can use kubectl to interact with it 

kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0 // create a deployment with the hello-app image from google samples repository 

kubectl expose deployment hello-server --type=LoadBalancer --port 8080 // expose the deployment as a service of type LoadBalancer on port 8080 

kubectl get service // get the external IP of the service 

http://[EXTERNAL-IP]:8080 // access the service 

gcloud container clusters delete lab-cluster  // delete the cluster 

