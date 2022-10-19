On both clusters

```
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.15/samples/helloworld/helloworld.yaml -l service=helloworld -n sample
```


### Test 

```
kubectl exec  -n sample -c sleep \
    "$(kubectl get pod  -n sample -l \
    app=sleep -o jsonpath='{.items[0].metadata.name}')" \
    -- curl -sS helloworld.sample:5000/hello
```

    https://istio.io/latest/docs/setup/install/multicluster/verify/