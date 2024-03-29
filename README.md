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

This sample is provided "as is" and is not supported. Likewise, no commitments are made as to its longevity or maintenance. To discuss this sample with other users, please visit the Azure DevOps Services section of the Microsoft Developer Community: https://developercommunity.visualstudio.com/spaces/21/index.html.
