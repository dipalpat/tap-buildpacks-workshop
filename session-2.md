# Session 2
* Create ClusterBuildpack Resource
    ```shell
    kubectl apply -f resources/azure-java-buildpack.yaml
    ```
* Create ClusterBuilder Resource
    ```shell
    kubectl apply -f resources/azure-java-builder.yaml
    ```
* Change spring-petclinic workload to use the new clusterbuilder
    ```shell
    tanzu apps workload apply spring-petclinic \
    --build-env BP_JVM_VERSION=17 \
    --type web \
    --local-path . \
    --annotation autoscaling.knative.dev/minScale=1 \
    --label app.kubernetes.io/part-of=petclinic \
    --label apps.tanzu.vmware.com/has-tests="true" \
    --param clusterBuilder=jammy-openjdk \
    --tail \
    --yes
    ```
* Create bindind secret to use Azure Application Insights
    ```shell
    kubectl apply -f resources/azure-application-insights.yaml
    ```
* Change spring-petclinic workload to use build time binding
    ```shell
    tanzu apps workload apply spring-petclinic \
    --build-env BP_JVM_VERSION=17 \
    --type web \
    --local-path . \
    --annotation autoscaling.knative.dev/minScale=1 \
    --label app.kubernetes.io/part-of=petclinic \
    --label apps.tanzu.vmware.com/has-tests="true" \
    --param clusterBuilder=jammy-openjdk \
    --param-yaml buildServiceBindings='[{"name": "azure-insights-build-bindings", "kind": "Secret"}]' \
    --tail \
    --yes
    ```
  * Review Build logs to see Application Insights buildpacks participating
* Create Secret that will hold connection information to Azure Application Insights Instance
    ```shell
    kubectl apply -f resources/azure-ai-secret.yaml
    ```
* Add runtime configuration to send Metrics/Logs to Azure Application Insights
    ```shell
    tanzu apps workload apply spring-petclinic \
    --build-env BP_JVM_VERSION=17 \
    --type web \
    --local-path . \
    --annotation autoscaling.knative.dev/minScale=1 \
    --label app.kubernetes.io/part-of=petclinic \
    --label apps.tanzu.vmware.com/has-tests="true" \
    --param clusterBuilder=jammy-openjdk \
    --param-yaml buildServiceBindings='[{"name": "azure-insights-build-bindings", "kind": "Secret"}]' \
    --service-ref service-binding-name=v1:Secret:azure-runtime-bindings \
    --tail \
    --yes
    ```
