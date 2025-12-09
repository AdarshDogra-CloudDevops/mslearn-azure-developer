## Exercise 3: Deploy a container to Azure Container Apps with the Azure CLI

## Lab Scenario
In this exercise, you deploy a containerized application to Azure Container Apps using Azure CLI. You learn how to create a container app environment, deploy your container, and verify that your application is running in Azure.

## Lab Objectives
In this lab, you will perform:

* Task 1: Create an Azure Container Apps environment
* Task 2: Deploy a container app to the environment

## Estimated Timing: 15 Minutes

### Task 1: Create an Azure Container Apps environment

An environment in Azure Container Apps creates a secure boundary around a group of container apps. Container Apps deployed to the same environment are deployed in the same virtual network and write logs to the same Log Analytics workspace.

1. Run the following command to ensure you have the latest version of the Azure Container Apps extension for the CLI is installed.

    ```azurecli
    az extension add --name containerapp --upgrade
    ```

1. Create an environment with the **az containerapp env create** command. Replace **myResourceGroup** and **myLocation** with the values you used earlier. It takes a few minutes for the operation to complete.

    ```bash
    az containerapp env create \
        --name my-container-env<inject key="DeploymentID" enableCopy="false"/> \
        --resource-group ConfidentialStack-<inject key="DeploymentID" enableCopy="false"/> \
        --location <inject key="Region" enableCopy="false"/>
    ```

     ![](./media/lab5-e3-1.png)

### Task 2: Deploy a container app to the environment

After the container app environment finishes deploying, you can deploy a container image to your environment.

1. Deploy a sample app container image with the **containerapp create** command. Replace **myResourceGroup** with the value you used earlier.

    ```bash
    az containerapp create \
        --name my-container-app<inject key="DeploymentID" enableCopy="false"/> \
        --resource-group ConfidentialStack-<inject key="DeploymentID" enableCopy="false"/> \
        --environment my-container-env<inject key="DeploymentID" enableCopy="false"/> \
        --image mcr.microsoft.com/azuredocs/containerapps-helloworld:latest \
        --target-port 80 \
        --ingress 'external' \
        --query properties.configuration.ingress.fqdn
    ```

    ![](./media/lab5-e3-2.png)

    By setting **--ingress** to **external**, you make the container app available to public requests. The command returns a link to access your app.

    ```
    Container app created. Access your app at <url>
    ```

1. To verify the deployment select the URL returned by the **az containerapp create** command to verify the container app is running.

    ![](./media/lab5-e3-3.png)