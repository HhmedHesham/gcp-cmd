gcloud config set compute/zone us-central1-b // set the project zone 

gcloud container clusters create io // create cluster called io 

gsutil cp -r gs://spls/gsp021/* . // copy the files from the bucket

kubectl create deployment nginx --image=nginx:1.10.0 // create a deployment called nginx with the image nginx:1.10.0

kubectl get pods // get the pods

kubectl expose deployment nginx --port 80 --type LoadBalancer // expose the deployment to the internet on port 80

kubectl get services // get the services of the cluster so you can see the external ip

kubectl create -f pods/monolith.yaml // create pod called monolith from the monolith.yaml file

kubectl get pods // get the pods to see the monolith pod

kubectl describe pods monolith // describe the monolith pod

kubectl port-forward monolith 10080:80 // forward port 10080 to port 80 on the monolith pod so you can access it locally on port 10080 

curl http://127.0.0.1:10080 // curl the monolith pod on port 10080 to see the output of the monolith pod 

kubectl logs monolith // get the logs of the monolith pod

kubectl logs -f monolith // get real time logs of the monolith pod

kubectl exec monolith --stdin --tty -c monolith -- /bin/sh // exec into the monolith pod and run /bin/sh which is useful for debugging cause its opens a shell so you can run commands

kubectl create secret generic tls-certs --from-file tls/ // create a secret called tls-certs from the tls folder which contains the tls certs for the cluster 

kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf // create a configmap which is a way to store non-sensitive data in kubernetes called nginx-proxy-conf from the proxy.conf file in the nginx folder

kubectl create -f pods/secure-monolith.yaml // create a secure monolith pod from the secure-monolith.yaml file 

gcloud compute firewall-rules create allow-monolith-nodeport \ // create a firewall rule to allow traffic on port 31000 on the monolith pods 
  --allow=tcp:31000

gcloud compute instances list // list the instances in the cluster to get the external ip of the monolith pod 

curl -k https://<EXTERNAL_IP>:31000 // curl the monolith pod on port 31000 to see the output of the monolith pod 

kubectl get pods -l "app=monolith" // get the pods with the label app=monolith to see the monolith pods 

kubectl get pods -l "app=monolith,secure=enabled" // get the pods with the label app=monolith and secure=enabled to see the secure monolith pods

kubectl label pods secure-monolith 'secure=enabled' // label the secure-monolith pod with secure=enabled 

kubectl get pods secure-monolith --show-labels // get the secure-monolith pod and show the labels 

kubectl describe services monolith | grep Endpoints // get the endpoints of the monolith service 

kubectl create -f deployments/auth.yaml // create the auth deployment from the auth.yaml file 

kubectl create -f services/auth.yaml // create the auth service from the auth.yaml file 

kubectl create -f deployments/hello.yaml  // create the hello deployment from the hello.yaml file 
kubectl create -f services/hello.yaml  // create the hello service from the hello.yaml file 

kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf // create a configmap called nginx-frontend-conf from the frontend.conf file in the nginx folder

kubectl create -f deployments/frontend.yaml // create the frontend deployment from the frontend.yaml file 

kubectl create -f services/frontend.yaml // create the frontend service from the frontend.yaml file 

kubectl get services frontend // get the frontend service to get the external ip 

curl -k https://<EXTERNAL-IP> // curl the frontend service to see the output of the frontend service
( -k is used to ignore the self signed cert )





























Services use labels to determine what Pods they operate on. If Pods have the correct labels, they are automatically picked up and exposed by our services.

The level of access a service provides to a set of pods depends on the Service's type. Currently there are three types:

ClusterIP (internal) -- the default type means that this Service is only visible inside of the cluster,
NodePort gives each node in the cluster an externally accessible IP and
LoadBalancer adds a load balancer from the cloud provider which forwards traffic from the service to Nodes within it.
Now you'll learn how to:

Create a service

Use label selectors to exposAhm