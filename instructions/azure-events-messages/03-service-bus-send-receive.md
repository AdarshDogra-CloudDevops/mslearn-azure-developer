## Exercise 3: Send and receive messages from Azure Service Bus

## Lab Scenario

In this exercise, you create and configure Azure Service Bus resources, then build a .NET app to send and receive messages using the **Azure.Messaging.ServiceBus** SDK. You learn how to provision a Service Bus namespace and queue, assign permissions, and interact with messages programmatically. 

## Lab Objectives
In this lab, you will perform:

* Task 1: Create Azure Event Hubs resources
* Task 2: Create an Azure Service Bus namespace and queue
* Task 3: Assign a role to your Microsoft Entra user name
* Task 4: Create a .NET console app to send and receive messages
* Task 5: Add the starter code for the project
* Task 6: Add code to send messages to queue
* Task 7: Add code to process messages in the queue
* Task 8: Sign into Azure and run the app

## Estimated Timing: 30 Minutes

### Task 1: Create Azure Event Hubs resources

In this section of the exercise you create the needed resources in Azure with the Azure CLI.

1. In your browser navigate to the Azure portal 

1. On the Azure portal homepage, click the **\[>\_] Cloud Shell (1)** button located to the right of the **Copilot** tab at the top. This opens a new Cloud Shell session. In the **Welcome to Azure Cloud Shell** window, choose **Bash (2)**.

    ![](./media/lab7-12---1.png)

    ![](./media/lab7-12---2.png)

1. In the **Getting started** window, ensure **No storage account required (1)** is selected. From the **Subscription** drop-down, choose **Default subscription (2)**, then click **Apply (3)**.
   
    ![](./media/lab7-12---3.png)

    >**Note:** If your Cloud Shell environment is already configured, you can skip this step and proceed to the next.

1. In the cloud shell toolbar, in the **Settings** menu, select **Go to Classic version** (this is required to use the code editor).

     ![](./media/lab7-12-1.1.png)

1. Many of the commands require unique names and use the same parameters. Creating some variables will reduce the changes needed to the commands that create resources. Run the following commands to create the needed variables. 

    ```
    resourceGroup=PubSubEvents-<inject key="DeploymentID" enableCopy="false"/>
    location=<inject key="Region" enableCopy="false"/>
    namespaceName=svcbusns<inject key="DeploymentID" enableCopy="false"/>
    ```

1. You will need the name assigned to the namespace later in this exercise. Run the following command and record output.

    ```
    echo $namespaceName
    ```

    ![](./media/lab7-e3-1.png)

    >**Note:** Copy the namespace: **svcbusns<inject key="DeploymentID" enableCopy="false"/>** value into a Notepad file. You will use it in a later task.

### Task 2: Create an Azure Service Bus namespace and queue

1. Create a Service Bus messaging namespace. The following command creates a namespace using the variable you created earlier. The operation takes a few minutes to complete.

    ```bash
    az servicebus namespace create \
        --resource-group $resourceGroup \
        --name $namespaceName \
        --location $location
    ```

     ![](./media/lab7-e3-2.png)

1. Now that a namespace is created, you need to create a queue to hold the messages. Run the following command to create a queue named **myqueue**.

    ```bash
    az servicebus queue create --resource-group $resourceGroup \
        --namespace-name $namespaceName \
        --name myqueue<inject key="DeploymentID" enableCopy="false"/>
    ```

     ![](./media/lab7-e3-3.png)

### Task 3: Assign a role to your Microsoft Entra user name

To allow your app to send and receive messages, assign your Microsoft Entra user to the **Azure Service Bus Data Owner** role at the Service Bus namespace level. This gives your user account permission to manage and access queues and topics using Azure RBAC. Perform the following steps in the cloud shell.

1. Run the following command to retrieve the **userPrincipalName** from your account. This represents who the role will be assigned to.

    ```
    userPrincipal=$(az rest --method GET --url https://graph.microsoft.com/v1.0/me \
        --headers 'Content-Type=application/json' \
        --query userPrincipalName --output tsv)
    ```

1. Run the following command to retrieve the resource ID of the Service Bus namespace. The resource ID sets the scope for the role assignment to a specific namespace.

    ```
    resourceID=$(az servicebus namespace show --name $namespaceName \
        --resource-group $resourceGroup \
        --query id --output tsv)
    ```
1. Run the following command to create and assign the **Azure Service Bus Data Owner** role.

    ```
    az role assignment create --assignee $userPrincipal \
        --role "Azure Service Bus Data Owner" \
        --scope $resourceID
    ```
    
    ![](./media/lab7-e3-4.png)

### Task 4: Create a .NET console app to send and receive messages

Now that the needed resources are deployed to Azure the next step is to set up the console application. The following steps are performed in the cloud shell.

>**Tip:** Resize the cloud shell to display more information, and code, by dragging the top border. You can also use the minimize and maximize buttons to switch between the cloud shell and the main portal interface.

1. Run the following commands to create a directory to contain the project and change into the project directory.

    ```
    mkdir svcbus
    cd svcbus
    ```

1. Create the .NET console application.

    ```
    dotnet new console
    ```

1. Run the following commands to add the **Azure.Messaging.ServiceBus** and **Azure.Identity** packages to the project.

    ```
    dotnet add package Azure.Messaging.ServiceBus
    dotnet add package Azure.Identity
    ```

### Task 5: Add the starter code for the project

1. Run the following command in the cloud shell to begin editing the application.

    ```
    code Program.cs
    ```

1. Replace any existing contents with the following code. Be sure to review the comments in the code, and replace **<YOUR-NAMESPACE>** with the Service Bus namespace you recorded earlier.

    ```csharp
    using Azure.Messaging.ServiceBus;
    using Azure.Identity;
    using System.Timers;
    
    
    // TODO: Replace <YOUR-NAMESPACE> with your Service Bus namespace
    string svcbusNameSpace = "<YOUR-NAMESPACE>.servicebus.windows.net";
    string queueName = "myqueue<inject key="DeploymentID" enableCopy="false"/>";
    
    
    // ADD CODE TO CREATE A SERVICE BUS CLIENT
    
    
    
    // ADD CODE TO SEND MESSAGES TO THE QUEUE
    
    
    
    // ADD CODE TO PROCESS MESSAGES FROM THE QUEUE
    
    
    
    // Dispose client after use
    await client.DisposeAsync();
    ```

     ![](./media/lab7-e3-6.png)

1. Press **ctrl+s** to save your changes.

### Task 6: Add code to send messages to queue

Now it's time to add code to create the Service Bus client and send a batch of messages to the queue.

1. Locate the **// ADD CODE TO CREATE A SERVICE BUS CLIENT** comment and add the following code directly after the comment. Be sure to review the code and comments.

    ```csharp
    // Create a DefaultAzureCredentialOptions object to configure the DefaultAzureCredential
    DefaultAzureCredentialOptions options = new()
    {
        ExcludeEnvironmentCredential = true,
        ExcludeManagedIdentityCredential = true
    };
    
    // Create a Service Bus client using the namespace and DefaultAzureCredential
    // The DefaultAzureCredential will use the Azure CLI credentials, so ensure you are logged in
    ServiceBusClient client = new(svcbusNameSpace, new DefaultAzureCredential(options));
    ```

     ![](./media/lab7-e3-7.png)

1. Locate the **// ADD CODE TO SEND MESSAGES TO THE QUEUE** comment and add the following code directly after the comment. Be sure to review the code and comments.

    ```csharp
    // Create a sender for the specified queue
    ServiceBusSender sender = client.CreateSender(queueName);
    
    // create a batch 
    using ServiceBusMessageBatch messageBatch = await sender.CreateMessageBatchAsync();
    
    // number of messages to be sent to the queue
    const int numOfMessages = 3;
    
    for (int i = 1; i <= numOfMessages; i++)
    {
        // try adding a message to the batch
        if (!messageBatch.TryAddMessage(new ServiceBusMessage($"Message {i}")))
        {
            // if it is too large for the batch
            throw new Exception($"The message {i} is too large to fit in the batch.");
        }
    }
    
    try
    {
        // Use the producer client to send the batch of messages to the Service Bus queue
        await sender.SendMessagesAsync(messageBatch);
        Console.WriteLine($"A batch of {numOfMessages} messages has been published to the queue.");
    }
    finally
    {
        // Calling DisposeAsync on client types is required to ensure that network
        // resources and other unmanaged objects are properly cleaned up.
        await sender.DisposeAsync();
    }
    
    Console.WriteLine("Press any key to continue");
    Console.ReadKey();
    ```

     ![](./media/lab7-e3-8.png)

1. Press **ctrl+s** to save the file, then continue with the exercise.

### Task 7: Add code to process messages in the queue

1. Locate the **// ADD CODE TO PROCESS MESSAGES FROM THE QUEUE** comment and add the following code directly after the comment. Be sure to review the code and comments.

    ```csharp
    // Create a processor that we can use to process the messages in the queue
    ServiceBusProcessor processor = client.CreateProcessor(queueName, new ServiceBusProcessorOptions());
    
    // Idle timeout in milliseconds, the idle timer will stop the processor if there are no more 
    // messages in the queue to process
    const int idleTimeoutMs = 3000;
    System.Timers.Timer idleTimer = new(idleTimeoutMs);
    idleTimer.Elapsed += async (s, e) =>
    {
        Console.WriteLine($"No messages received for {idleTimeoutMs / 1000} seconds. Stopping processor...");
        await processor.StopProcessingAsync();
    };
    
    try
    {
        // add handler to process messages
        processor.ProcessMessageAsync += MessageHandler;
    
        // add handler to process any errors
        processor.ProcessErrorAsync += ErrorHandler;
    
        // start processing 
        idleTimer.Start();
        await processor.StartProcessingAsync();
    
        Console.WriteLine($"Processor started. Will stop after {idleTimeoutMs / 1000} seconds of inactivity.");
        // Wait for the processor to stop
        while (processor.IsProcessing)
        {
            await Task.Delay(500);
        }
        idleTimer.Stop();
        Console.WriteLine("Stopped receiving messages");
    }
    finally
    {
        // Dispose processor after use
        await processor.DisposeAsync();
    }
    
    // handle received messages
    async Task MessageHandler(ProcessMessageEventArgs args)
    {
        string body = args.Message.Body.ToString();
        Console.WriteLine($"Received: {body}");
    
        // Reset the idle timer on each message
        idleTimer.Stop();
        idleTimer.Start();
    
        // complete the message. message is deleted from the queue. 
        await args.CompleteMessageAsync(args.Message);
    }
    
    // handle any errors when receiving messages
    Task ErrorHandler(ProcessErrorEventArgs args)
    {
        Console.WriteLine(args.Exception.ToString());
        return Task.CompletedTask;
    }
    ```

     ![](./media/lab7-e3-9.png)

1. Press **ctrl+s** to save the file, then **ctrl+q** to exit the editor.

### Task 8: Sign into Azure and run the app

1. In the cloud shell command-line pane, enter the following command to sign into Azure.

    ```
    az login
    ```

    **<font color="red">You must sign into Azure - even though the cloud shell session is already authenticated.</font>**

    > **Note**: In most scenarios, just using *az login* will be sufficient. However, if you have subscriptions in multiple tenants, you may need to specify the tenant by using the *--tenant* parameter. See [Sign into Azure interactively using Azure CLI](https://learn.microsoft.com/cli/azure/authenticate-azure-cli-interactively) for details.

1. After running the az login command, select the **authentication link** shown in the Cloud Shell output and **copy the displayed code**.

1. On the **Enter code to allow access** page, paste the copied code into the field and select **Next** to complete authentication.

     ![](./media/lab7-12-14.png)

1. When prompted to **Pick an account**, select your ODL_User account to proceed

     ![](./media/lab7-12-15.png)

1. On the **Are you trying to sign in to Microsoft Azure CLI?** page, select **Continue** to authorize the sign-in request.

     ![](./media/lab7-12-16.png)

1. Back in Cloud Shell, when the subscription selection appears, type **1** and press **Enter** to continue.

     ![](./media/lab7-12-17.png)

1. Run the following command to start the console app. The app will pause a various stages and prompt you to press a key to continue. This gives you an opportunity to view the messages in the Azure portal.

    ```
    dotnet run
    ```

1. In the Azure portal, navigate **PubSubEvents-<inject key="DeploymentID" enableCopy="false"/>** resource group and and select the **svcbusns<inject key="DeploymentID" enableCopy="false"/>** Service Bus namespace.

     ![](./media/lab7-e3-10.png)

1. On **svcbusns<inject key="DeploymentID" enableCopy="false"/>** page, select **myqueue<inject key="DeploymentID" enableCopy="false"/>** at the bottom of the **Overview** window.

     ![](./media/lab7-e3-11.png)

1. On **myqueue<inject key="DeploymentID" enableCopy="false"/>** page, select **Service Bus Explorer (1)** in the left navigation pane.

1. Select **Peek from start (2)** and the three messages should appear after a few seconds.

      ![](./media/lab7-e3-12.png)

1. Navigate back to cloud shell, press any key to continue and the application will process the three messages. 
 
     ![](./media/lab7-e3-13.png)

1. Return to the portal after the application has completed processing the messages. Select **Peek from start** again and notice there are no messages in the queue.

     ![](./media/lab7-e3-14.png)
