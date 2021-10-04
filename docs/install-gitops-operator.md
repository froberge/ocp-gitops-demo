# Install OpenShift Pipeline operator

OpenShift Pipelines is provided as an add-on on top of OpenShift that can be installed via an operator that is available in the OpenShift OperatorHub.

## Steps:

First, connect to OpenShift console with a user that has Cluster administration right and make sure you are on the Administrator perspective as shown below:
![Administration Perspective](images/admin-perspective.png)

Then, go to the `Operators -> OperatorHub`. You should now see a list of available operators for OpenShift provided by Red Hat, the community and our partners.
![Operator Hub](images/operator-hub.png)

To facilitate the process, in the `Filter by keyword...`, type OpenShift Pipeline to find the required operator:

![Pipeline Operator](images/pipeline-operator.png)

Click on the Openshift Pipeline operator to start the installation.  Leave the default setting and click install to start the installation process.

![Installation](images/install-pipeline.png)

Once the installation is done, you should now see the OpenShift Pipeline Operator in the Installed Operators tab, as shown below:

![Installed Operator Tab](images/install-operator-pipeline.png)

:tada: CONGRATULATIONS 
The operator is now installed on the cluster.

The Pipeline option should now be available in the left menu, as shown here:

![Left menu](images/left-menu-pipeline.png)