# Couchbase

### How to increase memory size that is available for a Docker container
Docker menu whale menu > Preferences

Memory: By default, Docker Desktop is set to use 2 GB runtime memory. Increase the to 6Gb RAM.

```bash
minikube stop
minikube delete
minikube start --memory 4096 --cpus 2
```

```bash
helm repo add couchbase https://couchbase-partners.github.io/helm-charts/
helm repo update
helm install couchbase --values myvalues.yaml couchbase/couchbase-operator
helm install couchbase couchbase/couchbase-operator
helm uninstall couchbase
```

