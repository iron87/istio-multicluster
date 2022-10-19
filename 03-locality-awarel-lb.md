

# Locality load balancing

## Destination rule


```
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: hello-outlier-detection
  namespace: sample
spec:
  host: helloworld.sample.svc.cluster.local
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 1000
      http:
        http2MaxRequests: 1000
        maxRequestsPerConnection: 10
    loadBalancer:
      simple: ROUND_ROBIN
      localityLbSetting:
        enabled: true
    outlierDetection:
      consecutiveErrors: 7
      interval: 30s
      baseEjectionTime: 30s
```


## Cluster local


**These feature doesn't support failover**

https://istio.io/latest/docs/ops/configuration/traffic-management/multicluster/

```
istioctl manifest generate --set profile=demo --set meshConfig.serviceSettings[0].settings.clusterLocal=true --set  meshConfig.serviceSettings[0].hosts[0]="*"  -f cluster1.yaml > cluster-1-cluster-local-manifest.yaml


kubectl apply -f cluster-1-cluster-local-manifest.yaml
```

---
### References
- https://istio.io/latest/docs/tasks/traffic-management/locality-load-balancing/
- https://istio.io/latest/docs/reference/config/networking/destination-rule/
- https://istiobyexample.dev/locality-load-balancing/


