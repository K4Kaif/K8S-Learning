# K8S-Learning
Q1.What is Kubernetes and why do we need it if we already have Docker?
A.Kubernetes is a container orchestration platform used to automate the deployment, scaling, networking, and management of containerized applications. Docker can run containers, but when we have many containers across multiple machines, manually managing them becomes difficult. Kubernetes helps solve problems such as scaling, self-healing, service discovery, and managing workloads across a cluster

Q2. explain Worker node and its components.
A. A Worker Node is a machine inside a Kubernetes cluster where your actual application workloads run.
worker node consist of three components. kubelet, kubeproxy and container runtime.
1. kubelet : it's like an agent that runs on every workernode which always ensure that the pod/container is running correctly or not
2. container runtime : The container runtime is the component that actually handles running containers.
--It handles things such as:
    1. pulling container images
    2. creating containers
    3. starting containers
    4. stopping containers
    5. managing container lifecycle
3. kubeproxy : kube-proxy is a networking component that runs on nodes and helps implement Kubernetes Service networking

Q3. what is the kube-apiServer from the control plane ?
A. The kube-apiserver is the front end of the Kubernetes control plane. It exposes the Kubernetes API and acts as the central entry point through which kubectl, users, and other Kubernetes components communicate with the cluster

Q4. what is the use of kubernetes bootstrap token? "cluster administration"
A. A kubeadm bootstrap token is a temporary credential used to authenticate a new node while it joins an existing Kubernetes cluster.
  1. On k8s-master

Generate a fresh join command:

kubeadm token create --print-join-command

It will give you:

kubeadm join 192.168.175.141:6443 \
  --token xxxxx.xxxxxxxxxxxxxxxx \
  --discovery-token-ca-cert-hash sha256:xxxxxxxx
2. On k8s-worker3

Run that generated command:

sudo kubeadm join 192.168.175.141:6443 \
  --token xxxxx.xxxxxxxxxxxxxxxx \
  --discovery-token-ca-cert-hash sha256:xxxxxxxx

Then on the master:

kubectl get nodes

You'll see:

k8s-master     Ready
k8s-worker1    Ready
k8s-worker2    Ready
k8s-worker3    Ready
