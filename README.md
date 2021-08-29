# iotdb-cluster-k8s-example
Simple Example how to run IoTDB as Cluster on Kubernetes

## Startup

You need a Kubernetes cluster (e.g. Minikube, see here for details: https://minikube.sigs.k8s.io/docs/).

First, create a namespace

```
kubectl create namespace iotdb2
```

Then, install all  ressources in the cluster

```
kubectl -n iotdb2 -f configmap.yaml
kubectl -n iotdb2 -f service.yaml
kubectl -n iotdb2 -f headless-service.yaml
kubectl -n iotdb2 -f statefulset.yaml
```

This should trigger the creation of 2 pods named `cluster2-seeds-0` and `cluster2-seeds-1` which form a two node cluster.

## Testing

Forward a Port to one of the pods to your local system via 

```
kubectl port-forward -n iotdb2 pods/cluster2-seeds-0 6667:6667
```

and use the cli to connect to the cluster with default settings (localhost and port 6667).

## Further Notice

This is not starting iotdb `0.12.2` directly but a slightly patched version from the branch https://github.com/apache/iotdb/tree/experimental/0.12.2-cluster-for-k8s which is based on the release 0.12.2 branch.

**Changes**
```
Patches for rel/0.12 to make it run on k8s.
* No IP resolution at startup
* if the string "hostname" is set as "internal_ip" in iotdb-cluster.properties it will resolve the hostname at startup (FQDN) and set as internal_ip parameter
```
