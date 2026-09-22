# PostgreSQL StatefulSet Example

This example deploys a single-instance PostgreSQL database on Kubernetes using a `StatefulSet`. The base manifests define the PostgreSQL `Secret`, `PersistentVolumeClaims`, and `Services` required to expose the instance inside the cluster and through a `NodePort`.

The PVCs use the `synology-iscsi` `StorageClass` by default. Adjust the `StorageClass` and `Secret` before deploying to another environment.

Deploy the complete example with:

```bash
kubectl apply -k .
```

MetalLB is required only when the optional LoadBalancer Service shown below is used.

```yaml
oc create -f - <<EOF
apiVersion: v1
kind: Service
metadata:
  name: postgres-lb
  labels:
    zone: lab
spec:
  selector:
    app: postgres
  ports:
    - port: 5432
      targetPort: 5432
      protocol: TCP
  type: LoadBalancer
EOF
```
