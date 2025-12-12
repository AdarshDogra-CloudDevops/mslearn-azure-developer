
# Lab 08: Create an Azure Function with Visual Studio Code

## Lab Scenario
In this lab, you will create and monitor a C# Azure Function by first building and testing it locally in Visual Studio Code and then deploying it to Azure. You will provision Azure resources using Azure Cloud Shell, deploy your function code from Visual Studio Code, execute the function in the cloud, and finally clean up deployed resources. This hands‑on experience demonstrates the end‑to‑end workflow for serverless application development.

## Lab Objectives
In this lab, you will perform:

- **Exercise 1:** Create your local Azure Functions project  
- **Exercise 2:** Run the function locally  
- **Exercise 3:** Deploy the function using Azure Cloud Shell  
- **Exercise 4:** Execute the function in Azure   

## Estimated timing: 30 minutes

# Exercise 1: Create your local function project

## Task 1: Create a new Function App project

1. Open **Visual Studio Code**.  
2. Install **C# Dev Kit** and **Azure Functions** extensions.
3. Open the Terminal and run the command below:

    ```
    choco upgrade azure-functions-core-tools
    ```

4. Press **F1** → Run **Azure Functions: Create New Project...**  
5. Select an empty folder.  
6. Provide the following values during setup:

    | Prompt | Action |
    |--|--|
    | Select language | **C#** |
    | Select .NET runtime | **.NET 8.0 Isolated** |
    | Select template | **HTTP Trigger** |
    | Function name | `HttpExample` |
    | Namespace | `My.Function` |
    | Authorization level | **Anonymous** |

7. Choose **Open in current window**.  
8. If prompted with *"Do you trust the authors?"* → Select **Yes**.

---

# Exercise 2: Run the function locally

## Task 1: Start the function runtime

Visual Studio Code integrates with Azure Functions Core tools to let you run this project on your local development computer before you publish to Azure.

1. Make sure the terminal is open in Visual Studio Code. You can open the terminal by selecting **Terminal** and then **New Terminal** in the menu bar. 

1. Press **F5** to start the function app project in the debugger. If you are prompted to choose a storage account select **Skip for now**.

    ![Screenshot of the dialog box prompting for storage account creation.](./media/01/select-storage-acct.png)

1. Output from Core Tools is displayed in the **Terminal** panel. You can see the URL endpoint of your HTTP-triggered function running locally.

    ![Screenshot of the endpoint of your HTTP-triggered function is displayed in the Terminal panel.](./media/01/run-function-local.png)

1. With Core Tools running, open the **Azure** extension. In the **Workspace** section of the extension, expand **Local Project** > **Functions**. Right-click the **HttpExample** function and select **Execute Function Now...**.

    ![Screenshot showing the location of the Execute Function Now... step.](./media/01/execute-function-local.png)

1. In **Enter request body** you see the request message body value of `{ "name": "Azure" }`. Press **Enter** to send this request message to your function. When the function executes locally and returns a response, a notification is raised in Visual Studio Code.

    ![](./media/01/F7.png)

- Select the notification bell icon to view the notification. Information about the function execution is shown in **Terminal** panel.

6. Press **Shift + F5** to stop Core Tools and disconnect the debugger.

After verifying that the function runs correctly on your local computer, it's time to use Visual Studio Code to publish the project directly to Azure.

---

# Exercise 3: Deploy and create resources using Azure Cloud Shell

All Azure‑side creation will be executed via **Cloud Shell (Bash)**.

---

## Task 1: Open Cloud Shell

1. Open Cloud Shell using the **[\>_]** button at the top of the Azure portal, and choose a **Bash** environment.  

    ![](./media/01/A01.png)

   ![](./media/01/A02.png)

   If prompted to choose storage, select **No storage account required**, choose your subscription, and select **Apply**.

   ![](./media/01/A03.png)

   > **Note**: If Cloud Shell is currently set to **PowerShell**, switch to **Bash**.

      ![](./media/01/C23.png)

## Task 2: Create Azure Resources in Cloud Shell

In this task, you will create the required Azure resources for deploying your Function App.  

1. You first define environment variables that store the names and locations for all resources used in this lab.  
These values will be reused by the commands that follow.

- Replace the names with the below values :
   - RESOURCE_GROUP="Serverless-<inject key="DeploymentID" enableCopy="false"/>"
    - LOCATION="eastus"
   - FUNCTIONAPP_NAME="myfunctionapp<inject key="DeploymentID" enableCopy="false"/>"
   - STORAGE_NAME="funcstor<inject key="DeploymentID" enableCopy="false"/>"
   - PLAN_NAME="funcplan<inject key="DeploymentID" enableCopy="false"/>"
 
    ```bash
    RESOURCE_GROUP="<your-resource-group-name>"
    LOCATION="<your-location>"
    FUNCTIONAPP_NAME="<your-functionapp-name>"
    STORAGE_NAME="<your-storage-account-name>"
    PLAN_NAME="<your-plan-name>"
    ```

2. Create a Storage Account required by the Function App to store logs and runtime metadata.

    ```bash
    az storage account create \
    --name $STORAGE_NAME \
    --location $LOCATION \
    --resource-group $RESOURCE_GROUP \
    --sku Standard_LRS
    ```
3. Create a Linux-based Function App using the .NET isolated runtime on a serverless consumption plan.

    ```bash
    az functionapp create \
    --resource-group $RESOURCE_GROUP \
    --consumption-plan-location $LOCATION \
    --runtime dotnet-isolated \
    --functions-version 4 \
    --name $FUNCTIONAPP_NAME \
    --storage-account $STORAGE_NAME \
    --os-type Linux
    ```
    ![](./media/01/F10.png)

<validaation step= "7870d4b0-9860-425e-9838-1a9025c8e736" />

# Exercise 4: Deploy the function to Azure

## Task 1: Deploy from Visual Studio Code

> **! Important:** Publishing to an existing function overwrites any previous deployments.

1. In the command palette, search for and run the command **Azure Functions: Deploy to Function App...**.

1. Select the subscription you used when creating the resources.

1. Select the function app you created. When prompted about overwriting previous deployments, select **Deploy** to deploy your function code to the new function app resource.

1. After deployment completes, select **View Output** to view the details of the deployment results. If you miss the notification, select the notification bell icon in the lower right corner to see it again.

    ![Screenshot of the View Output button.](./media/01/function-view-output.png)


---
## Troubleshooting Deployment

1. If deployment fails, navigate bak to Azure cloud shell run:

    ### Restart the Function App

    ```bash
    az functionapp restart \
    --name myfunctionapp<inject key="DeploymentID" enableCopy="false"/> \
    --resource-group Serverless-<inject key="DeploymentID" enableCopy="false"/>
    ```

    ### View live logs

    ```bash
    az functionapp log tail \
    --name myfunctionapp<inject key="DeploymentID" enableCopy="false"/> \
    --resource-group Serverless-<inject key="DeploymentID" enableCopy="false"/>
    ```

- Once all commands run successfully in Azure Cloud Shell, return to VS Code and perform Task 1 again.

# Exercise 5: Execute the function in Azure

1. Back in the **Resources** area in the side bar, expand your subscription, your new function app, and **Functions**. **Right-click** the **HttpExample** function and choose **Execute Function Now...**.

    ![Screenshot of the Execute Function Now option.](./media/01/execute-function-remote.png)

1. In **Enter request body** you see the request message body value of `{ "name": "Azure" }`. Press **Enter** to send this request message to your function.

1. When the function executes in Azure and returns a response, a notification is raised in Visual Studio Code. select the notification bell icon to view the notification.

3. Confirm the success notification as shown in the below screenshot.

    ![](./media/01/F8.png)
---


# Summary

In this lab, you successfully completed the end‑to‑end process of building and deploying an Azure Function:

- You created and configured a local Azure Function project using Visual Studio Code.  
- You ran and validated the function locally using Azure Functions Core Tools.  
- You provisioned all required Azure resources through Azure Cloud Shell.  
- You deployed your function code to Azure and verified execution in the cloud.  
- You cleaned up deployed resources to avoid unnecessary usage.

## You have successfully completed the lab.
