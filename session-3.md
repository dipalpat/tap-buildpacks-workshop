# Session 3 - Stack and Buildpack Update
### Update Azure Java Buildpack to 9.14.0 from 9.13.0
Change the name to match the clusterbuildpack create in session 2 ex azure-java-ns1. This will ensure only your resource is updated.
```shell
kubectl apply -f resources/azure-java-buildpack-9.0.14.yaml
```
Alternatively, you can use kp cli to create clusterbuildpack resource. Adjust the image as per your environment. 
```shell
kp clusterbuildpack create azure-java --image europe-west1-docker.pkg.dev/tap-sandbox-dev/tapv-glorious-stinkbug/buildservice/java-azure@sha256:1b2cabe3290a15928d541e16738d4f7bf85dbd903baa500d833bb14d7e08f082  --dry-run --output yaml > azure-java-9.14.0.yaml
```
#### Review Azure Java Buildpack Status
```shell
tanzu build-service clusterbuildpack status azure-java
```
#### Check the latest build logs
```shell
tanzu build-service build logs spring-petclinic
```
