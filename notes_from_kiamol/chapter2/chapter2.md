# 2.1 How Kubernetes runs and manages containers

-   Kubernetes wraps the container in another virtualized environment:
    the Pod.
-   A Pod is a unit of compute, which runs on a single node in the cluster
    -   A Pod has its own virtual IP address
-   All the containers in a Pod are part of the same virtual environment, so they share same network address and communicate using localhost

```
# Some basic commands

# run a Pod with a single container; the restart flag tells Kubernetes
# to create just the Pod and no other resources:
kubectl run hello-kiamol --image=kiamol/ch02-hello-kiamol --restart=Never

# wait for the Pod to be ready:
kubectl wait --for=condition=Ready pod hello-kiamol

# list all the Pods in the cluster:
kubectl get pods

# show detailed information about the Pod:
kubectl describe pod hello-kiamol

```

-   Pods are allocated to one node when they are created
-   It is that node's responsibility to manage the pod and its containers
-   Does that by working with the container runtime using a known API called the CRI (Container Runtime Interface)

<br>

# 2.2 Running Pods with controllers

-   A controller is a kubernetes resource that manages other resources
-   works with Kubernetes API to watch the current state of the system
-   Kubernetes has many controllers, but the main one for managing Pods is the `Deployment`

```

# create a Deployment called "hello-kiamol-2", running the same web app:
kubectl create deployment hello-kiamol-2 --image=kiamol/ch02-hello-kiamol

# list all the Pods:
kubectl get pods

```

<br>

# 2.3 Defining deployments in application manifests

Example yaml file

```
# Manifests always specify the version of the Kubernetes API
# and the type of resource.
apiVersion: v1
kind: Pod

# Metadata for the resource includes the name (mandatory)
# and labels (optional).
metadata:
 name: hello-kiamol-3

# The spec is the actual specification for the resource.
# For a Pod the minimum is the container(s) to run,
# with the container name and image.
spec:
 containers:
 - name: web
 image: kiamol/ch02-hello-kiamol

```

-   TIL: Kubectl does not need a local copy of a manifest file. Can read contents from any public URL

```
# Example
kubectl apply -f https://raw.githubusercontent.com/sixeyed/kiamol/master/ch02/pod.yaml

```

<br>

# 2.5 Understanding kubernetes resource management

```
# list all running Pods:
kubectl get pods

# delete all Pods:
kubectl delete pods --all

# check again:
kubectl get pods

```
