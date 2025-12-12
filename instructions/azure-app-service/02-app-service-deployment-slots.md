# Lab 04: Module 2: Swap Deployment Slots in Azure App Service

## Lab Scenario

In this exercise, you deploy a static HTML website to Azure App Service, create a staging deployment slot, make changes to the code and deploy them to the staging slot, and then swap the staging and production slots to promote the changes to production. You learn how to use deployment slots for safe application updates and blue-green deployments.

## Lab Objectives
In this lab, you will perform:

- Download and deploy the sample app to Azure App Service.
- Create a staging deployment slot.
- Make a change to the sample app and deploy it to the staging slot.
- Swap the staging and default production slots to move the changes to the production slot.

## Estimated timing: 30 minutes

# Exercise 1: Download and deploy the sample app

In this section you download the sample app, set variables to simplify commands, create an Azure App Service resource, and deploy a static HTML website using Azure CLI.

## Task 1: Prepare Cloud Shell and Clone Repository

1. Open **Cloud Shell**, choose **Bash**, select **No storage account required (1)**, choose the available **Subscription (2)**, and then click **Apply (3)** to continue.

    ![](./media/02/A001.png)

    ![](./media/02/A02.png)

    ![](./media/02/A03.png)

2. Switch to **Classic version** in Cloud Shell.  

    ![](./media/02/E9.png)

5. Run the following command to clone the sample app:

    ```bash
    git clone https://github.com/Azure-Samples/html-docs-hello-world.git
    ```

## Task 2: Set Variables

1. Run the following commands to set the required variables for your resource group and web app, and copy the generated app name into Notepad as it will be needed in later tasks.

    ```bash
    resourceGroup=rg-mywebapp
    appName=mywebapp$RANDOM
    echo $appName
    ```

    ![](./media/02/D5.png)

## Task 3: Deploy to App Service Using `az webapp up`

1. Run the following commands to navigate into the project directory and deploy the web app to Azure.


    ```bash
    cd html-docs-hello-world
    az webapp up -g $resourceGroup -n $appName --sku P0V3 --html
    ```

    > After the deployment completes, follow the steps below to access your web app:


2. Search for your **mywebapp** by entering its name in the Azure portal search bar.  

   ![](./media/02/D06.png)

3. Open the application by selecting the **Default domain** link on the Overview page. 

   ![](./media/02/D90.png)

4. The web app URL will appear similar to the example shown below:  

   ![](./media/02/D100.png)

<validation step="00206e92-d4d2-4fef-a336-68f6f444ab9a" />

# Exercise 2: Deploy Updated Code to a Deployment Slot

## Task 1: Create the Staging Slot

1. Run the following command to create a **staging** deployment slot for your web app:

    ```bash
    az webapp deployment slot create -n $appName -g $resourceGroup --slot staging
    ```

2. After the slot is created, verify it in the portal:

    - In the Azure portal, open your **Web App**, then on the Overview page select the **Deployment (1)** dropdown and choose **Deployment slots (2)** to view the newly created **staging slot (3)**.

        ![](./media/02/D70.png)

## Task 2: Modify Code and Deploy to Staging

1. Navigate back to Cloud Shell and open the HTML file by running:

    ```bash
    code index.html
    ```

2. Update the heading text:

    Replace:

    ```
    Azure App Service - Sample Static HTML Site
    ```

    With:

    ```
    Azure App Service Staging Slot
    ```

3. Save your changes (**Ctrl + S**) and exit the editor (**Ctrl + Q**).

4. Create a ZIP package containing the updated site:

    ```bash
    zip -r stagingcode.zip .
    ```

5. Deploy the ZIP package to the **staging** slot:

    ```bash
    az webapp deploy -g $resourceGroup -n $appName --src-path ./stagingcode.zip --slot staging
    ```

6. Open the staging slot:

- In the Azure portal, select **Deployment slots**, choose **staging**, and open the **Default domain** link to view the updated application.

    ![](./media/02/D011.png)

7. The web app URL will appear similar to the example shown below:  

    ![](./media/02/D012.png)

---

# Exercise 3: Swap the Staging and Production Slots

1. Select your **Web App**, then in the left panel open the **Deployment (1)** dropdown, go to **Deployment slots (2)**, and select **Swap (3)**. 

    ![](./media/02/D07.png)

2. Set **Source** to **staging (1)**.  
3. Set **Target** to **production (2)**.  
4. Select **Start Swap (3)** to begin the process.  
5. Monitor the swap progress in the **Notifications** panel.  

    ![](./media/02/D08.png)

6. Open the production site and verify that the updated heading appears (refresh the page if needed).

    ![](./media/02/D100.png)

---

# Summary

In this lab, you:

- Deployed a static HTML website to Azure App Service.
- Created and deployed changes to a staging slot.
- Performed a slot swap to safely promote changes to production.

## You have successfully completed this lab.