# Lab 3 - Detect Language

In this lab we are going to integrate language detection ability of cognitive services into our bot.

## Lab 3.0 : Creating a LUIS APP

1. Navigate to [https://www.luis.ai](https://www.luis.ai). We will create a new LUIS app to support our bot.

1. Sign in using the azure credentials provided.You can get the credentials from the **Environment Details** page.

1. On the **My Apps** dashboard, select your **Subscription** and **Authoring resource** from the dropdown menu.

   ![](./1.png)

1. To create a new app, click on **+ New app for conversation**, from the dropdown select **New app for conversation**

1. On the **Create new app** page, type a name, description and select Done. Close the "How to create an effective LUIS app" dialog.

   ![](./2.png)

1. From the Luisapp dashboard, select the Luis app which we created in the previous lab and click on Manage in the top toolbar.

1. Select Settings from the left hand side menu and copy the App ID value to notepad.

   ![](./3.png)

1. From Azure portal, navigate to resource group and select the cognitive service resource that starts with **luisbot**

   ![](./4.png)

1. Select Keys and Endpoint from the left hand side menu which is under **Resource Management** and copy the values of **Key1** and **Endpoint** into notepad.

   ![](./5.png)

## Lab 3.1: Retrieve your Cognitive Services url and keys

1. Open the Azure Portal https://portal.azure.com

2. Navigate to your resource group, select the cognitive services resource named **cogsmoderator**.

3. Under **RESOURCE MANAGEMENT**, select the **Keys and Endpoint** tab and record the Endpoint and the key 1 for the cognitive services resource.

   ![](./6.png)

## Lab 3.2: Add language support to your bot

1. Please open the **PictureBot** solution from **C:\AllFiles\AI-100-Design-Implement-Azure-AISol-master\Lab8-Detect_Language\code\Finished\PictureBot.sln**

      >**Note:** please make sure we are selecting the solution from the finished folder

2. Right-click the project and select **Manage Nuget Packages**

3. Select **Browse**

4. Search for **Microsoft.Azure.CognitiveServices.Language.TextAnalytics**, select it then select **Install**, then select **I Accept**

5. Open the **Startup.cs** file, Check the following using statements:

```csharp
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime;
```

6. Check the following code to the **ConfigureServices** method:

```csharp
services.AddSingleton(sp =>
{
    string cogsBaseUrl = Configuration.GetSection("cogsBaseUrl")?.Value;
    string cogsKey = Configuration.GetSection("cogsKey")?.Value;

    var credentials = new ApiKeyServiceClientCredentials(cogsKey);
    TextAnalyticsClient client = new TextAnalyticsClient(credentials)
    {
        Endpoint = cogsBaseUrl
    };

    return client;
});
```

7. Open the **PictureBot.cs** file, check the following using statements:

```csharp
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
```

8. check the following class variable:

```csharp
private TextAnalyticsClient _textAnalyticsClient;
```

9. Check whether the constructor includes the new TextAnalyticsClient:

```csharp
public PictureBot(PictureBotAccessors accessors, ILoggerFactory loggerFactory,LuisRecognizer recognizer, TextAnalyticsClient analyticsClient)
```

10. Inside the constructor, check the class variable is initialized:

```csharp
_textAnalyticsClient = analyticsClient;
```

11. Navigate to the **OnTurnAsync** method and find the following line of code:

```csharp
var utterance = turnContext.Activity.Text;
var state = await _accessors.PictureState.GetAsync(turnContext, () => new PictureState());
state.UtteranceList.Add(utterance);
await _accessors.ConversationState.SaveChangesAsync(turnContext);
```

12. Check the following line of code is present after it

```csharp
//Check the language
var result = _textAnalyticsClient.DetectLanguage(turnContext.Activity.Text, "us");

switch (result.DetectedLanguages[0].Name)
{
    case "English":
        break;
    default:
        //throw error
        await turnContext.SendActivityAsync($"I'm sorry, I can only understand English. [{result.DetectedLanguages[0].Name}]");
        return;
        break;
}
```

13. Open the **appsettings.json** file and ensure that your cognitive services settings and LUIS app settings are entered:

```csharp
"cogsBaseUrl": "",
"cogsKey" :  "",
"luisAppId": "",
"luisAppKey": "",
"luisEndPoint": ""
```

`Note: For cogsBaseURL and cogsKey you will get in your azure environment and use the values of LUIS app you copied earlier to notepad`

14. Also, add following value that you collect in previous Labs.

```
  "MicrosoftAppId": "YourAppID",
  "MicrosoftAppPassword": "YourAppIDKey",
  "BlobStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=XXXXX;AccountKey=XXXXX;EndpointSuffix=core.windows.net",
  "BlobStorageContainer": "chatlog"

```

15. Press **F5** to start your bot

16. Using the Bot Emulator, send in a few phrases and see what happens:

- Como Estes?
- Bon Jour!
- Привет
- Hello
