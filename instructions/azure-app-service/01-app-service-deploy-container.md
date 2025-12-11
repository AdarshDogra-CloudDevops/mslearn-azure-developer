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

1. Select the **+ Create a resource** located in the **Azure Services** heading near the top of the homepage. 

    ![](./media/01/C1.png)

3. In the **Search the Marketplace** search bar, enter **web app (1)** and press **Enter** to start searching.
4. In the Web App tile, select the **Create (2)** drop-down and then select **Web App (3)**.

    ![Screenshot of the Web App tile.](./media/01/C2.png)

5. Fill out the **Basics** tab with the information in the following table:

    | Setting | Action |
    |--------|--------|
    | **Subscription** | Retain the default value. |
    | **Resource group** |  **ManagedPlatform-<inject key="DeploymentID" enableCopy="false"/>** |
    | **Name** | Enter a unique name **containerwebapp-<inject key="DeploymentID" enableCopy="false"/>** |
    | **Slider under Name** | Select the slider to turn it off (if visible). |
    | **Publish** | Select **Container**. |
    | **Operating System** | Ensure **Linux** is selected. |
    | **Region** | Retain the default selection, or choose a region near you. |
    | **Linux Plan** | Retain the default value. |
    | **Pricing plan** | Select the drop-down and choose **Free F1**. |

    ![](./media/01/D1.png)

6. Navigate to the **Container** tab, enter the required details, and then select **Review + create (6)**.

    | Setting | Action |
    |--------|--------|
    | **Sidecar support** | Off (1)|
    | **Image Source** | Other container registries (2)|
    | **Access Type** | Public (3)|
    | **Registry server URL** | `mcr.microsoft.com/k8se` (4)|
    | **Image and Tag** | `quickstart:latest` (5) |
    | **Startup Command** | Leave blank |

    ![](./media/01/D2.png)

1. Verify your selections, and then select **Create** to deploy the web app.

    ![](./media/01/D3.png)

2. Wait until deployment completes and select **Go to resource**.

    ![](./media/01/dep01.png)

---

## Task 3: View the Web App

1. In the App Service overview page, select the link next to **Default domain**.  

    ![](./media/01/D09.png)

2. A new browser tab will open showing your deployed containerized app.

    ![](./media/01/D100.png)

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