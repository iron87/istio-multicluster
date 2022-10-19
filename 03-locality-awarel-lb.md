
**These feature doesn't support failover**

https://istio.io/latest/docs/ops/configuration/traffic-management/multicluster/

istioctl manifest generate --set profile=demo --set meshConfig.serviceSettings[0].settings.clusterLocal=true --set  meshConfig.serviceSettings[0].hosts[0]="*"  -f cluster1.yaml > cluster-1-cluster-local-manifest.yaml


kubectl apply -f cluster-1-cluster-local-manifest.yaml


----


I will try this feature:
https://istiobyexample.dev/locality-load-balancing/

but first, I need to deploy the second cluster in a different zone

