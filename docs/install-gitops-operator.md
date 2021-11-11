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


#### Connecting to ArgoCD Cluster.

Give the ability to the ArgoCD service account to manage the cluster by running the following command:
```
oc adm policy add-cluster-role-to-user cluster-admin -z openshift-gitops-argocd-application-controller -n openshift-gitops
```

You now need to retrieve the password for the `admin` user with the following command: Note that it can also be found the the console, in the secrets for the openshidt-gitops project.

```
argoPass=$(oc get secret/openshift-gitops-cluster -n openshift-gitops -o jsonpath='{.data.admin\.password}' | base64 -d)
echo $argoPass
```

Extract the route with the command below or or use the options link in the OpenShiftconsole:
```
argoURL=$(oc get route openshift-gitops-server -n openshift-gitops -o jsonpath='{.spec.host}{"\n"}')
echo $argoURL
```

![Top menu](images/top-menu.png)

Select Cluster Argo CD, this should bring you to the Argo CD Cluster:

![argoCD cluster](images/argoCD-cluster.png)

Enter the credential found above:
`admin/argoPass`

![argoCD logged](images/argoCD-logged.png)
