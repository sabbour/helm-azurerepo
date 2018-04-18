# Assumptions

This guide assumes:

- You have access to a Kubernetes cluster (Minikube or on Azure)
- You have the [Azure CLI installed](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- You have [authenticated](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) the Azure CLI

# Installing Helm

Follow the instructions on the Helm install document to [install Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) on your system. Make sure you have a valid Kubernetes configuraton (either Minikube oran actual Kubernetes cluster).

# Creating your first chart

1. Create a local directory to to store your development charts. Most likely this should be connected to your project's source control folder

    ```sh
    mkdir helmcharts
    cd helmcharts
    ```

1. Then run the command below to create the chart scaffolding

    ```sh
    helm create myfirstchart
    ```
    you should now have a directory structure like the below:
    ![Chart directory structure](docs/chart-dir.png)

# Creating a repository on Azure Storage

There a a couple of ways that you can use to store the packaged Helm charts. In this guide, you'll store them onto an Azure Storage account.

The steps below are loosely based on the guide posted by [Michal Cwienczek](https://cwienczek.com/setting-up-secure-helm-chart-repository-on-azure-blob-storage/).

1. Create a Resource Group

    ```sh
    az group create --name helmgroup --location "westeurope"
    ```

1. Create an Azure Storage account

    ```sh
    export AZURE_STORAGE_ACCOUNT=<unique storage account name>

    az storage account create --resource-group helmgroup --name $AZURE_STORAGE_ACCOUNT --sku Standard_LRS
    ```

1. Get the Storage Account Name and Key
    ```sh
    export AZURE_STORAGE_KEY=$(az storage account keys list --resource-group helmgroup --account-name $AZURE_STORAGE_ACCOUNT | grep -m 1 value | awk -F'"' '{print $4}')
    ```

1. Create a blob container to hold your packages

    ```sh
    az storage container create --name helm
    ```

# Packaging your chart

    ```sh
    export APP_VERSION=<your build number>
    helm package --app-version $APP_VERSION
    ```

# Uploading your chart

Now that you have your chart packaged, it is time to upload it to Azure Storage.

```sh
```

