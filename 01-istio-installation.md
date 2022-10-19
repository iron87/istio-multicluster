## Install istioctl

```
curl -sL https://istio.io/downloadIstioctl | sh - 
```

Add the istioctl to your path with:
  export PATH=$HOME/.istioctl/bin:$PATH 
 


## Install istio 

```
export CTX_CLUSTER1=<your cluster1 context>
export CTX_CLUSTER2=<your cluster2 context>
```



```
kubectl --context="${CTX_CLUSTER1}" create namespace istio-system
```
```
kubectl --context="${CTX_CLUSTER1}" get namespace istio-system && \
  kubectl --context="${CTX_CLUSTER1}" label namespace istio-system topology.istio.io/network=network1
```


```
cat <<EOF > cluster1.yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  values:
    global:
      meshID: mesh1
      multiCluster:
        clusterName: cluster1
      network: network1
EOF
```



```
istioctl install --context="${CTX_CLUSTER1}" -f cluster1.yaml --set profile=demo
```
in alternative, if you want to customize the k8s manifest: 
```
istioctl manifest generate --set profile=demo -f cluster1.yaml > cluster-1-manifest.yaml
```
then, after manifest customization
```
kubectl apply --context="${CTX_CLUSTER1}" -f cluster-1-manifest.yaml
kubectl --context="${CTX_CLUSTER1}" apply -n istio-system -f https://raw.githubusercontent.com/istio/istio/release-1.15/samples/multicluster/expose-services.yaml
```

Then on cluster 2

```
kubectl --context="${CTX_CLUSTER2}" create namespace istio-system

kubectl --context="${CTX_CLUSTER2}" get namespace istio-system && \
  kubectl --context="${CTX_CLUSTER2}" label namespace istio-system topology.istio.io/network=network2
```

```
cat <<EOF > cluster2.yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  values:
    global:
      meshID: mesh1
      multiCluster:
        clusterName: cluster2
      network: network2
EOF
```

```
istioctl install --context="${CTX_CLUSTER2}" -f cluster2.yaml --set profile=demo
```
in alternative, if you want to customize the k8s manifest: 
```
istioctl manifest generate --set profile=demo -f cluster2.yaml > cluster-2-manifest.yaml
```
then, after manifest customization
```
kubectl apply --context="${CTX_CLUSTER2}" -f cluster-2-manifest.yaml
kubectl --context="${CTX_CLUSTER2}" apply -n istio-system -f https://raw.githubusercontent.com/istio/istio/release-1.15/samples/multicluster/expose-services.yaml
```

## Enable endpoint discovery

Install a remote secret in cluster2 that provides access to cluster1’s API server.
```
istioctl x create-remote-secret \
  --context="${CTX_CLUSTER1}" \
  --name=cluster1 | \
  kubectl apply -f - --context="${CTX_CLUSTER2}"
```
Install a remote secret in cluster1 that provides access to cluster2’s API server.

```
istioctl x create-remote-secret \
  --context="${CTX_CLUSTER2}" \
  --name=cluster2 | \
  kubectl apply -f - --context="${CTX_CLUSTER1}"
```

## Generate clusters certificates

https://istio.io/latest/docs/tasks/security/cert-management/plugin-ca-cert/