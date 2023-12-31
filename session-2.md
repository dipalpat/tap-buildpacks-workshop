# Session 2 - Tanzu Build Customizations
This section relies on setup done in Session 1. 

## Review deployed buildpacks, builders and stacks
### Buildpacks
```shell
tanzu build-service clusterbuildpack list
```
```shell
tanzu build-service clusterbuildpack status java-lite-9.0.4
```
### Stacks
```shell
tanzu build-service clusterstacks list
```
```shell
tanzu build-service clusterstacks status default
```
### Builders  
```shell
tanzu build-service clusterbuilders list
```
```shell
tanzu build-service clusterbuilders status default
```
## Swap Bellsoft Liberica with Azure Java
### Create ClusterBuildpack Resource
Edit ***resources/azure-java-buildpack-9.13.0.yaml*** file and change the name to something unique ex. azure-java-ns1. This is so that we can exercise builpack and builder update for every attendee and avoid overriding resources that others create.
```shell
kubectl apply -f resources/azure-java-buildpack-9.13.0.yaml
```
* Alternatively, you can use kp cli to create clusterbuildpack resource. Adjust the image as per your environment. 
* ```shell
  kp clusterbuildpack create azure-java --image europe-west4-docker.pkg.dev/tap-sandbox-dev/tapv-proud-corgi/buildservice/java-azure@sha256:4e677d8bfa9ffc0ddc2b4ae11b718b5b2d9f64b06867088ac2be7ed6708ef80d  --dry-run --output yaml > azure-java-9.13.0.yaml
  ```
  Replace ServiceAccountRef under spec to the file before applying if you are using YAML created by kp cli
  ``````shell
  serviceAccountRef:
      name: dependencies-pull-serviceaccount
      namespace: build-service
  ``````
Check the status of clusterbuildpack
``````shell
tanzu build-service clusterbuildpack status azure-java-$NS
``````
### Create ClusterStack Resource
Edit ***resources/clusterstack-tiny-0.1.65.yaml*** file and change the name to something unique ex. tiny-jammy-ns1. This is so that we can exercise builpack and builder update for every attendee and avoid overriding resources that others create.
```shell
kubectl apply -f resources/clusterstack-tiny-0.1.65.yaml
```
* Alternatively, you can use kp cli to create clusterbuildpack resource. Adjust the image as per your environment. 
* ```shell
  TBD
  ```
* Replace ServiceAccountRef under spec in the file before applying if you are using YAML created by kp cli
* ``````shell
  serviceAccountRef:
      name: dependencies-pull-serviceaccount
      namespace: build-service
  ``````
Check the status of clusterstack
``````shell
tanzu build-service clusterstack status tiny-jammy-$NS
``````
### Create ClusterBuilder Resource
Edit ***resources/azure-java-builder.yaml*** file and change the name to something unique ex. jammy-openjdk-ns1. Change the stack name to reflect your cluster stack you created in earlier step. This is so that we can exercise builpack and builder update for every attendee and avoid overriding resources that others create.
```shell
kubectl apply -f resources/azure-java-builder.yaml
```
* ``````shell
  Add KP CLI command to generate clusterbuilder resource - TBD
  ``````
Check the status of cluster builder
``````shell
tanzu build-service clusterbuilder status jammy-openjdk-$NS
``````
### Change spring-petclinic workload to use the new clusterbuilder
Change the clusterBuilder param to match clusterbuilder created earlier.
```shell
tanzu apps workload apply spring-petclinic \
--build-env BP_JVM_VERSION=17 \
--type web \
--annotation autoscaling.knative.dev/minScale=1 \
--label app.kubernetes.io/part-of=petclinic \
--label apps.tanzu.vmware.com/has-tests="true" \
--param clusterBuilder=jammy-openjdk-$NS \
--yes
```
```shell
tanzu apps workload tail spring-petclinic --timestamp --since 1h
```
> Review the logs, build reason and check the detected buildpacks
``````shell
tanzu build-service build logs spring-petclinic
``````
## Use Azure Application Insights APM with Buildpacks Bindings feature
#### Create build time binding secret to use Azure Application Insights
Apply ***resources/azure-application-insights.yaml*** as is.
```shell
kubectl apply -f resources/azure-application-insights.yaml
```
### Change spring-petclinic workload to use build time binding
```shell
tanzu apps workload apply spring-petclinic \
--build-env BP_JVM_VERSION=17 \
--type web \
--annotation autoscaling.knative.dev/minScale=1 \
--label app.kubernetes.io/part-of=petclinic \
--label apps.tanzu.vmware.com/has-tests="true" \
--param clusterBuilder=jammy-openjdk-$NS \
--param-yaml buildServiceBindings='[{"name": "azure-insights-build-bindings", "kind": "Secret"}]' \
--yes
```
```shell
tanzu apps workload tail spring-petclinic --timestamp --since 1h
```
  > Review Build logs to see Application Insights buildpacks participating
### Create Secret that will hold connection information of Azure Application Insights Instance
Add the connection string to ***resources/azure-ai-secrets.yaml*** before applying the resource. This is runtime bindings.
```shell
kubectl apply -f resources/azure-ai-secret.yaml
```
#### Add runtime configuration to send Metrics/Logs to Azure Application Insights
```shell
tanzu apps workload apply spring-petclinic \
--build-env BP_JVM_VERSION=17 \
--type web \
--annotation autoscaling.knative.dev/minScale=1 \
--label app.kubernetes.io/part-of=petclinic \
--label apps.tanzu.vmware.com/has-tests="true" \
--param clusterBuilder=jammy-openjdk-NS^ \
--param-yaml buildServiceBindings='[{"name": "azure-insights-build-bindings", "kind": "Secret"}]' \
--service-ref service-binding-name=v1:Secret:azure-runtime-bindings \
--yes
```
```shell
tanzu apps workload tail spring-petclinic --timestamp --since 1h
```
Check the Azure Application Insights to see the metrics
