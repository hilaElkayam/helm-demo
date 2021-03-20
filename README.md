# dummy-app

## Deploy multiple version dynamically with Helm

Very simple python Flask application that has 2 deployment environments:

- **dev** - deploy will deploy one pod
- **prod** - deploy will deploy two pod


## Build
Set environment variables:

- `BUILD_NUMBER`, the default is 1
- `REGISTRY_URL`, the default is localhost:5000
Run build.sh to build and push docker images
(Make sure you are logged in to your registry)

```
export BUILD_NUMBER=1
export REGISTRY_URL=localhost:5000
./build.sh
```

## Deployment


### Deploy to dev
Run:
```
cd helm-chart
helm install -f dummy-app/values-dev.yaml  helm-app ./dummy-app/
OR
helm upgrade --install --cleanup-on-fail  -f dummy-app/values-dev.yaml helm-app ./dummy-app
```

Port Forward:
```
kubectl port-forward service/dummy-app 5001:5000 -n <UsedNamespace>
```

Call your service:
```
http://localhost:5001
```

### Deploy to prod

Run:
```
cd helm-chart
helm upgrade --install --cleanup-on-fail  -f dummy-app/values-prod.yaml helm-app ./dummy-app
```

Port Forward:
```
kubectl port-forward service/dummy-app 5001:5000 -n <UsedNamespace>
```

Call your service:
```
http://localhost:5001
```

### upgrade
check history:
```
helm history helm-appp
```
Upgrade version and set replica count to 2:
```
helm upgrade -f dummy-app/values-dev.yaml  helm-app  ./dummy-app/ --set replicaCount=2
```
check history again:
```
helm history helm-appp
```