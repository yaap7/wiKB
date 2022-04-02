# Kubernetes

In this documentation, I take notes from my first play of the [Learn Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/) tutorial.

There are notes about:

* [Glossary](#Glossary)
* [minikube](#minikube)
* [kubectl](#kubectl)

Otherwise, here is the official [cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/).

## Glossary

### Nodes and pods

> A node is a worker machine in Kubernetes and may be a VM or physical machine, depending on the cluster. Multiple Pods can run on one Node.

![Nodes and pods explanation](https://d33wubrfki0l68.cloudfront.net/5cb72d407cbe2755e581b6de757e0d81760d5b86/a9df9/docs/tutorials/kubernetes-basics/public/images/module_03_nodes.svg)

### Services

More explanation here: <https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/>

![Services and labels](https://d33wubrfki0l68.cloudfront.net/7a13fe12acc9ea0728460c482c67e0eb31ff5303/2c8a7/docs/tutorials/kubernetes-basics/public/images/module_04_labels.svg)

## minikube

If I understand well, `minikube` is used to provide a lightweight deployment environment.
When deploying in the cloud, it is possible to use `kubectl` to manage the cloud instances without using minikube.
I will try that next time I play with Kubernetes.

### minikube installation

Warning: the information here install `kubectl` and `minicube` from the current branch, but does not allow their automatic upgrades via apt or snap.
To get a propre installation media which allows automatic upgrades, see <https://kubernetes.io/fr/docs/tasks/tools/install-minikube/>

Installation of `minicube`:

``` bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod u+x minikube
sudo install minikube /usr/local/bin/

minikube version
```

### minikube usage

See also the [hello minikube tutorial](https://kubernetes.io/docs/tutorials/hello-minikube/).

On my system, the default driver was `docker`.
It was automatically selected when starting the minikube service with:

``` bash
minikube start
```

If you want to use another driver, it is possible to use:

``` bash
minikube start --driver=virtualbox
```

And then `minikube status` to see if everything goes well.

Launch the graphical dashboard in the default web browser:

``` bash
minikube dashboard
```

Then close it with `Ctrl-C`.

Optionally, stop the Minikube virtual machine (VM):

``` bash
minikube stop
```

Optionally, delete the Minikube VM:

``` bash
minikube delete
```

### Use the metrics-server addon

List the currently supported addons:

``` bash
minikube addons list
```

Enable the `metrics-server` addon:

``` bash
minikube addons enable metrics-server
```

It is now possible to get more metrics about the pods and services:

``` bash
kubectl get pod,svc -n kube-system
```

Finally, disable the addon with:

``` bash
minikube addons disable metrics-server
```

## kubectl

### kubectl installation

Installation of `kubectl`:

``` bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod u+x kubectl
sudo install kubectl /usr/local/bin/

kubectl version --client
```

### kubectl cheat sheet

* `kubectl version` - get the client (kubectl) and server (node provider) versions.
* `kubectl get` - list resources. Example: `kubectl get nodes` or `kubectl get pods` or `kubectl get deployments`
* `kubectl describe` - show detailed information about a resource. Example: `kubectl describe nodes` or `kubectl describe pods`
* `kubectl logs` - print the logs from a container in a pod. Example: `kubectl logs kubernetes-bootcamp-65d5b99f84-w7d62`
* `kubectl exec` - execute a command on a container in a pod. Example: `kubectl exec kubernetes-bootcamp-65d5b99f84-w7d62 -- whoami`

### kubectl usage

Get version of kubectl and cluster:

``` bash
kubectl version
```

Get info from cluster:

``` bash
kubectl cluster-info
```

Get info about nodes:

``` bash
kubectl get nodes
```

View the `kubectl` configuration:

``` bash
kubectl config view
```

Create a new deployment:

``` bash
kubectl create deployment <deployment-name> --image=<image>
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
```

Delete a deployment:

``` bash
kubectl delete deployment <deployment-name>
kubectl delete deployment hello-node
```

View the deployment:

``` bash
kubectl get deployments
```

View cluster events:

``` bash
kubectl get events
```

By default, the Pod is only accessible by its internal IP address within the Kubernetes cluster. To make the hello-node Container accessible from outside the Kubernetes virtual network, you have to expose the Pod as a Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/).

Expose the Pod to the public internet using the `kubectl expose` command:

``` bash
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

View the services:

``` bash
kubectl get services
```

On cloud providers that support load balancers, an external IP address would be provisioned to access the Service. On minikube, the `LoadBalancer` type makes the Service accessible through the `minikube service` command.

Make the service available with the following command:

``` bash
minikube service hello-node
```

Then see the URL of the available services:

``` text
$ minikube service --all
|-----------|------------|--------------|---------------------------|
| NAMESPACE |    NAME    | TARGET PORT  |            URL            |
|-----------|------------|--------------|---------------------------|
| default   | hello-node |         8080 | http://192.168.49.2:31108 |
| default   | kubernetes | No node port |
|-----------|------------|--------------|---------------------------|
```

The application `hello-node` is then available at URL: `http://192.168.49.2:31108`

Delete the service:

``` bash
kubectl delete service hello-node
```

Delete the deployment:

``` bash
kubectl delete deployment hello-node
```

Pods are running in an isolated, private network - so we need to proxy access to them so we can debug and interact with them. To do this, we'll use the `kubectl proxy` command.

``` bash
kubectl proxy
```

Get a shell in the pod to see what is going on:

``` bash
kubectl exec -ti $POD_NAME -- bash
```

### kubectl services management

Services can be exposed in different ways by specifying a type in the ServiceSpec:

* `ClusterIP` (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
* `NodePort` - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using `<NodeIP>:<NodePort>`. Superset of `ClusterIP`.
* `LoadBalancer` - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of `NodePort`.
* `ExternalName` - Maps the Service to the contents of the `externalName` field (e.g. `foo.bar.example.com`), by returning a `CNAME` record with its value. No proxying of any kind is set up. This type requires v1.7 or higher of kube-dns, or CoreDNS version 0.0.8 or higher.

Get the exposed services:

``` bash
kubectl get services
```

Expose using NodePort:

``` bash
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

Show which port is open (using `NodePort`):

``` bash
kubectl describe services/kubernetes-bootcamp
```

Or directly get the opened port with:

``` bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```

Then, the service is available at `$(minikube ip):$NODE_PORT` when using `minikube`.
So it is possible to access it by using:

``` bash
curl $(minikube ip):$NODE_PORT
```

### kubectl and labels

It is possible to see the labels of a deployment using:

``` bash
kubectl describe deployment
```

It is then possible to get informations for this particular label:

``` bash
kubectl get pods -l app=kubernetes-bootcamp
kubectl get services -l app=kubernetes-bootcamp
```

Set a label to a pod:

``` bash
kubectl label pods $POD_NAME version=v1
kubectl label service $SERVICE_NAME version=v1
```

### Scaling

Get the current scaling status:

``` bash
kubectl get deployments
kubectl get rs
```

Let’s scale the Deployment to 4 replicas:

``` bash
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

We can see the number of pods increased:

``` bash
kubectl get pods -o wide
```

### Rolling updates

Show images of running pods:

``` bash
kubectl describe pods | grep -e '^Name:' -e 'Image:'
```

Set a new image for pods:

``` bash
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```

Check the status of the new Pods, and view the old one terminating with the get pods command:

``` bash
kubectl get pods
```

Check the update status by using `rollout status`:

``` bash
kubectl rollout status deployments/kubernetes-bootcamp
```

If a deployment failed (check using `get pods`, and the events in `kubectl describe pods/<pod_name>`), it is possible to rollback with:

``` bash
kubectl rollout undo deployments/kubernetes-bootcamp
```

⚠️ it will only rollback from one deployment.
When launch again, the `rollout undo` command will go "back" and deploy again the latest image update.

More info here: <https://learnk8s.io/kubernetes-rollbacks>

Long story short: when deploying a new image, it creates a new `ReplicatSet`.
When rolling back, it restart pods of the old ReplicaSet.
By default, Kubernetes keeps the last 10 ReplicaSet.

Here is an example where I upgrade two times my application:

``` text
$ k get rs
NAME                             DESIRED   CURRENT   READY   AGE
kubernetes-bootcamp-5bbb6c8c4    0         0         0       13m
kubernetes-bootcamp-65d5b99f84   0         0         0       3h10m
kubernetes-bootcamp-684bd555bb   4         4         4       20m
```

I can get the revisions by using:

``` text
$ kubectl rollout history deployment/kubernetes-bootcamp
deployment.apps/kubernetes-bootcamp 
REVISION  CHANGE-CAUSE
11        <none>
12        <none>
13        <none>
```

And choose to rollback to a specific revision:

``` bash
kubectl rollout undo deployment/kubernetes-bootcamp --to-revision=1
```

It is possible to know which revision uses which image by showing the `ReplicaSet` details:

``` text
$ kubectl get replicaset -o yaml | grep -e 'revision: ' -e '- image: '
      deployment.kubernetes.io/revision: "11"
        - image: gcr.io/google-samples/kubernetes-bootcamp:v10
      deployment.kubernetes.io/revision: "13"
        - image: gcr.io/google-samples/kubernetes-bootcamp:v1
      deployment.kubernetes.io/revision: "12"
        - image: jocatalin/kubernetes-bootcamp:v2
```
