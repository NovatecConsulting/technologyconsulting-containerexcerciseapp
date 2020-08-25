
The provided Vagrantfile and ansible playbooks serve to set up a real Kubernetes
cluster consisting of a master node and two worker nodes.

It is based on this article (with does not work out of the box):
https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/

./vagrant up --provision

When everything is finished, ssh into k8s-master via
./vagrant ssh k8s-master

Launch the application on k8s-master VM by
cd /deploy/
devspace dev

This will rebuild the two required Docker images (~120 s for me, be patient) and stores
them in the local registry created by ansible rules. By this, this example perfectly
serves to try out changes in the application without having to upload new images all
the time to the internet.

The todoui web frontend can now be opened on the host in any webbrowser by calling this url:
http://localhost:8090/

The required port forwarding rules are included in the Vagrantfile and the kubernetes
.yml files.

Opening the devspace UI is available by doing ssh with additional parameters:
./vagrant ssh k8s-master -- -L 8099:localhost:8090
Then open on the host browser the devspace UI:
http://localhost:8099
