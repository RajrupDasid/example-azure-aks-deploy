# Bitbucket Pipelines Pipe Sample: Azure AKS Deploy

This sample uses the Bitbucket Azure AKS Deploy pipeline to deploy the Azure Voting App to an [Azure Kubernetes Service](https://docs.microsoft.com/en-us/azure/aks/) (AKS) cluster. The application interface has been built using Python / Flask. The data component is using Redis.

## Prerequisites

You will need to configure required Azure resources before running the pipe. The easiest way to do it is by using the Azure cli. You can either [install the Azure cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) on your local machine, or you can use the [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) provided by the Azure Portal in a browser.

### Service principal

You will need a service principal with sufficient access to create an AKS service, or update an existing AKS service. To create a service principal using the Azure CLI, execute the following command in a bash shell:

```sh
az ad sp create-for-rbac --name MyAKSServicePrincipal
```

Refer to the following documentation for more detail:

* [Service principals with Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/kubernetes-service-principal)
* [Create an Azure service principal with Azure CLI](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli)

### AKS Instance

Using the service principal credentials obtained in the previous step, you can use the following commands to create an AKS instance in a bash shell:

```bash
az login --service-principal --username ${AZURE_APP_ID}  --password ${AZURE_PASSWORD} --tenant ${AZURE_TENANT_ID}

az group create --name ${AZURE_RESOURCE_GROUP} --location australiaeast

az aks create \
--resource-group ${AZURE_RESOURCE_GROUP} \
--name ${AZURE_AKS_NAME} \
--node-count 1 \
--enable-addons monitoring \
--service-principal ${AZURE_APP_ID} \
--client-secret ${AZURE_PASSWORD} \
--generate-ssh-keys
```

To walk through a quick deployment of this application, see the AKS [quick start](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough).

To walk through a complete experience where this code is packaged into container images, uploaded to Azure Container Registry, and then run in and AKS cluster, see the [AKS tutorials](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-prepare-app).

## Support

If you’d like help with this pipe, or you have an issue or feature request, please contact us:

**Option 1** - Azure portal - Goto: https://portal.azure.com

- Go to Help+Support (at the left navigation)
- Click “+New support request”
- Choose Issue Type “Technical”
- Choose Services “All Services”
- Under “Developer Tools” choose “Azure DevOps Services”
- Problem Type, choose “Pipelines”

**Option 2** - Azure DevOps - Goto: https://azure.microsoft.com/en-us/support/create-ticket/

- Choose Azure DevOps Support ticket tile
- Choose the Appropriate “Create an incident” button
- The screen will be pre populated with “Developer Tools” “Azure DevOps”.
- Choose Pipelines as the category and follow the wizard.


If you’re reporting an issue, please include:

- the version of the pipe
- relevant logs and error messages
- steps to reproduce
