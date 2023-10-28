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
    --tail \
    --annotation autoscaling.knative.dev/minScale=1 \
    --label app.kubernetes.io/part-of=petclinic \
    --label apps.tanzu.vmware.com/has-tests="true" \
    --yes
    ```
* Execute in terminal - build with Kaniko
    ```shell
    tanzu apps workload create petclinic-kaniko \
    --git-repo https://github.com/jlafata/spring-petclinic \
    --git-branch accelerator \
    --param dockerfile=./Dockerfile \
    --build-env BP_JVM_VERSION=17 \
    --type web \
    --annotation autoscaling.knative.dev/minScale=1 \
    --label app.kubernetes.io/part-of=petclinic \
    --label apps.tanzu.vmware.com/has-tests="true" \
    --yes
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
    tanzu bs build list
    tanzu bs build logs spring-petclinic -b 1
    ```
* On the Image Scanner step of the SupplyChain - View detected CVEs, Scan Policy, and options to download SBOMs.
  * (Optional) Setup steps for [Tanzu Insights CLI](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.6/tap/cli-plugins-insight-cli-configuration.html)