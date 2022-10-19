 istioctl manifest generate --set profile=demo --set meshConfig.outboundTrafficPolicy.mode=REGISTRY_ONLY  -f cluster1.yaml > cluster-1-registry-only.yaml


 kubectl apply -f cluster-1-registry-only.yaml


now you can't access external service, in fact if you run:

```
kubectl exec  -n sample -c sleep     "$(kubectl get pod  -n sample -l \
    app=sleep -o jsonpath='{.items[0].metadata.name}')"     -- curl -sS https:/www.google.com
```

you'll receive a 503 error.

In order to access the external service, you need to add a `ServiceEntry`


```
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: google
spec:
  hosts:
  - www.google.com
  ports:
  - number: 443
    name: https
    protocol: HTTPS
  resolution: DNS
  location: MESH_EXTERNAL
EOF
```

### References

- https://istio.io/latest/docs/tasks/traffic-management/egress/egress-control/

