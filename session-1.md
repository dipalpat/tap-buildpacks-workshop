# Session 1
* Access Accelerators Tanzu Developer Portal
* Search for petclinic
* Download the project and unzip
* Open in IDE to see the project structure
* Execute in terminal - build with Cloud Native Buildpacks
    ```shell
    tanzu apps workload apply spring-petclinic \
    --build-env BP_JVM_VERSION=17 \
    --type web \
    --local-path . \
    --annotation autoscaling.knative.dev/minScale=1 \
    --label app.kubernetes.io/part-of=petclinic \
    --label apps.tanzu.vmware.com/has-tests="true" \
    --tail \
    --yes
    ```
    ```shell
    tanzu apps workload tail spring-petclinic --timestamp --since 1h
    ```
    ```shell
    tanzu apps workload list
    ```
    ```shell
    tanzu apps workload get spring-petclinic
    ```
* Execute in terminal - build with Kaniko
    ```shell
    tanzu apps workload create spring-petclinic-kaniko \
    --build-env BP_JVM_VERSION=17 \
    --type web \
    --local-path . \
    --annotation autoscaling.knative.dev/minScale=1 \
    --label app.kubernetes.io/part-of=petclinic \
    --label apps.tanzu.vmware.com/has-tests="true" \
    --param dockerfile=./Dockerfile \
    --tail \
    --yes
   ```
    ```shell
    tanzu apps workload tail spring-petclinic-kaniko --timestamp --since 1h
    ```
* View Image CR - Note latestImage and confirm with image in container in next step
    ```shell
    kubectl get img spring-petclinic -o yaml
    ```
* View Knative Configuration
    ```shell
    kubectl get config spring-petclinic -o yaml
    ```
* View build log
    ```shell
    tanzu build-service build list
    tanzu build-service build logs spring-petclinic -b 1
    ```
* On the Image Scanner step of the SupplyChain - View detected CVEs, Scan Policy, and options to download SBOMs.
  * (Optional) Setup steps for [Tanzu Insights CLI](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.6/tap/cli-plugins-insight-cli-configuration.html)