# Lab 1: Implementing the LUIS model

This hands-on lab guides you through creating a model to enhance the Natural Language Processing capabilities of your applications, using Microsoft's Language Understanding Intelligent Service (LUIS).

## Introduction

In this lab, you will build, train and publish a LUIS model to help your bot (which will be created in a future lab) communicate effectively with human users.

> **Note** In this lab, we will only be creating the LUIS model that you will use in a future lab to build a more intelligent bot.

The LUIS functionality and functionality has already been covered in the workshop; for a refresher on LUIS, [read more](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/Home).

Now that we know what LUIS is, we'll want to plan our LUIS app. We already created a basic bot ("PictureBot") that responds to messages containing certain text. We will need to create intents that trigger the different actions that our bot can do, and create entities that require different actions. For example, an intent for our PictureBot may be "OrderPic" and it triggers the bot to provide an appropriate response.

For example, in the case of Search (not implemented here), our PictureBot intent may be "SearchPic" and it triggers Azure Cognitive Search service to look for photos, which requires a "facet" entity to know what to search for.  You can see more examples for planning your app [here](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/plan-your-app).

Once we've thought out our app, we are ready to [build and train it](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-get-started-create-app).

As a review, these are the steps you will generally take when creating LUIS applications:

  1. [Add intents](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/add-intents)
  2. [Add utterances](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/add-example-utterances)
  3. [Add entities](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/add-entities)
  4. [Improve performance using phrase lists](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/add-features) and [patterns](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-model-intent-pattern)
  5. [Train and test](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/train-test)
  6. [Review endpoint utterances](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/label-suggested-utterances)
  7. [Publish](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/publishapp)

## Lab 1.0: Creating the LUIS service in the portal (optional)

Creating a LUIS service in the portal is optional, as LUIS provides you with a "starter key" that you can use for the labs. However, if you want to see how to create a free or paid service in the portal, you can follow the steps below.  

> **Note** If you ran the pre-req ARM Template, you will already have a cognitive services resource that included the Language Understanding APIs.

1. From your environment, open a browser and navigate to azure portal -> https://portal.azure.com

1. Sign-in using the Azure credentials provided in the environment details tab. Please use ![](./pics/1_1.png) to copy the value.

   ![](./pics/1.png)

1. Select **Create a resource**

   ![](./pics/2.png)

1. Enter **Language Understanding** in the search box and choose **Language Understanding**

   ![](./pics/3.png)

1. Select **Create**

1. Select the available subscription and resource group with name like **ai-100-deploymentID**.

1. For the name, type **luisbot**deploymentid or **luisbot**uniqueID.

1. Under Authoring Resource, for authoring location select location as WEST US.

1. For the Authoring pricing tier, select **Free F0** 

1. Under Prediction Resource, select any other location like EAST US, WEST US 2 etc.

1. For the prediction pricing tier, select **Free F0**

1. Select **Create**

**Note** The Luis AI web site does not allow you to control or publish your Azure based cognitive services resources.  You will need to call the APIs in order to train and publish them.

## Lab 1.1: Adding intelligence to your applications with LUIS

Let's look at how we can use LUIS to add some natural language capabilities. LUIS allows you to map natural language utterances (words/phrases/sentences the user might say when talking to the bot) to intents (tasks or actions the user wants to perform). For our application, we might have several intents: finding pictures, sharing pictures, and ordering prints of pictures, for example. We can give a few example utterances as ways to ask for each of these things, and LUIS will map additional new utterances to each intent based on what it has learned.

> **Warning**: Though Azure services use IE as the default browser, we do not recommend it for LUIS. Please use the browser in your lab environment like microsoft edge.

1. Open a new tab in the same session where the azure portal is signed-in and navigate to [https://www.luis.ai](https://www.luis.ai). We will create a new LUIS app to support our bot.

1. Click on **Sign In** in the top right-corner. It will automatically sign in with same azure credentials you provided before.

1. The following pop-up will appear and select the provided subscription and click on **Select or create an authoring resource**.

   ![](./pics/ai-lab6-new1.png)

1. A pop-window will appear - **Choose an authoring resource**, select the available subscription and for LUIS authoring resource, select the **luisbotdeploymentID-Authoring** and click on **Done**.

   ![](./pics/ai-lab6-new2.png)

1. From the main page, select the **+ New app** button.

   ![](./pics/5.png)

1. For Name provide **PictureBotLuisModel**, for culture select **English** and select **Done**.

   ![](./pics/6.png)

1. Close any pop-ups if appears, In the top navigation, select the **BUILD** link.  Notice there is one intent called "None".  Random utterances that don't map to any of your intents may be mapped to "None".

   ![](./pics/7.png)

We want our bot to be able to do the following things:

- Search/find pictures
- Share pictures on social media
- Order prints of pictures
- Greet the user (although this can also be done other ways, as we will see later)

Let's create intents for the user requesting each of these.  

1. Select the Intents and then Select the **+ Create** button.

   ![](./pics/8.png)

1. Name the first intent **Greetings** and select **Done**. 

   ![](./pics/9.png)

1. Give several examples of things the user might say when greeting the bot, pressing "Enter" after each examples entered.

   ![](./pics/11.png)

Let's see how to create an entity.  When the user requests to search the pictures, they may specify what they are looking for.  Let's capture that in an entity.

1. Select on **Entities** in the left-hand column and then select **+ Create**. 

   ![](./pics/12.png)

1. Give it an entity name **facet**

1. For the entity type select **Machine learned**.  

1. Select **Create**.

   ![](./pics/13.png)

1. Select **Intents** in the left-hand sidebar and then click the **Create new intent** button.  

1. Give it an intent name of **SearchPic** and then click **Done**.

Just as we did for Greetings, let's add some sample utterances (words/phrases/sentences the user might say when talking to the bot).  People might search for pictures in many ways.  Feel free to use some of the utterances below, and add your own wording for how you would ask a bot to search for pictures.

- Find outdoor pics
- Are there pictures of a train?
- Find pictures of food.
- Search for photos of kids playing
- Show me beach pics
- Find dog photos
- Show me pictures of men wearing glasses
- Show me happy baby pics

Once we have some utterances, we have to teach LUIS how to pick out the **search topic** as the "facet" entity. Whatever the "facet" entity picks up is what will be searched.

1. Hover and click the word (or click consecutive words to select a group of words) and then select the "facet" entity.

   ![](./pics/14.png)

So your utterances may become something like this when facets are labeled:

   ![](./pics/15_1.png)

>**Note** This workshop does not include Azure Cognitive Search, however, this functionality has been left in for the sake of demonstration.

1. Select **Intents** in the left sidebar and add two more intents:

- Name one intent **"SharePic"**.  This might be identified by utterances like:

  - Share this pic
  - Can you tweet that?
  - Post to Twitter

  ![](./pics/16.png)
 
- Create another intent named **"OrderPic"**.  This could be communicated with utterances like:

  - Print this picture
  - I would like to order prints
  - Can I get an 8x10 of that one
  - Order wallets

  ![](./pics/17.png)

When choosing utterances, it can be helpful to use a combination of questions, commands, and "I would like to..." formats.

1. Finally add some sample utterances to the "None" intent. This helps LUIS label when things are outside the scope of your application. Add things like "I'm hungry for pizza", "Search videos", etc. You should have about 10-15% of your app's utterances within the None intent.

## Lab 1.2: Training the LUIS model

We're now ready to train our model. In this exercise, you will perform a simple training operation in order to test your model.  The testing will take place using the built-in testing panel in the LUIS portal.

1. In the top toolbar, select **Train**. During training, LUIS builds a model to map utterances to intents based on the training data you’ve provided.

    > [!TIP]
    > Training is not always immediate. Sometimes it gets queued and can take several minutes.

  ![](./pics/17_1.png)
## Create a public endpoint for the LUIS service

1. After training is finished, select **Manage** in the top toolbar. The following options will appear on the left toolbar:

    > [!NOTE]
    > The categories on the left pane may change as the portals are updated.  As a result, the keys and endpoints may fall under a different category than the one listed here.

    - **Settings**
    - **Publish settings**
    - **Azure Resources**
    - **Versions**

1. Select **Azure Resources**. This screen is used to manage the URL endpoints used to access the LUIS service.

    > [!NOTE]
    > An endpoint named **Starter_Key** is automatically created for testing purposes, and you could use that here - however to use the service in a production environment or inside of an application, you will always want to tie it to a real Language Understanding resource created in Azure.

1. You should see a **Prediction Resource** and a **Starter_Key** resource already created.  If you see the **Prediction Resource**, advance to the next section on **Publish the app**.
1. If you do not see an existing **Prediction Resource**, select **Add prediction resource**. The **Tenant** will already be selected.

   ![](./pics/18_1.png)

1. Select your subscription, and the resource with name **luisbotdeploymentID** you created in the Azure portal earlier and then select **Done** to connect the Language Understanding resource to the LUIS service.

   ![](./pics/18.png)

## Publish the app

1. In the top toolbar, select **Publish**.

   > [!NOTE]
   > You can publish to your **Production** or **Staging** endpoint. Select **Production**, and read about the reasons for the two endpoints.

   ![](./pics/19.png)
   
1. Under **Choose your publishing slot and settings**, select **Production Slot** and then select **Done**.

    Publishing creates an endpoint to call the LUIS model. The endpoint URL will be displayed. Copy the endpoint URL and add it to your list of keys for future use.

    ![](./pics/20.png)
1. In the top bar, select **Test**. Try typing a few utterances and see which intents are returned. Here are some examples you can try:

   | Utterance | Result | Score meaning |
   |---------|---------|---------|
   | **Show me pictures of a local beach** | Returns the **SearchPic** intent with a score | Positive match |
   | **Hello** | Returns the **Greeting** intent with a score | Fairly positive match |
   | **Send to Tom** | Returns the **Utilities** with a low score | Needs retraining or doesn't match any intents |

   ![](./pics/21.png)
To retrain the model for utterances with low scores, take the following steps:

1. Beside the low-scoring utterance (in this case, **Send to Tom**), select **Inspect**.

    ![](./pics/22.png)
    
1. Beside **Top-scoring intent**, select the drop-down and choose **SharePic** from the list.

    ![](./pics/23.png)
    
1. Close the **Test** panel.
1. Select the **Train** button to retrain your model.
1. Test the **Send to Tom** utterance again. It should now return the **SharePic** intent with a higher score.

     ![](./pics/24.png)

Your LUIS app is now ready to be used by client applications, tested in a browser through the listed endpoint, or integrated into a bot.


You can also [test your published endpoint in a browser](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/PublishApp#test-your-published-endpoint-in-a-browser). Copy the Endpoint URL. To open this URL in your browser, set the URL parameter `&q=` to your test query. For example, append `Find pictures of dogs` to your URL, and then press Enter. The browser displays the JSON response of your HTTP endpoint.

## Going further

If you still have time, spend time exploring the www.luis.ai site. Select "Prebuilt domains" and see [what is already available for you](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-prebuilt-domains). You can also review some of the [other features](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-concept-feature) and [patterns](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-concept-patterns)
, and check out the [BotBuilder-tools](https://github.com/Microsoft/botbuilder-tools) for creating LUIS models, managing LUIS models, simulating conversations, and more. Later, you may also be interested in [another course that includes how to design LUIS schema](https://aka.ms/daaia).
