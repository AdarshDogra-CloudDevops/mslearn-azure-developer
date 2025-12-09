# Lab 1: Import and configure an API with Azure API Management

## Lab Scenario
In this exercise, you create an Azure API Management instance, import an OpenAPI specification backend API, configure the API settings including the web service URL and subscription requirements, and test the API operations to verify they work correctly.

## Lab Objectives
In this lab, you will perform:

- Create an Azure API Management (APIM) instance
- Import an API
- Configure backend settings
- Test the API

## Estimated Timing: 20 minutes
# Exercise 1: Create an API Management instance

In this section of the exercise, you create a resource group and an Azure Storage account. You also record the endpoint and access key for the account.

1. Use the **[>_] (1)** button to the right of the search bar at the top of the page to create a new Cloud Shell in the Azure portal, selecting a **Bash (2)** environment.

     ![](./media/A01.png)

     ![](./media/A02.png)

2.  If you are prompted to select a storage account to persist your files, select **No storage account required (1)**, select the default **subscription (2)**, and then select **Apply (3)**.

    ![](./media/A03.png)

4. Now create variables using the code below. Copy it into Notepad, update the values accordingly, and then run it in the Bash terminal:

    | Settings | Values |
    | -- | -- |
    | `myApiName` | **myApi-<inject key="DeploymentID" enableCopy="false"/>** |
    | `myLocation`|  **<inject key="Region" enableCopy="false"/>** |
    | `myEmail` | **odl_user_<inject key="DeploymentID" enableCopy="false"/>@cloudlabsai.com** |
    |`myResourceGroup` | **ApiService-<inject key="DeploymentID" enableCopy="false"/>** |

    ```bash
    myApiName=<myapiname>
    myLocation=<myLocation>
    myEmail=<myEmail>
    myResourceGroup=<myResourceGroup>
    ```
     ![](./media/A010.png)

5. Create an APIM instance:

    ```bash
    az apim create -n $myApiName \
        --location $myLocation \
        --publisher-email $myEmail  \
        --resource-group $myResourceGroup \
        --publisher-name Import-API-Exercise \
        --sku-name Consumption 
    ```
     ![](./media/A04.png)

> **Note:** The operation should complete within approximately five minutes.
---

# Exercise 2: Import a Backend API

1. Search for **API Management services** in Azure Portal and select your instance **myApi-<inject key="Deployment-ID" enableCopy="false"/>**.

     ![](./media/A005.png)

2. Once the API **myApi-<inject key="Deployment-ID" enableCopy="false"/>** page opens, under **APIs (1)** from the left panel select **APIs (2)**. And select **OpenAPI** (3)

    ![](./media/A007.png)

3. In the **Create from OpenAPI specification** tab, set mode to **Full (1)** and follow these instructions to fill out the properties:

     | Setting | Value |
     |--------|--------|
     | **OpenAPI Specification** | `https://petstore3.swagger.io/api/v3/openapi.json` **(2)** |
     | **URL scheme** | HTTPS **(3)** |

4. Select **Create (4)**.

    ![](./media/A09.png)
---

# Exercise 3: Test the API

1. Select **Test**.
2. Select **Find Pets by status**.
3. Select **Send**. Response should be **200 OK**.

![](./media/A11.png)

> **Note:** Modify **status** parameter to test different outputs.

---
# Summary

## You have completed:

- Creating APIM instance  
- Importing API  
- Configuring backend settings  
- Testing API operations  

## **You have successfully completed the lab.**
