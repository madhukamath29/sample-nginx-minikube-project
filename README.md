# sample-nginx-minikube-project
deploying static nginx webpage 
Step 1: We will install Minikube and kubectl on our ubuntu server
Docker Installation steps on ubuntu 
Pre-requisites: 

Instance type: t2.medium(2cpu and 4gib RAM)
Ubuntu version: 22.04
---------------------------------------------

sudo apt update
sudo apt install apt-transport-https curl

sudo apt install docker.io

sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker


---------------------------------------------

Installation of minikube 

sudo apt update
sudo apt install -y curl wget apt-transport-https


curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube version


curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

chmod +x kubectl
sudo mv kubectl /usr/local/bin/

kubectl version -o yaml

minikube start — driver=docker

minikube start --driver=docker --force

if the above command doesn't work, run the below commands 

exit

sudo usermod -aG docker $USER
newgrp docker

docker ps

minikube delete --all --purge

sudo sysctl fs.protected_regular=0

minikube start --driver=docker --cpus=2 --memory=2g

kubectl get nodes

minikube status

kubectl cluster-info

Step 2: Create a file nginx-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

Step 3: Apply the configurations
kubectl apply -f nginx-deployment.yaml

Step 4: Check the deployments 
kubectl get deployments
kubectl get pods

Step 5: Expose deployment as a service by creating nginx-service.yaml file 

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort

Step 6: Apply the configuration changes 
kubectl apply -f nginx-service.yaml

Step 7: Check the service 
kubectl get services

Step 8: Accessing nginx 
Go to the terminal and run the below command:
 
minikube service nginx-service
# This will automatically open nginx default page on the web browser 
