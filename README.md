
## FOllow:-
Step 1) Installing Vagrant
Vagrant must first be installed on the machine you want to run it on. To make installation easy, Vagrant is distributed as a binary package for all supported platforms and architectures. Installing Vagrant is extremely easy. Head over to the Vagrant downloads page and get the appropriate installer or package for your platform. Install the package using standard procedures for your operating system.

After vagrant has been installed. You can verify its installation by running the binary to show the version

root@ubuntuvm01:~# ./vagrant version
Installed Version: 2.2.6
Latest Version: 2.2.6
You're running an up-to-date version of Vagrant!
Step 2) Install git
This may be necessary if it isn’t already installed on your system

apt-get install git -y
Step 3) Download The Repo
Next, you need to download the vagrant repo from BitBucket

git clone https://exxsyseng@bitbucket.org/exxsyseng/k8s_centos.git      # Centos k8s Cluster
git clone https://exxsyseng@bitbucket.org/exxsyseng/k8s_ubuntu.git      # Ubuntu k8s Cluster
Step 4) Change into Vagrant Provisioning Directory
Once the repo has been download, change into the vagrant provisioning directory and there you will see all the configuration files

root@ubuntuvm01:~# cd kubernetes/vagrant-provisioning
root@ubuntuvm01:~/kubernetes/vagrant-provisioning# ls -ltr
total 24
-rw-r--r-- 1 root root 1073 Oct 29 20:48 Vagrantfile
-rw-r--r-- 1 root root 3202 Oct 29 20:48 kube-flannel.yml
-rw-r--r-- 1 root root 2394 Oct 29 20:48 bootstrap.sh
-rw-r--r-- 1 root root  337 Oct 29 20:48 bootstrap_kworker.sh
-rw-r--r-- 1 root root  726 Oct 29 20:48 bootstrap_kmaster.sh
-rw-r--r-- 1 root root  839 Oct 29 20:48 bootstrap_kmaster_calico.sh
Vagrantfile: The primary function of the Vagrantfile is to describe the type of machine required for a project, and how to configure and provision these machines. In this file, you will have settings such as operating system type, hostnames of your VMs in the cluster, node count for a few examples. The file also specifies the bootstrap scripts.

bootstrap.sh: This file is a basic set of initial instructions that will generally be applied to both the master Kubernetes node and the worker nodes as well. It does things such as update the /etc/hosts file to include all the nodes, installs docker, disable selling and firewalld. It also installed Kubernetes 

bootstrap_kmaster.sh: This file Initialize Kubernetes, create the flannel network

bootstrap_kworker: This script will join the worker nodes to the cluster

Almost all interaction with Vagrant is done through the command-line interface. The interface is available using the vagrant command, and comes installed with Vagrant automatically. The vagrant command in turn has many sub commands.
MicroK8s GPU Workstation | Kubernetes

Step 5) Bringing up the Kubernetes Cluster
To create and configure our Kubernetes cluster run the Vagrant command with the up flag. The binary will read the Vagrantfile and bring up the Kubernetes cluster.

root@ubuntuvm01:~/kubernetes/vagrant-provisioning# vagrant up
Bringing machine 'kmaster' up with 'virtualbox' provider...
Bringing machine 'kworker1' up with 'virtualbox' provider...
Bringing machine 'kworker2' up with 'virtualbox' provider...
Step 6) Checking the Status
Once the cluster is completed build you can check the status of there system with the status flag.

exx@microk8s-01:~/kubernetes/vagrant-provisioning$ vagrant status
Current machine states:
kmaster                   running (virtualbox)
kworker1                  running (virtualbox)
kworker2                  running (virtualbox)
Step 7) Logging into the Head Node
To log into the head node or any of the nodes in the cluster simply run “vagrant ssh <hostname>.

exx@microk8s-01:~/kubernetes/vagrant-provisioning$ vagrant ssh kmaster
Last login: Wed Oct 30 00:27:31 2019
Your Cluster is now ready to use. You can verify by running “kubectl get nodes”.

[vagrant@kmaster ~]$ kubectl get nodes
NAME                   STATUS     ROLES    AGE    VERSION
kmaster.example.com    Ready   master   14m    v1.16.2
kworker1.example.com   Ready   <none>   11m    v1.16.2
kworker2.example.com   Ready   <none>   9m8s   v1.16.2
This variable specifies the model location based on what model is specified in the ‘PARAM_SET’ variable.

[vagrant@kmaster ~]$ kubectl get all --all-namespaces
NAMESPACE     NAME                                              READY   STATUS    RESTARTS   AGE
kube-system   pod/coredns-5644d7b6d9-9b7tv                      0/1     Pending   0          14m
kube-system   pod/coredns-5644d7b6d9-c7ls2                      0/1     Pending   0          14m
kube-system   pod/etcd-kmaster.example.com                      1/1     Running   0          13m
kube-system   pod/kube-apiserver-kmaster.example.com            1/1     Running   0          13m
kube-system   pod/kube-controller-manager-kmaster.example.com   1/1     Running   0          13m
kube-system   pod/kube-proxy-2p9vh                              1/1     Running   0          9m35s
kube-system   pod/kube-proxy-hpmd5                              1/1     Running   0          11m
kube-system   pod/kube-proxy-qj8h2                              1/1     Running   0          14m
kube-system   pod/kube-scheduler-kmaster.example.com            1/1     Running   0          13m
NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  14m
kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   14m
NAMESPACE     NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
kube-system   daemonset.apps/kube-proxy   3         3         3       3            3           beta.kubernetes.io/os=linux   14m
NAMESPACE     NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/coredns   0/2     2            0           14m
NAMESPACE     NAME                                 DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/coredns-5644d7b6d9   2         2         0       14m

###Credits

Credits to this person:- https://blog.exxactcorp.com/building-a-kubernetes-cluster-using-vagrant/

This was created because i could not clone the oroginal repo and i wanted to use the repo in a automation yml file

## Kubernetes

Kubernetes (commonly stylized as k8s[3]) is an open-source container-orchestration system for automating application deployment, scaling, and management. It aims to provide a "platform for automating deployment, scaling, and operations of application containers across clusters of hosts".[3] It works with a range of container tools, including Docker.[5] Many cloud services offer a Kubernetes-based platform or infrastructure as a service (PaaS or IaaS) on which Kubernetes can be deployed as a platform-providing service. 

---

## Tutorials

This tutorial has been prepared for those who want to understand the containerized infrastructure and deployment of application on containers. This tutorial will help in understanding the concepts of container management using Kubernetes.

