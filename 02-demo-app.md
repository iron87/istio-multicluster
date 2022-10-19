### Install the demo app


https://istio.io/latest/docs/setup/install/multicluster/verify/



### Test 

```
kubectl exec  -n sample -c sleep \
    "$(kubectl get pod  -n sample -l \
    app=sleep -o jsonpath='{.items[0].metadata.name}')" \
    -- curl -sS helloworld.sample:5000/hello
```


### Troubleshooting

https://istio.io/latest/docs/ops/diagnostic-tools/multicluster/