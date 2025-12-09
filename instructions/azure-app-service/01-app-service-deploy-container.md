# Lab 04: Module 1: Deploy a Containerized App to Azure App Service

## Lab Scenario

In this exercise, you create an Azure App Service web app configured to run a containerized application by specifying a container image from Microsoft Container Registry. You learn how to configure container settings, deploy the app, and verify that the containerized application is running successfully in Azure App Service.

## Lab Objectives

In this lab, you will perform:

- Create an Azure App Service resource and deploy a containerized app
- View the results

## Estimated timing: 15 minutes

# Exercise 1: Create a web app resource

## Task 1: Create the Web App

1. In your browser navigate to the Azure portal https://portal.azure.com; sign in with your Azure credentials if prompted.
2. Select the **+ Create a resource** located in the **Azure Services** heading near the top of the homepage. 
3. In the **Search the Marketplace** search bar, enter *web app* and press **Enter** to start searching.
4. In the Web App tile, select the **Create** drop-down and then select **Web App**.

    ![Screenshot of the Web App tile.](./media/01/create-web-app-tile.png)

5. Fill out the **Basics** tab with the information in the following table:

    | Setting | Action |
    |--------|--------|
    | **Subscription** | Retain the default value. |
    | **Resource group** | Select **Create new**, enter `rg-WebApp`, and then select OK. You may also select an existing resource group. |
    | **Name** | Enter a unique name, for example **your-initials-containerwebapp**. Replace *your-initials* with your initials or another value. The name must be globally unique. |
    | **Slider under Name** | Select the slider to turn it off (if visible). |
    | **Publish** | Select **Container**. |
    | **Operating System** | Ensure **Linux** is selected. |
    | **Region** | Retain the default selection, or choose a region near you. |
    | **Linux Plan** | Retain the default value. |
    | **Pricing plan** | Select the drop-down and choose **Free F1**. |

---

## Task 2: Configure the Container Settings

Navigate to the **Container** tab and enter the following details:

| Setting | Action |
|--------|--------|
| **Sidecar support** | Off |
| **Image Source** | Other container registries |
| **Access Type** | Public |
| **Registry server URL** | `mcr.microsoft.com/k8se` |
| **Image and Tag** | `quickstart:latest` |
| **Startup Command** | Leave blank |

---

## Task 3: Review and Create

1. Select the **Review + create** tab.  
2. Review your selections.  
3. Select **Create** to deploy the web app.  
4. Wait until deployment completes and select **Go to resource**.

---

## Task 4: View the Web App

1. In the **Essentials** section of the App Service overview page, select the link next to **Default domain**.  
2. A new browser tab will open showing your deployed containerized app.

> **Note:** It may take a few minutes for the container to fully load.

---

# Exercise Review

In this exercise, you created a Linux-based Azure App Service Web App, configured it to use a public container image from Microsoft Container Registry, deployed the application, and verified it is running successfully in a browser.

---

# Summary

In this lab, you completed the following tasks:

- Created an Azure App Service configured for container deployment  
- Configured the container settings using an image from Microsoft Container Registry  
- Deployed and validated the containerized application  