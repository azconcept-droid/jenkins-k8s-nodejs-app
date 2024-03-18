
## Dockerize node.js app 
docker build . -t express-demo    

docker run -d -it -p 0.0.0.0:80:3000 express-demo 

docker tag express-demo yalecthub/express-demo:v1

sudo docker login

docker push yalecthub/express-demo:v1

## Launch k8s (minikube)
### Install mninikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
sudo usermod -aG docker $USER && newgrp docker

### start minikube
minikube start --driver=docker --container-runtime=docker --network-plugin=cni --cni=calico   

###
k create deployment node-app --image yalecthub/express-demo:latest --replicas 2  
k expose deployment node-app --port 5000 --target-port 3000 --type NodePort  


## Launch Jenkins
cd jenkins  
docker compose up -d --build
## Access your jenkins from internet
```
http://<server-ip>:8090
```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword  

-- Destroy jenkins  
docker compose down -v

## Extra
minikube start --driver=docker --container-runtime=docker --network-plugin=cni --cni=calico  
minikube node add --worker  
alias k=kubectl  
k get nodes -owide  

k get po --all-namespaces   
k get po --all-namespaces -owide  

k get services  

k create namespace prod  
k create namespace dev  
k get namespace  
k config get-contexts  

k run node-app --image jomoflash/node-app  
k get po  
k delete po node-app  

k run node-app --image jomoflash/node-app --dry-run=client -oyaml  
k apply -f app.yml  

minikube delete --all
