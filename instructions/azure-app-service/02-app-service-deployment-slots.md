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

1. Navigate to the Azure portal: https://portal.azure.com  
2. Select the **[\>_]** Cloud Shell icon → choose **Bash**.  
3. If asked to create storage: select **No storage account required → Apply**.  
4. From the **Settings** menu in Cloud Shell, select **Go to Classic version** (required for the code editor).  
5. Run the following command to clone the sample app:

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## Task 2: Set Variables

```bash
resourceGroup=rg-mywebapp
appName=mywebapp$RANDOM
echo $appName
```

## Task 3: Deploy to App Service Using `az webapp up`

```bash
cd html-docs-hello-world
az webapp up -g $resourceGroup -n $appName --sku P0V3 --html
```

After deployment completes:

1. Search for the Web App using its name in the portal.
2. Open the app via the **Default domain** link.

---

# Exercise 2: Deploy Updated Code to a Deployment Slot

## Task 1: Create the Staging Slot

```bash
az webapp deployment slot create -n $appName -g $resourceGroup --slot staging
```

View the newly created slot:

- Portal → Web App → **Deployment slots**

## Task 2: Modify Code and Deploy to Staging

1. Open the HTML file:

```bash
code index.html
```

2. Change:

`Azure App Service - Sample Static HTML Site`  
to  
`Azure App Service Staging Slot`

3. Save (**Ctrl+S**) and exit (**Ctrl+Q**).

4. Create a ZIP package:

```bash
zip -r stagingcode.zip .
```

5. Deploy to staging:

```bash
az webapp deploy -g $resourceGroup -n $appName --src-path ./stagingcode.zip --slot staging
```

6. Open the staging slot:

Portal → **Deployment slots** → Select **staging** → Open **Default domain** link.

---

# Exercise 3: Swap the Staging and Production Slots

1. In the Azure portal, select **Swap** from the toolbar.  
2. Source = **staging**  
3. Target = **production**  
4. Select **Start Swap**  
5. Track progress via the Notifications panel.  
6. Open the production site and verify the updated heading. Refresh if needed.

---

# Summary

In this lab, you:

- Deployed a static HTML website to Azure App Service.
- Created and deployed changes to a staging slot.
- Performed a slot swap to safely promote changes to production.

