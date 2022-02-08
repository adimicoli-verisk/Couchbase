# Couchbase
### How to increase memory size that is available for a Docker container
```text
Docker menu whale menu > Preferences
Memory: By default, Docker Desktop is set to use 2 GB runtime memory. Increase the to 6Gb RAM.
```
### Restart Minikube
```bash
minikube stop
minikube delete
minikube start --memory 4096 --cpus 2
```
### Install the Couchbase Helm Chart
https://github.com/couchbase-partners/helm-charts
```bash
helm repo add couchbase https://couchbase-partners.github.io/helm-charts/
helm repo update
helm install couchbase --values myvalues.yaml couchbase/couchbase-operator
helm install couchbase couchbase/couchbase-operator
helm uninstall couchbase
```
### Docker hub
https://hub.docker.com/_/couchbase/
#### on Macbook with Apple M1 chipset
https://hub.docker.com/r/amd64/couchbase/ 
```bash
QuickStart with Couchbase Server and Docker
Here is how to get a single node Couchbase Server cluster running on Docker containers:

Step - 1 : Run Couchbase Server docker container
on Macbook with Apple M1 chipset use 'amd64/couchbase' image instead

docker run -d --rm \
--name couchbase_db \
-p 8091-8094:8091-8094 \
-p 11210:11210 \
-v $PWD/couchbase_data:/opt/couchbase/var \
couchbase

Step - 2 : Next, visit http://localhost:8091 on the host machine to see the Web Console to start Couchbase Server setup.
```
### Best Practices
```bash
docker run -d --rm \
--ulimit nofile=40960:40960 \
--ulimit core=100000000:100000000 \
--ulimit memlock=100000000:100000000 \
--name couchbase_db \
-p 8091-8094:8091-8094 \
-p 11210:11210 \
couchbase
```
```bash
docker run -d --rm --name db -p 8091-8094:8091-8094 -p 11210:11210 amd64/couchbase:community
curl -X POST -u Administrator:qwerty \
http://localhost:8091/sampleBuckets/install \
-d '["travel-sample", "beer-sample"]'
```