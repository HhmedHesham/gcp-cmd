
gcloud auth list => to determine the credentials to use.

gcloud config list project => to determine the project ID and credentials.

gcloud config list --format 'value(core.project)' => to determine the project ID.

gcloud init: Initialize, authorize, and configure the gcloud CLI.

gcloud version: Display version and installed components.

gcloud components install: Install specific components.

gcloud components update: Update your gcloud CLI to the latest version.

gcloud config set project: Set a default Google Cloud project to work on.

gcloud info: Display current gcloud CLI environment details.

gcloud config set: Define a property (like compute/zone) for the current configuration.

gcloud config get-value: Fetch the value of a gcloud CLI property.

gcloud config list: Display all the properties for the current configuration.

gcloud config configurations create: Create a new named configuration.

gcloud config configurations list: Display a list of all available configurations.

gcloud config configurations activate: Switch to an existing named configuration.

// create custom network
gcloud compute networks create <NetworkName> --subnet-mode=custom

// create subnet in custom network
gcloud compute networks subnets create <SubnetName> --network=<NetworkName> --region=<Region> --range=<CIDR>

// create firewall rule
gcloud compute firewall-rules create <FirewallRuleName> --network=<NetworkName> --allow=<Protocol>:<Port> --direction=INGRESS --source-ranges=<CIDR> --rules=<Protocol>:<Port>

// create instance
gcloud compute instances create <InstanceName> --zone=<Zone> --machine-type=<MachineType> --subnet=<SubnetName> --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=<ServiceAccount> --scopes=https://www.googleapis.com/auth/cloud-platform --image=<Image> --image-project=<ImageProject> --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=<InstanceName> --reservation-affinity=any

// create instance with startup script
gcloud compute instances create <InstanceName> --zone=<Zone> --machine-type=<MachineType> --subnet=<SubnetName> --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=<ServiceAccount> --scopes=https://www.googleapis.com/auth/cloud-platform --image=<Image> --image-project=<ImageProject> --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=<InstanceName> --reservation-affinity=any --metadata-from-file startup-script=<StartupScript>

// list the available VPC networks
gcloud compute networks list

// create cluster with 3 nodes and custom network
gcloud container clusters create <ClusterName> \ --machine-type <MachineType> \ --num-nodes 3 \ --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw" \ --zone <Zone> \ --network <NetworkName> \ --subnetwork <SubnetName>


gsutil cp <LocalFile> gs://<BucketName>/<RemoteFile>

gsutil cp gs://<BucketName>/<RemoteFile> <LocalFile>

gsutil rm gs://<BucketName>/<RemoteFile>

// create a deployment and a replicaset
kubectl create -f <DeploymentFile> 

what is a replicaset ?
A ReplicaSets purpose is to maintain a stable set of replica Pods running at any given time. 
As such, it is often used to guarantee the availability of a specified number of identical Pods.

When a deployment is updated with a new version, 
it creates a new ReplicaSet and slowly increases the number of replicas in the new ReplicaSet as it decreases the replicas in the old ReplicaSet.
as in image_1.png


// get deployments and replicasets
kubectl get deployments
kubectl get replicasets

// get pods : Pod is created by the Kubernetes when the ReplicaSet is created and it is managed by the ReplicaSet.
kubectl get pods

// get services
kubectl get services

// scale up the deployment
kubectl scale deployment <DeploymentName> --replicas=<Replicas>

// get number of pods running in the deployment
kubectl get deployment <DeploymentName> -o jsonpath='{.status.availableReplicas}'
or 
kubectl get pods | grep <DeploymentName>- | wc -l

// get the external IP of the service
kubectl get services <ServiceName> -o jsonpath='{.status.loadBalancer.ingress[0].ip}'

// see the new entry in the rollout history of the deployment 
kubectl rollout history deployment/<DeploymentName>