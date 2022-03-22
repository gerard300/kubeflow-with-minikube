# kubeflow-with-minikube

::: {#page}
::: {#main .aui-page-panel}
::: {#main-header}
::: {#breadcrumb-section}
1.  [Desarrollo](index.html)
:::

[ Desarrollo : kubeflow ]{#title-text} {#title-heading .pagetitle}
======================================
:::

::: {#content .view}
::: {.page-metadata}
Created by [ Gerardo Aguirre Vivar]{.author}, last modified on Mar 20,
2022
:::

::: {#main-content .wiki-content .group}
Resources {#kubeflow-Resources}
=========

-   <https://v0-2.kubeflow.org/docs/started/getting-started-minikube/>

-   <https://github.com/kubeflow/manifests#installation>

-   

Install kubeflow with minikube {#kubeflow-Installkubeflowwithminikube}
==============================

Prerequisites {#kubeflow-Prerequisites}
-------------

::: {.confluence-information-macro .confluence-information-macro-note}
[]{.aui-icon .aui-icon-small .aui-iconfont-warning
.confluence-information-macro-icon}

::: {.confluence-information-macro-body}
-   `Kubernetes` (up to `1.21`) with a default

    [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/){.external-link}

    -   Kubeflow 1.5.0 is not compatible with version 1.22 and onwards.
        You can track the remaining work for K8s 1.22 support in
        [kubeflow/kubeflow\#6353](https://github.com/kubeflow/kubeflow/issues/6353){.external-link}

-   `kustomize` (version `3.2.0`) ([download
    link](https://github.com/kubernetes-sigs/kustomize/releases/tag/v3.2.0){.external-link})

    -   Kubeflow 1.5.0 is not compatible with the latest versions of of
        kustomize 4.x. This is due to changes in the order resources are
        sorted and printed. Please see
        [kubernetes-sigs/kustomize\#3794](https://github.com/kubernetes-sigs/kustomize/issues/3794){.external-link}
        and
        [kubeflow/manifests\#1797](https://github.com/kubeflow/manifests/issues/1797){.external-link}.
        We know this is not ideal and are working with the upstream
        kustomize team to add support for the latest versions of
        kustomize as soon as we can.

-   `kubectl`

-   Check your Architecture: `dpkg --print-architecture`
:::
:::

### Install kubectl v1.21.0 {#kubeflow-Installkubectlv1.21.0}

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"}
# Note that kubectl version must follow the same k8s version deployed on minikube
curl -LO "https://dl.k8s.io/release/v1.21.0/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
:::
:::

### Install kustomize in Linux v3.2.0 {#kubeflow-InstallkustomizeinLinuxv3.2.0}

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"}
wget https://github.com/kubernetes-sigs/kustomize/releases/download/v3.2.0/kustomize_3.2.0_linux_amd64
sudo mv kustomize_3.2.0_linux_amd64 /usr/local/bin/kustomize
sudo chmod 700 /usr/local/bin/kustomize
sudo chown root:root /usr/local/bin/kustomize

# Check 
kustomize version
```
:::
:::

### Deploy a minikube cluster v1.21.0 {#kubeflow-Deployaminikubeclusterv1.21.0}

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"}
minikube start --cpus 4 --memory 4096 --disk-size=40g -p kubeflow-test --kubernetes-version=v1.21.0

# First checks
kubectl get nodes -o wide
kubectl config get-contexts
minikube profile list
kubectl get namespace
```
:::
:::

kubeflow installation {#kubeflow-kubeflowinstallation}
---------------------

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"}
# Clone repository
git clone https://github.com/kubeflow/manifests.git

cd manifest

# Deploy kubeflow in k8s (minikube)
while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```
:::
:::

Wait for a 15 mins and then You will be able to see something like this:

[![](attachments/275152897/275546198.png){.confluence-embedded-image
.image-center}]{.confluence-embedded-file-wrapper .image-center-wrapper}

### Check if the installation was successfully {#kubeflow-Checkiftheinstallationwassuccessfully}

Perform the next commands in order to get access to the kubeflow
centrald ashboard:

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"}
minikube ip -p kubeflow-test
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80

# Go to : https://localhost:8080
```
:::
:::

After the installation {#kubeflow-Aftertheinstallation}
======================

~~Mac draft~~ {#kubeflow-Macdraft}
=============

Action {#kubeflow-Action}
------

Check your docker daemon have the next parameters setted:

[![](attachments/275152897/275283969.png){.confluence-embedded-image
.image-center}]{.confluence-embedded-file-wrapper .image-center-wrapper}

::: {.confluence-information-macro .confluence-information-macro-note}
[]{.aui-icon .aui-icon-small .aui-iconfont-warning
.confluence-information-macro-icon}

::: {.confluence-information-macro-body}
At least more than the parameter specified below
:::
:::

Installing Kubeflow using Bootstrapper {#kubeflow-InstallingKubeflowusingBootstrapper}
======================================

Prerrequisites {#kubeflow-Prerrequisites}
--------------

-   Download the YAML file\

    ::: {.code .panel .pdl style="border-width: 1px;"}
    ::: {.codeContent .panelContent .pdl}
    ``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"}
    curl -O https://raw.githubusercontent.com/kubeflow/kubeflow/v0.2-branch/bootstrap/bootstrapper.yaml
    ```
    :::
    :::

-   Delete `v1beta1` and `v1beta2` from the bootstrapper.yaml and
    replace them by just `v1`\

    [![](attachments/275152897/275382279.png){.confluence-embedded-image
    .image-center}]{.confluence-embedded-file-wrapper
    .image-center-wrapper}

Bootstraping {#kubeflow-Bootstraping}
------------

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"}
kubectl create -f bootstrapper.yaml
```
:::
:::

In case of error, perform the next commands

::: {.code .panel .pdl style="border-width: 1px;"}
::: {.codeContent .panelContent .pdl}
``` {.syntaxhighlighter-pre data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"}
# In case of error perform the next commands
kubectl delete all --all -n kubeflow-admin
kubectl delete all --all -n kubeflow
kubectl delete clusterRoleBinding kubeflow-cluster-admin
kubectl delete namespace kubeflow-admin
kubectl delete namespace kubeflow
```
:::
:::
:::

::: {.pageSection .group}
::: {.pageSectionHeader}
Attachments: {#attachments .pageSectionTitle}
------------
:::

::: {.greybox align="left"}
![](images/icons/bullet_blue.gif){width="8" height="8"}
[image-20220317-165817.png](attachments/275152897/275283969.png)
(image/png)\
![](images/icons/bullet_blue.gif){width="8" height="8"}
[image-20220317-171851.png](attachments/275152897/275382279.png)
(image/png)\
![](images/icons/bullet_blue.gif){width="8" height="8"}
[image-20220320-185219.png](attachments/275152897/275611695.png)
(image/png)\
![](images/icons/bullet_blue.gif){width="8" height="8"}
[image-20220320-185224.png](attachments/275152897/275546198.png)
(image/png)\
:::
:::
:::
:::

::: {#footer role="contentinfo"}
::: {.section .footer-body}
Document generated by Confluence on Mar 22, 2022 08:48

::: {#footer-logo}
[Atlassian](http://www.atlassian.com/)
:::
:::
:::
:::

