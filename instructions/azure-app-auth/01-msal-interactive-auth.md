# Lab 2: Azure authentication and authorization

## Lab Scenario

In this exercise, you register an application in Microsoft Entra ID and build a .NET console application that uses MSAL.NET to authenticate a user interactively. You'll configure scopes, prompt for user consent, and observe how MSAL caches tokens for future runs.

## Lab Objectives
In this lab, you will perform:

- Task 1: Register a new application
- Task 2: Create a .NET console app to acquire a token
- Task 3: Configure the console application
- Task 4: Add the starter code for the project
- Task 5: Run the application

## Estimated Timing: 20 Minutes

### Exercise 1: Implement interactive authentication with MSAL.NET

### Task 1: Register a new application

In this task, you will create a new app registration in Microsoft Entra ID and record the IDs needed for authentication.

1. In the lab VM, click on the **Azure Portal icon** as shown below:

    ![](./media/lab2-12-0.png)

    - On the **Sign in to Microsoft Azure** tab, you will see the login screen. Enter your credentials:
      
        * **Email/Username:** <inject key="AzureAdUserEmail"></inject>
    
    - Next, provide your password:

        * **Temporary Access Pass:** <inject key="AzureAdUserPassword"></inject>

1. In the portal, search for **App registrations (1)** and select **App registrations (2)**. 

     ![](./media/lab2-12-1.png)

1. Select **+ New registration**, and when the **Register an application** page appears, enter your application's registration information:

    | Field | Value |
    |--|--|
    | **Name** | Enter `myMsalApplication` **(1)** |
    | **Supported account types** | Select **Accounts in this organizational directory only (2)** |
    | **Redirect URI (optional)** | Select **Public client/native (mobile & desktop) (3)** and enter `http://localhost` **(4)** in the box to the right. |

     ![](./media/lab2-12-2.png)

     ![](./media/lab2-12-3.png)

1. Select **Register (5)**. Microsoft Entra ID assigns a unique application (client) ID to your app, and you're taken to your application's **Overview** page. 

1. In the **Essentials** section of the **Overview** page record the **Application (client) ID (1)** and the **Directory (tenant) ID (2)**. The information is needed for the application.

    ![](./media/lab2-12-4.png)

> **Congratulations** on completing the task! Now, it's time to validate it.
<validation step="0b912b02-c883-4643-9b6f-c7f200323136" />
 
### Task 2: Create a .NET console app to acquire a token

In this task, you will set up the .NET console project and prepare the development environment for building the authentication application.

1. In Lab VM, open **File Explorer**, navigate to the **Downloads** folder, and create a new folder named **authapp** for the project.

     ![](./media/lab2-12-5.png)

1. In Lab VM type **Visual Studio Code (1)** in the search bar and select **Visual Studio Code (2)** from the results to open.

     ![](./media/lab2-12-6.png)

1. In Visual Studio Code, select **File (1)** and choose **Open Folder (2)**

     ![](./media/lab2-12-7.png)

1. In the Open Folder window, navigate to the **Downloads (1)** directory, select the **authapp (2)** folder, and then choose **Select Folder (3)** to open it in Visual Studio Code.

     ![](./media/lab2-12-8.png)

     > **Note:** When prompted with a security message asking **"Do you trust the authors of the files in this folder?"**, select **Yes, I trust the authors** to allow Visual Studio Code to fully load the project.

1. In Visual Studio Code, open the **Extensions (1)** view, search for **C# Dev Kit (2)**, select it **C# Dev Kit (3)** from the results, and choose **Install (4)**.

     ![](./media/vs-extension.png)

1. In Visual Studio Code, on the top menu, select **View (1) > Terminal (2)** to open a new terminal window.

     ![](./media/lab2-12-10.png)

1. Run the following command in the VS Code terminal to create the .NET console application.

    ```
    dotnet new console
    ```

1. Run the following commands to add the **Microsoft.Identity.Client** and **dotenv.net** packages to the project.

    ```
    dotnet add package Microsoft.Identity.Client
    dotnet add package dotenv.net
    ```

### Task 3: Configure the console application

In this task, you will create and configure the .env file to store the application settings required for authentication. 

1. Select **New file...** and create a file named **.env** in the project folder.

     ![](./media/lab2-12--11.png)

1. Open the **.env (1)** file and add the following code. Replace **YOUR_CLIENT_ID**, and **YOUR_TENANT_ID** with the values you recorded earlier **(2)**.

    ```
    CLIENT_ID="YOUR_CLIENT_ID"
    TENANT_ID="YOUR_TENANT_ID"
    ```

    ![](./media/lab2-12-17.png)

1. Press **ctrl+s** to save your changes.

### Task 4: Add the starter code for the project

In this task, you will add the starter code to the console application and prepare the structure needed to implement MSAL.NET authentication.

1. Open the **Program.cs (1)** file and replace any existing contents with the following code **(2)**. Be sure to review the comments in the code.

    ```csharp
    using Microsoft.Identity.Client;
    using dotenv.net;
    
    // Load environment variables from .env file
    DotEnv.Load();
    var envVars = DotEnv.Read();
    
    // Retrieve Azure AD Application ID and tenant ID from environment variables
    string _clientId = envVars["CLIENT_ID"];
    string _tenantId = envVars["TENANT_ID"];
    
    // ADD CODE TO DEFINE SCOPES AND CREATE CLIENT 
    
    
    
    // ADD CODE TO ACQUIRE AN ACCESS TOKEN
    
    
    ```

     ![](./media/lab2-12-18.png)

1. Press **ctrl+s** to save your changes.

### Add code to complete the application

1. Locate the **// ADD CODE TO DEFINE SCOPES AND CREATE CLIENT** comment and add the following code directly after the comment. Be sure to review the comments in the code.

    ```csharp
    // Define the scopes required for authentication
    string[] _scopes = { "User.Read" };
    
    // Build the MSAL public client application with authority and redirect URI
    var app = PublicClientApplicationBuilder.Create(_clientId)
        .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
        .WithDefaultRedirectUri()
        .Build();
    ```

    ![](./media/lab2-12-19.png)

1. Locate the **// ADD CODE TO ACQUIRE AN ACCESS TOKEN** comment and add the following code directly after the comment. Be sure to review the comments in the code.

    ```csharp
    // Attempt to acquire an access token silently or interactively
    AuthenticationResult result;
    try
    {
        // Try to acquire token silently from cache for the first available account
        var accounts = await app.GetAccountsAsync();
        result = await app.AcquireTokenSilent(_scopes, accounts.FirstOrDefault())
                    .ExecuteAsync();
    }
    catch (MsalUiRequiredException)
    {
        // If silent token acquisition fails, prompt the user interactively
        result = await app.AcquireTokenInteractive(_scopes)
                    .ExecuteAsync();
    }
    
    // Output the acquired access token to the console
    Console.WriteLine($"Access Token:\n{result.AccessToken}");
    ```

    ![](./media/lab2-12-19.1.png)

1. Press **ctrl+s** to save the file

### Task 5: Run the application

In this task, you will run the console application to authenticate interactively and verify that the access token is retrieved successfully.

1. Start the application by running the following command:

    ```
    dotnet run
    ```

1. The app opens the default browser and displays the **Pick an account** prompt. Select your **ODL_User** account to continue with authentication.

    ![](./media/lab2-12-20.png)

1. If this is the first time you've authenticated to the registered app you receive a **Permissions requested** notification asking you to approve the app to sign you in and read your profile, and maintain access to data you have given it access to. Select **Accept**.

    ![](./media/lab2-12-21.png)

1. You should see the results similar to the example below in the console.

    ```
    Access Token:
    eyJ0eXAiOiJKV1QiLCJub25jZSI6IlZF.........
    ```

     ![](./media/lab2-12-22.png)

1. Start the application a second time and notice you no longer receive the **Permissions requested** notification. The permission you granted earlier was cached.

## Summary

In this lab, you:

- Registered an application in Microsoft Entra ID

- Created and configured a .NET console project in Visual Studio Code

- Added authentication settings and environment variables

- Implemented MSAL.NET logic to acquire an access token

- Ran the application and completed an interactive authentication flow

## You have successfully completed the lab. Click on Next >>

![](./media/next.png)
