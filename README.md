# Introduction to ArgoCD using OpenShift GitOps Demo

Welcome to the Introduction to ArgoCD using OpenShift Pipelines demo!


[ArgoCD](https://argo-cd.readthedocs.io/en/stable/) is a declarative, [GitOps](https://www.redhat.com/en/topics/devops/what-is-gitops) continuous delivery tool for [Kubernetes](https://kubernetes.io/). 

### What is GitOps
`GitOps` is a set of practices to manage infrastructure and application configurations using [Git](Githttps://git-scm.com), an open source version control system. GitOps works by using Git as a single source of truth for declarative infrastructure and applications.

## Why [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)?

Application definitions, configurations and environments should be declarative and version controlled just as we been doing with applicatins code.  Application deployment and lifecycle management should be `automated`, `auditable` and `easy to ubderstand`. 

[ArgoCD](https://argo-cd.readthedocs.io/en/stable/) follow the GitOps pattern of using git repositories as the sources of truth for defining the desired application state. The manifest can be specified using any of the following ways:
* [`Kustomize`](https://kustomize.io/)
* helm
* ksonnets
* jsonnets
* pain directory of YAML/json manifest
* other

 Any change to the desired state made in git can be automatically or manually applied & reflected in the specific environment.

#### Features 
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


#### Concepts:

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

#### Application Set
ApplicationSet controller is a sub-project of ArgoCD whih adds Application automation. Application may be templated from multiple different sources, including Git or ArgoCD own define cluster. It gives the ability to use a single manifest to target multiple cluster. It also permits a single manigest to deploy multiple application from 1+ git repository. There is 2 types of generators:
* List Generator
* Cluster Generator
* Git Generator  

#### Sync Phase and Waves

`ArgoCD` executes synch operation in a number of steps. At a high level there is 3 phases:
1. `Pre-sync` phase
1. `Sync` phase
1. `Post-Sync` phase

Each phase can contain one or more `waves` assuring ertain resourcs are healthy before other resources are sync.

## OpenShift GitOps

OpenShift GitOps is an OpenShift add-on which provides ARgo CD and other tooling to enable teams to implement Gitops workflows for cluster configuration and application delivery. 

![gitops-ocp](docs/images/gitops-ocp.png)


The operator can be installed using the Operator Hub inside the OpenShift Console. Follow [these instruction](/docs/install-gitops-operator.md) to install the operator using the console.