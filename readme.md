# Kubernets
<details>
  <summary>HOW TO!</summary>

  ## Instalation Kubernets
  1. curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  2. sudo install minikube-linux-amd64 /usr/local/bin/minikube
 
 ## Instalation Kubectl
  1. curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
  2. sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  3. kubectl version --client

  ## Kubernets Commands
  1. ### Manage your cluster
     1. Pause Kubernetes without impacting deployed applications:
        * minikube pause
     2. Unpause a paused instance:
        * minikube unpause
     3. Halt the cluster
        * minikube stop
     4. Increase the default memory limit (requires a restart):
        * minikube config set memory 16384
     5. Browse the catalog of easily installed Kubernetes services:
        * minikube addons list
     6. Create a second cluster running an older Kubernetes release:
        * minikube start -p aged --kubernetes-version=v1.16.1
     7. Delete all of the minikube clusters:
        * minikube delete --all
  2. ### Deploy Applications
     1. Create a sample deployment and expose it on port 8080:
        * kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
        * kubectl expose deployment hello-minikube --type=NodePort --port=8080
     2. It may take a moment, but your deployment will soon show up when you run:
        * kubectl get services hello-minikube
     3. The easiest way to access this service is to let minikube launch a web browser for you:
        * minikube service hello-minikube
     4. Alternatively, use kubectl to forward the port:
        * kubectl port-forward service/hello-minikube 7080:8080
</details>

---
<br>

# Project Requiriments

### Kubernetes 
- Start kubernets with a minumum 8192 of memory  and 4 cpus 
>   minikube start --memory 8192 --cpus 4

---
<br>

### Build the docker image 
- Build the docker image with spark and hadoop <br>
    OBS: change the docker images repository to the minikube images repository before !!
> eval $(minikube docker-env) <br>
> docker build -f docker/Dockerfile -t spark-hadoop:3.2.0 ./docker
---
<br>

### Create the deployments and services
1. kubectl create -f ./kubernetes/spark-master-deployment.yaml
2. kubectl create -f ./kubernetes/spark-master-service.yaml
3. kubectl create -f ./kubernetes/spark-worker-deployment.yaml
4. kubectl create -f ./kubernetes/spark-worker-service.yaml
5. minikube addons enable ingress
6. kubectl apply -f ./kubernetes/minikube-ingress.yaml

### Add an entry to /etc/hosts:
1. echo "$(minikube ip) spark-kubernetes-master spark-kubernetes-woker" | sudo tee -a /etc/hosts
2. http://spark-kubernetes-master/
3. http://spark-kubernetes-worker/

# Test 
1. Get you master container ip address
   * kubectl get pods -o wide
2. start a pyspark shell
   * kubectl exec `<spark-master-pod>` -it -- pyspark --conf spark.driver.bindAddress=`<spark-master-ip-address>` --conf spark.driver.host=`<spark-master-ip-address>`
