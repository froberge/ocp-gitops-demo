# Install OpenShift GitOps operator

OpenShift Gitops is provided as an add-on on top of OpenShift that can be installed via an operator that is available in the OpenShift OperatorHub.

## Steps:

First, connect to OpenShift console with a user that has Cluster administration right and make sure you are on the Administrator perspective as shown below:
![Administration Perspective](images/admin-perspective.png)

Then, go to the `Operators -> OperatorHub`. You should now see a list of available operators for OpenShift provided by Red Hat, the community and our partners.
![Operator Hub](images/operator-hub.png)

To facilitate the process, in the `Filter by keyword...`, type OpenShift GitOps to find the required operator:

![GitOps Operator](images/gitOps-operator.png)

Click on the Openshift GitOps operator to start the installation.  Leave the default setting and click install to start the installation process.

![Installation](images/install-gitops-operator.png)

Once the installation is done, you should now see the OpenShift Pipeline Operator in the Installed Operators tab, as shown below:

![Installed Operator Tab](images/installed-operator.png)

:tada: CONGRATULATIONS 
The operator is now installed on the cluster.

The ArgoCD UI is now available in the Application Menu on the top.

![Top menu](images/top-menu.png)

Select Cluster Argo CD, this should bring you to the Argo CD Cluster:
![argoCD cluster](images/argoCD-cluster.png)