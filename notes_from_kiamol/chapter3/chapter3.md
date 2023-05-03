# Connecting Pods over the network with Services

<br>

# 3.1 How Kubernetes routes network traffic

Example

```
# start up your lab environment—run Docker Desktop if it's not running—
# and switch to this chapter’s directory in your copy of the source code:
cd ch03

# create two Deployments, which each run one Pod:
kubectl apply -f sleep/sleep1.yaml -f sleep/sleep2.yaml

# wait for the Pod to be ready:
kubectl wait --for=condition=Ready pod -l app=sleep-2

# check the IP address of the second Pod:
kubectl get pod -l app=sleep-2 --output
jsonpath='{.items[0].status.podIP}'

# use that address to ping the second Pod from the first:
kubectl exec deploy/sleep-1 -- ping -c 2 $(kubectl get pod -l app=sleep-2 --output jsonpath='{.items[0].status.podIP}')

```

-   The problem of needing a permanent address for resources that can change is an old
    one—the internet solved it using DNS (the Domain Name System), mapping friendly
    names to IP addresses
-   A Kubernetes cluster has a DNS server built in, which maps Service names to IP addresses

![image](./dns-pods-example.png)

-   This type of Service is an abstraction over a Pod and its network address
-   Similar to Deployment being an abstraction over a Pod and its container.
-   Service has its own IP address which is static

Example Service Manifest

```
apiVersion: v1 # Services use the core v1 API.
kind: Service

metadata:
 name: sleep-2 # The name of a Service is used as the DNS domain name

# The specification requires a selector and a list of ports.
spec:
 selector:
 app: sleep-2 # Matches all Pods with an app label set to sleep-2.
 ports:
 - port: 80 # Listens on port 80 and sends to port 80 on the Pod

```

<br>

# 3.2 Routing traffic between pods

-   The default type of Service in Kubernetes is called ClusterIP
-   The IP address works only within the
    cluster, so ClusterIP Services are useful only for communicating between Pods.
