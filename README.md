# technologyconsulting-containerexcerciseapp

Following a short summary of the most important commands
to get these example files running in a local machine
setup running, tested on Ubuntu 18.04 with minikube.

= Install

== kubectl
sudo snap install kubectl --classic
kubectl --version

kubectl completion bash > kubectl_bashCompletion.sh
sudo mv kubectl_bashCompletion.sh /etc/bash_completion.d/kubectl

== virtualbox
deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian bionic contrib
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
sudo apt install virtualbox-6.0

== minikube

https://kubernetes.io/docs/setup/learning-environment/minikube/
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb

== devspace

https://github.com/devspace-cloud/devspace
curl -s -L "https://github.com/devspace-cloud/devspace/releases/latest" | sed -nE 's!.*"([^"]*devspace-linux-amd64)".*!https://github.com\1!p' | xargs -n 1 curl -L -o devspace && chmod +x devspace;
sudo install devspace /usr/local/bin

= Setup

minikube start --vm-driver=virtualbox
minikube stop
Virtualbox --> RAM = 4GB,CPU = 4 Cores

= Execute

== Launch minikube

minikube start
minikube dashboard
minikube tunnel > /dev/null

kubectl get nodes -o wide
>> NAME       STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE              KERNEL-VERSION   CONTAINER-RUNTIME
>> minikube   Ready    master   39h   v1.17.0   192.168.99.101   <none>        Buildroot 2019.02.6   4.19.76          docker://18.9.9
minikube dashboard

== Use local docker images
eval $(minikube docker-env)
and run the docker build commands once more

devspace init
devspace use namespace default
devspace dev

== Check for running services
kubectl get service
>> NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE
>> service/kubernetes    ClusterIP      10.96.0.1        <none>           443/TCP          39h
>> service/postgresdb    ClusterIP      10.108.117.68    <none>           5432/TCP         38h
>> service/todobackend   LoadBalancer   10.98.156.161    10.98.156.161    8080:30384/TCP   38h
>> service/todoui        LoadBalancer   10.106.236.156   10.106.236.156   8090:30273/TCP   38h

== Test the backend application
curl 192.168.99.101:30273/fail
curl 192.168.99.101:30273/hello
Browser: http://192.168.99.101:30273/

== Reload images with adapted source code
mvn -f todobackend install
mvn -f todoui install
