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

Q5. what is flannel? (CNI)
A. Flannel is basically the networking system we added to our Kubernetes cluster.
Kubernetes itself doesn't provide the actual Pod-to-Pod network implementation. It expects a CNI (Container Network Interface) plugin to provide that networking.

Q6. what is Workload?
A. A workload in Kubernetes represents an application or task that Kubernetes needs to run and manage. Kubernetes provides workload resources such as Deployments, StatefulSets, DaemonSets, Jobs, and CronJobs to manage Pods. For example, if I create a Deployment with three replicas of my EventCraft application, Kubernetes creates and maintains three Pods, and if one Pod fails, the Deployment ensures a replacement is created.
Easy memory trick:
Workload = What you want Kubernetes to run.
Pod = Where your container actually runs.

Q7. what's the difference between Docker compose and K8S pods?
A. Docker Compose is used to define and manage multiple Docker containers as an application, whereas a Kubernetes Pod is the smallest deployable unit in Kubernetes and can contain one or more tightly coupled containers

commands we learn today
1. kubectl get nodes (fetch all nodes)
2. kubectl get nodes -o wide (fetch nodes with full details)
3. kubectl create -f pod.yaml (command to create a pod using pod.yaml file)
4. kubectl get pods (to fetch all the running pods)
5. kubectl get pods -o wide (fetch pods with full details)
6. kubectl delete pod <podname> (to delete the specific pod)
7. kubectl describe pod <podname> (to get the details of a particular pod)
8. kubectl logs <podname> (provides to debug a pod and check the logs of a pod)

Q8. what is deployment?
A. deployment is the one who manage pods. for example you want to create 10 pods you'll define that in Yaml manifesto of deployment. and then the 
manifesto -> deployment -> Replica set -> pods
so here  deployment create defined pods with the help of replica set. 
why we use Deployment when there is pod.yaml already because with pod.yaml we have to create it manually one by one here deploment's replica set do it in few seconds

q9. what is a service?
A. a service is able to provide the stable network to the pods. suppose when a pod dies and we created a new Pod the pod get a new IP. instead of connnecting directly to the pod, client connect to the service and service forward the traffic to the appropriate pods.

Deployment → manages Pods
Pod → manage/runs containers
Service → connects users/applications to Pods

