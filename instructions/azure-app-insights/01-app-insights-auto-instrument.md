# Lab 03: Monitor an Application with Autoinstrumentation

## Lab Scenario
In this lab, you will learn how to monitor an application in Application Insights by configuring autoinstrumentation without modifying your application code. You will create an Azure App Service web app with Application Insights enabled, configure instrumentation at the service level, deploy a Blazor application, and then view application metrics and error data in Application Insights. This approach simplifies deployments and migrations by providing comprehensive monitoring without requiring code changes.

## Lab Objectives
In this lab, you will perform:

+ Exercise 1: Create a web app resource with Application Insights enabled  
+ Exercise 2: Configure instrumentation for the web app  
+ Exercise 3: Create a new Blazor app and deploy it to the web app resource  
+ Exercise 4: View application activity in Application Insights  

## Estimated timing: 20 minutes
# Exercise 1: Create resources in Azure

### Task 1: Create a Web App with Application Insights enabled

1. Select **+ Create a resource** located under the **Azure Services** heading on the homepage.



2. In the **Search the Marketplace** bar, enter **web app** and press **Enter**.
3. In the Web App tile, select the **Create** dropdown and then select **Web App**.

![](./media/create-web-app-tile.png)

5. On the **Basics** tab, configure the following settings:

| Setting | Action |
|--|--|
| **Subscription** | Retain the default value. |
| **Resource group** | Choose an existing resource group.  |
| **Name** | Enter a unique name `webapp-<inject key="DeploymentID" enableCopy="false"/>`.|
| Slider under **Name** | Turn it off (if visible). |
| **Publish** | Select **Code**. |
| **Runtime stack** | Select **.NET 8 (LTS)**. |
| **Operating system** | Select **Windows**. |
| **Region** | Retain the default selection or choose a region near you. |
| **Windows Plan** | Retain the default selection. |
| **Pricing plan** | Select **S1**. |

   > Note : If you're not able to see S1 pricing plan click on explore more pricing plan and choose S1

6. Navigate to the **Monitor + secure** tab and configure:

| Setting | Action |
|--|--|
| **Enable Application Insights** | Select **Yes**. |
| **Application Insights** | Select **Create new**, enter `autoinstrument-insights-<inject key="DeploymentID" enableCopy="false"/>`, and select **OK**. |
| **Workspace** | Select **create new** Enter `Workspace-<inject key="DeploymentID" enableCopy="false"/>` if the field is not already populated and locked. |

   > **Note:** If the **Enable Application Insights** is disabled use diffent regions 
       like West US, North Europe, East US, Southeast Asia.

7. Select **Review + create** → Review your configuration → Select **Create**.
8. After deployment completes, select **Go to resource**.

---

# Exercise 2: Configure instrumentation settings

### Task 1: Enable autoinstrumentation for the web app

1. In the left navigation menu, expand **Monitoring** and select **Application Insights**.
2. Locate the **Instrument your application** section and select **.NET Core**.
3. Under **Collection level**, select **Recommended**.
4. Select **Apply** and confirm the changes.
5. In the left navigation menu, select **Overview**.

---

# Exercise 3: Create and deploy a Blazor app

All steps in this exercise are performed in the Azure Cloud Shell.

### Task 1: Create the Blazor application

1. Open Cloud Shell using the **[\>_]** button at the top of the Azure portal, and choose a **Bash** environment.  
   If prompted to choose storage, select **No storage account required**, choose your subscription, and select **Apply**.

> **Note**: If Cloud Shell is currently set to **PowerShell**, switch to **Bash**.

2. Run the following commands to create a folder and move into it:
```
mkdir blazor
cd blazor
```

3. Create a new Blazor app:
```
dotnet new blazor
```

4. Build the application:
```
dotnet build
```

---

### Task 2: Publish and package the application

1. Publish the application into a **publish** directory:
```
dotnet publish -c Release -o ./publish
```

2. Create a `.zip` file of the published output:
```
cd publish
zip -r ../app.zip .
cd ..
```

---

### Task 3: Deploy the application to App Service

Replace the placeholders with your actual App Service name and resource group:

```
az webapp deploy --name YOUR-WEB-APP-NAME     --resource-group YOUR-RESOURCE-GROUP     --src-path ./app.zip
```

Once deployment is complete, open the application using the **Default domain** link in the Web App **Overview** page.

---

# Exercise 4: View metrics in Application Insights

1. Return to the Application Insights resource.
2. Review charts on the **Overview** tab:
   - Failed requests  
   - Server response time  
   - Server requests  
   - Availability  

### Generate telemetry:

1. Navigate through **Home**, **Counter**, and **Weather** pages in the application.
2. Refresh the web page multiple times to generate request and response data.
3. To generate errors, append `/failures` to the application URL.  
   (This route does not exist and will create failures.)  
   Refresh several times.

4. Return to Application Insights and wait 1–2 minutes for telemetry to appear.

5. In the left navigation menu, open **Investigate → Failures** to view detailed breakdowns.

---

# Summary
In this lab, you:

- Created a web app with Application Insights enabled  
- Configured autoinstrumentation at the service level  
- Built and deployed a Blazor application  
- Viewed telemetry, errors, and performance data in Application Insights  

## You have successfully completed the lab.
