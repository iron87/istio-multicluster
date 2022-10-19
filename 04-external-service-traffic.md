# External service traffic



```
 istioctl manifest generate --set profile=demo --set meshConfig.outboundTrafficPolicy.mode=REGISTRY_ONLY  -f cluster1.yaml > cluster-1-registry-only.yaml


 kubectl apply -f cluster-1-registry-only.yaml
```

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

## External TCP connection

In order to test an external TCP connection, I deployed a redis instance on an external VM and a redis app inside the mesh, then run a redis-cli command from the service inside the mesh.

So, like the HTTP example, we have to add a `ServiceEntry`

```
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: redis-vm
spec:
  hosts:
  # it will be ignored
  - redis-vm
  addresses:
  # Ip of the external redis as a CIDR block
  - 10.154.16.54/32 
  ports:
  - number: 6379
    name: tcp
    protocol: tcp
  location: MESH_EXTERNAL
EOF
```

then you can test the connection

```
kubectl exec  -n sample -c redis \
    "$(kubectl get pod  -n sample -l \
    app=redis -o jsonpath='{.items[0].metadata.name}')" \
    -- redis-cli -h 10.154.16.54 -p 6379 GET counter
```

---
### References

- https://istio.io/latest/docs/tasks/traffic-management/egress/egress-control/

