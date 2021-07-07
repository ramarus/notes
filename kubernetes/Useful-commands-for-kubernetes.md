### How to get the API server address
#### For current context:
```console
# kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}'
```
#### For any context:
```console
# kubectl config view -o jsonpath="{.clusters[?(@.name==<CLUSTER_NAME>")].cluster.server}"
```

### How to get the token value
```console
# kubectl get secret $(kubectl get serviceaccount default -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 --decode
```
OR
```console
# kubectl get secrets -o jsonpath="{.items[?(@.metadata.annotations['kubernetes\.io/service-account\.name']=='default')].data.token}" | base64 --decode
```
