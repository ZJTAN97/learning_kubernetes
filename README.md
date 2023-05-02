# Basic Understand of how Kubernetes work

<br>

# 1. Two core concepts of Kubernetes.

1. API, which you use to define your applications.
2. the cluster, which runs your applications.

<br>

-   Can define your applications in YAML files and send those files to the Kubernetes API.
-   Kubernetes looks at what you are asking for in the YAML and compares it to what is already running in the cluster.
-   Defining the structure of application is your job. Running and managing every thing is down to Kubernetes.
-   YAML files --> application manifests

<br>

# 2. How Kubernetes runs and manages containers

-   A container is a virtualized environment that typically runs a single application component.
-   Kubernetes wraps the container in another virtualized environment; the pod.
-   A Pod is a unit of compute, which runs on a single node in the cluster.
-   The Pod has its own virtual IP address, which is managed by Kubernetes and can communicate with other pods over that virtual network.
-   All containers in a pod are part of the same virtual environment, so they share the same network address and can communicate using localhost.

<br>

# 3. Pods in Kubernetes

-   Smallest deployment unit in Kubernetes.
-   Containers live inside a pod.
-   a pod can have multiple containers.

<br>

# 4. Replicasets in Kubernetes

-   Ensures specific number of pods are running at any point of time
