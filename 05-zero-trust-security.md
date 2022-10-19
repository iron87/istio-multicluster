## Zero Trust

Through Istio Authorization Policy enables access control on workloads in the mesh. 
In order to allow traffic only between `sleep` and `helloworl` apps we'll define two authorization policies.
The first one denies all the traffic inside the `sample` namespace:

```
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
 name: allow-nothing
 namespace: sample
spec:
  {}
```

The second one enable only the traffic from the app `sleep` to a specific api exposed by the `helloworld` app

```
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: allow-hello-world
  namespace: sample
spec:
  selector:
    matchLabels:
      app: sleep
  action: ALLOW
  rules:
  - from:
    - source:
        namespaces: ["sample"]
    to:
    - operation:
        methods: ["GET"]
        paths: ["/hello"]

```

### References

- https://istio.io/latest/docs/reference/config/security/authorization-policy/


