# Introduction to GitOps using ArgoCD with OpenShift GitOps Demo

Welcome to the Introduction to ArgoCD using OpenShift GitOps demo!

Application definitions, configurations and environments should be declarative and version controlled just as we been doing with applications code. Application deployment and lifecycle management should be `automated`, `auditable` and `easy to understand`. Hence the need for a framework such as [`GitOps`](#gitops) and tools like [`ArgoCD`](#argocd) and [`OpenShift GitOps`](#openshift-gitops)

## ArgoCD
[ArgoCD](https://argo-cd.readthedocs.io/en/stable/) is a declarative, `GitOps` continuous delivery tool for [Kubernetes](https://kubernetes.io/). 

It follows the `GitOps` pattern of using [Git](Githttps://git-scm.com) repositories as the sources of truth for defining the desired application state.

It automates the deployment of the desired application states in the specified taget environments. It can track updates to branches, tags or even pinned to a specifc version of a manifests at a Git commits. In other words, any changes to the desired state made in git can be automatically or manually applied & reflected in the specific environment.

The manifest can be specified using any of the following ways:
* [`Kustomize`](https://kustomize.io/)
* helm
* ksonnets
* jsonnets
* pain directory of YAML/json manifest
* other

## GitOps
[GitOps](https://cloud.redhat.com/learn/topics/gitops/) is a set of practices that leverage Git workflows to manage infrastructure and application configurations. By using Git repository as the single source of truth, it allows `DevOps` teams to store the entire state of the cluster configuration in a Git repository so that the trail of changes are visible and auditable.

It simplify the propagation of infrastructure and application configuration changes across multiple clusters by defining your infrastructure and applicaitons definition as `code`.
* Ensure that the cluster have similar states for configurations, monitoring or storage
* Resover or recreate cluster from a known state
* Create cluster with a known state
* Apply or revert configuration changes to multiple clusters
* Associate templated configuration with different environments.

## OpenShift GitOps

OpenShift GitOps is an OpenShift add-on which provides Argo CD and other tooling to enable teams to implement Gitops workflows for cluster configuration and application delivery. 

![gitops-ocp](docs/images/gitops-ocp.png)

## Overview

The tutorial is divide in different section:

* [Prerequisites](#prerequisites)
* [Learn about ArgoCD features](#argocd-features)
* [Learn about ArgoCD concepts](#argocd-concepts)
* [Install GitOps Operator](/docs/install-gitops-operator.md)
* [Deploying the demo](#deploying-the-demo)

## Prerequisites

* Any Openshift cluster 4.x or CodeReady Container base on OCP 4.3+
* OpenShift CLI `oc` install and connected to your cluster
* The [ArgoCd CLI](https://argo-cd.readthedocs.io/en/stable/cli_installation/) 'argocd` install
* The [Tekton CLI](https://github.com/tektoncd/cli) `tkn` install
* [Kustomize](https://kustomize.io/)

## ArgoCD Features 
* Automated deploymnet of app to a specified target environment
* Multiple config management or tooling supports
* Ability to manafe & deploy to multiple cluster
* SSO Integrations
* Multi-tenancy and RBAC integration
* `Automated configuration drift` detection ad visualisation
* Automated or manual sync to desired state
* Web UI for real-time view of activity
* CLI for aumomation and Countinuous Integration (CI)
* Audit trails and Prometheus metrics 
* Webhooks


## ArgoCD Concepts:
* `Application`: A group of Kubernetes resources. It represente a Custom Resource Definition (CRD).
* ` Application Source Type`: The tools use to build the applicaiton.
* `Target state`: The desired state as represented in a Git repository
* `Live state`: The live state of the application.
* `Sync status`: Wheter ot not the Target state or Live state matches.
* `Sync`: The process of making an applicaiton move to it target state.
* ` Sync operation status`: Wether the sync succeeded or not.
* `xRefresh`: Comare the latest code in Git with the live state. Figure out what is different.
* `Health`: The health of the application.
* `Tool`: A tool to create the manifest.

### [Application Set](https://argocd-applicationset.readthedocs.io/en/stable/)
ApplicationSet controller is a sub-project of ArgoCD which adds Application automation. Application may be templated from multiple different sources, including Git or ArgoCD own define cluster. It gives the ability to use a single manifest to target multiple cluster. It also permits a single manifest to deploy multiple application from 1+ git repository. There is 3 types of generators:
* List Generator
* Cluster Generator
* Git Generator  

### Sync Phase and Waves

`ArgoCD` executes synch operation in a number of steps. At a high level there is 3 phases:
1. `Pre-sync` phase
1. `Sync` phase
1. `Post-Sync` phase

Each phase can contain one or more `waves` assuring certain resources are healthy before other resources are sync.


## Install OpenShift GitOps

The operator can be installed using the Operator Hub inside the OpenShift Console. Follow [these instruction](docs/install-gitops-operator.md) to install the operator using the console. 


## Deploying the Demo

This demo install the `ArgoCD` operator as well as some cluster tenants to demonstrate a CI/CD pipeline workflow.
* Install a cluster ArgoCD
* Install a dev ArgoCD to manage the workspaces
* Install Tekton
    * Create 2 pipelines
* Create the elements required to deploy a part of the [coffeeshop application](https://github.com/froberge/coffeeshop-deploy).


### Steps to setup the cluster

1.  Clone the project locally.
2.  From the project, run the following Kustomize to initialize the project and create the required elements.
    ```
    until oc apply -k setup/overlays/demo
    do
      sleep 10
    done
    ```
3.  Retrive the cluster ArgoCD route
    ```
    oc get route openshift-gitops-server -n openshift-gitops -o jsonpath='{.spec.host}{"\n"}'
    ```

    :information_source: The cluster ArgoCD can also be access directly from OpenShift
    ![ocp-start-argo](docs/images/ocp-argocd-access.png)

4.  Retrieve the cluster ArgoCD password
    ```
    oc extract secret/openshift-gitops-cluster -n openshift-gitops --to=-
    ```


5.  Retrive the development ArgoCD route
    ```
    oc get route argocd-server -n coffeeshop-gitops -o jsonpath='{.spec.host}{"\n"}'
    ```

6. Retrieve the development ArgoCD password
    ```
    oc extract secret/argocd-cluster -n coffeeshop-gitops --to=-
    ```

:tada: We now have ArgoCD, Tekton and the requirer project needed to run the project. Any change can now be made from a GitHub push and will be reflected on the cluster.

## Get the application running.

The project should be in deprecated mode in ArgoCD since the application as not been build yet. We can do this by running the pipelines.

1.  From the code in [Coffee Deploy](https://github.com/froberge/coffeeshop-deploy) run the Tekton pipeline run to install the different component needed.

    * To build the UI.
      ```
      oc create -f tekton/pipelineruns/coffeeshop-run.yaml -n coffeeshop-pipeline
      ```

    * To build the backend.
      ```
      oc create -f tekton/pipelineruns/product-service-run.yaml -n coffeeshop-pipeline
      ```
1.  Populate the database
    1.  [Connect to the database](https://github.com/froberge/coffeeshop-documentation/tree/master/docs/populate-db.md)

    1.  Populate the Product Database by running the [following scripts](https://github.com/froberge/coffeeshop-documentation/blob/master/dbscripts/product-schema/createInsertProduct.sql)


1.  Connect to the UI and see what happens. The Menu should be empty.
![empty-menu](docs/images/empty-menu-ui.png)
1.  __To Fix__, we need to change the URL in the `ConfigMaps` _coffeeshop-config_.
    * First, change the URL from the OpenShift Web Console.
    _This shouldn't works since it will be drifting from the Git config. ArgoCD will reverted to what it originally was._

    * Now change the value in the repository and push the change. 
    _Argo should synch the project. Now redeploy the app and the menu should be appearing._
    ![full-menu](docs/images/full-menu-ui.png)

:warning: All the configuration changes are control by ArgoCD and need to be reflected form the repository.`Git as a single source of truth.`
  
