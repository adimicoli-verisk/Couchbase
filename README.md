# Couchbase

| Github Action Status                                                                            | 
|-------------------------------------------------------------------------------------------------|
| ![status](https://github.com/adimicoli-verisk/Couchbase/actions/workflows/test_0.yml/badge.svg) |
| ![status](https://github.com/adimicoli-verisk/Couchbase/actions/workflows/test_1.yml/badge.svg) |

### GitHub Actions
https://lo-victoria.com/github-actions-101-creating-your-first-workflow

https://lo-victoria.com/github-actions-101-deep-dive-into-workflow-attributes

https://lo-victoria.com/github-actions-101-actions-actions-actions

https://lo-victoria.com/github-actions-101-develop-a-cicd-workflow

### How to increase memory size that is available for a Docker container
```text
Docker menu whale menu > Preferences
By default, Docker Desktop is set to use 2 GB runtime memory. Increase the to 8Gb RAM and 6 CPUs.
```
### Restart Minikube
```bash
minikube stop
minikube delete
minikube start --memory 4096 --cpus 4
minikube start --memory 6144 --cpus 4 --nodes 3
minikube dashboard
```
### Install the Couchbase Helm Chart
https://github.com/couchbase-partners/helm-charts
```bash
helm repo add couchbase https://couchbase-partners.github.io/helm-charts/
helm repo update
helm install couchbase-operator charts/couchbase-operator/
helm status couchbase-operator
helm dependency update
helm install couchbase-monitor-stack .
helm status couchbase-monitor-stack
```
### Create Couchbase buckets
```bash
CBpassword=$(helm status couchbase-operator | grep password | awk '{print $2}')
curl -X POST -u Administrator:$CBpassword \
http://localhost:8091/sampleBuckets/install \
-d '["travel-sample", "beer-sample"]'
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
### Backup and restore Couchbase buckets
```bash
docker run -d --rm --name db-7 -p 8091-8094:8091-8094 -p 11210:11210 couchbase:community
docker run -d --rm --name db-4 -p 8191-8194:8091-8094 -p 12210:11210 couchbase:community-4.0.0
```
```bash
docker exec -it db-4 bash
cbbackup couchbase://localhost:8091 /backups -m full --single-node
mkdir -p /backups

cbbackup http://localhost:8091 -u Administrator -p qwerty /backups/beer-sample -b beer-sample
cbbackup http://localhost:8091 -u Administrator -p qwerty /backups/gamesim-sample -b gamesim-sample
cbbackup http://localhost:8091 -u Administrator -p qwerty /backups/travel-sample -b travel-sample
tar -cvf db-backups.tar /backups

docker cp db-4:db-backups.tar .
docker cp db-backups.tar db:/

# Please first create the destination / bucket before restoring.
docker exec -it db bash
tar xvf db-backups.tar
cbrestore /backups/beer-sample couchbase://localhost:8091 -u Administrator -p qwerty --bucket-source=beer-sample
cbrestore /backups/gamesim-sample couchbase://localhost:8091 -u Administrator -p qwerty --bucket-source=gamesim-sample
cbrestore /backups/travel-sample couchbase://localhost:8091 -u Administrator -p qwerty --bucket-source=travel-sample
```