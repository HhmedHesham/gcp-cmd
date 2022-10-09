gcloud config set compute/zone us-east1-b // Set the default compute zone to us-east1-b 

gsutil -m cp -r gs://spls/gsp053/orchestrate-with-kubernetes . // Copy the sample code to your Cloud Shell instance. 

cd orchestrate-with-kubernetes/kubernetes                                 // Navigate to the sample code directory. 

gcloud container clusters create bootcamp \                              // create a cluster named bootcamp
  --machine-type e2-small \                                              // use a machine type with 2 vCPUs and 2 GB of memory
  --num-nodes 3 \                                                       // create 3 nodes in the cluster 
  --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw" // grant the cluster access to Cloud Storage and Cloud Source Repositories

kubectl explain deployment // Get information about the Deployment resource type. 

kubectl explain deployment --recursive // Get detailed information about the Deployment resource type. 

kubectl explain deployment.metadata.name // Get information about the name field of the Deployment resource type.

vi deployments/auth.yaml // Open the auth deployment configuration file in the Cloud Shell editor.

i // Enter insert mode. 

...
containers:
- name: auth
  image: "kelseyhightower/auth:1.0.0" // Update the image field to use the latest version of the auth image.
...

:wq // Save and exit the editor.

cat deployments/auth.yaml // View the updated auth deployment configuration file. 

kubectl get deployments // List the deployments in the current namespace.

kubectl get replicasets // List the replica sets in the current namespace. replica sets are the next level of abstraction in the Kubernetes object hierarchy. which is used to manage the pods in a deployment.

kubectl get pods // List the pods in the current namespace.

kubectl create -f services/auth.yaml // Create the auth service. 

kubectl create secret generic tls-certs --from-file tls/ // Create a secret named tls-certs from the files in the tls directory.


kubectl create secret generic tls-certs --from-file tls/  // Create a secret named tls-certs from the files in the tls directory.
kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf // Create a configmap named nginx-frontend-conf from the frontend.conf file.which is used to configure the nginx frontend.
kubectl create -f deployments/frontend.yaml // Create the frontend deployment.
kubectl create -f services/frontend.yaml // Create the frontend service.

kubectl get services frontend // Get the external IP address of the frontend service. 

curl -ks https://<EXTERNAL-IP> // Make a request to the frontend service. (-ks flags are used to ignore certificate errors.)

curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"` // Make a request to the frontend service using kubectl.

kubectl explain deployment.spec.replicas // Get information about the replicas field of the Deployment resource type.

kubectl scale deployment hello --replicas=5 // Scale the hello deployment to 5 replicas. 

kubectl get pods | grep hello- | wc -l // Count the number of hello pods. 
(grep is used to filter the output of the get pods command to only include pods with the hello prefix.) 
(wc -l is used to count the number of lines in the output of the grep command.)

kubectl scale deployment hello --replicas=3

kubectl get pods | grep hello- | wc -l

 ------------- rolling update -------------
 # just edit the deployment file and change the image version and it will automatically update the pods.

kubectl edit deployment hello // Edit the hello deployment with the new version 

to stop rolling update => kubectl rollout pause deployment/hello
to resume rolling update => kubectl rollout resume deployment/hello
to know the status of rolling update => kubectl rollout status deployment/hello
to roll back to previous version => kubectl rollout undo deployment/hello
to see the history of rolling update => kubectl rollout history deployment/hello

------------ canary deployment -------------
A canary deployment consists of a separate deployment with your new version and a service that targets both your normal, stable deployment as well as your canary deployment.
create a new canary deployment for the new version of the hello service.
kubectl create -f deployments/hello-canary.yaml
to make user access the ui of the first deployment use the session affinity.

------------ blue-green deployment -------------
A blue-green deployment consists of two deployments, one for the stable version and one for the new version.
kubectl apply -f services/hello-blue.yaml
kubectl create -f deployments/hello-green.yaml
kubectl apply -f services/hello-green.yaml
to roll back to previous version => kubectl apply -f services/hello-blue.yaml

 