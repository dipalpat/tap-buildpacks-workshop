# Session 3 - Tanzu Stack and Buildpack Update
This section relies on setup done in Session 2. 

## Update Azure Java Buildpack to 9.14.0 from 9.13.0
Change the name in ***resources/azure-java-buildpack-9.0.14.yaml*** to match the clusterbuildpack create in session 2 ex azure-java-ns1. This will ensure only your resource is updated.
```shell
kubectl apply -f resources/azure-java-buildpack-9.14.0.yaml
```
Alternatively, you can use kp cli to create clusterbuildpack resource. Adjust the image as per your environment. 
```shell
kp clusterbuildpack create azure-java --image europe-west4-docker.pkg.dev/tap-sandbox-dev/tapv-proud-corgi/buildservice/java-azure@sha256:1b2cabe3290a15928d541e16738d4f7bf85dbd903baa500d833bb14d7e08f082  --dry-run --output yaml > azure-java-9.14.0.yaml
```
### Review Azure Java Buildpack Status
```shell
tanzu build-service clusterbuildpack status azure-java-$NS
```
Change the version in ***resources/azure-java-builder.yaml*** to 9.14.0
```shell
kubectl apply -f resources/azure-java-builder.yaml
```
### Check the latest build logs
```shell
tanzu build-service build logs spring-petclinic
```
## Update Stack - Run and Build Image
Change the name in ***resources/clusterstack-tiny-0.1.66.yaml*** to match the clusterstack name created in session 2 ex tiny-jammy-ns1. This will ensure only your resource is updated.
```shell
kubectl apply -f resources/clusterstack-tiny-0.1.66.yaml
```
### Review ClusterStack Status
```shell
tanzu build-service clusterstack status tiny-jammy-$NS
```
### Check the latest build logs
```shell
tanzu build-service build logs spring-petclinic
```
