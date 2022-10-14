# Azure chat bot with the Language Service and Azure Bot Service

# >Language service: 
The Language service includes a custom question answering feature that enables you to create a knowledge base of question and answer pairs that can be queried using natural language input
# >Azure Bot service:
This service provides a framework for developing, publishing, and managing bots on Azure.


## Concepts introduced in this sample
The [Custom question answering feature in Language Service][LS] enables you to build, train and publish a simple question and answer bot based on FAQ URLs, structured and unstructured documents or editorial content in minutes. In this sample, we demonstrate:


## Prerequisites
- This project requires a [Language resource](https://aka.ms/create-language-resource) with Custom question answering enabled and Microsoft Azure Account

To create a knowledge base, you must first provision a Language service resource in your Azure subscription.
## create a language service in Azure
![create-language-service-resource](https://user-images.githubusercontent.com/114275507/195900608-6e963698-325a-44da-8e5e-0a213c72ef4d.png)

## Language Studio. 
login into the language studio .If it's your first time logging in, you'll see a window appear that lets you choose a language resource.
![image](https://user-images.githubusercontent.com/112709511/195116278-0d61108b-9491-4c32-8980-eb6e731aaf60.png)

Select Create a new language resource. Then enter information for your new resource, such as a name, location and resource group.
# Creating a custom question answering knowledge base
The first challenge in creating a user support bot is to use the Language service to create a knowledge base. You can use the Language Studio's custom question answering feature to create, train, publish, and manage knowledge bases.


Bot Framework v4 Custom question answering bot sample. This sample demonstrates usage of advanced features of Custom question answering like [Precise answering][PA], support for unstructured sources along with [Multi-turn conversations][MT] and [Active Learning][AL] in a bot.

This bot has been created using [Bot Framework][BF], it shows how to create a bot that uses the [Custom question answering feature in Language Service][LS].


## Test the knowledge base
After creating a set of question-and-answer pairs, you must save it. This process analyzes your literal questions and answers and applies a built-in natural language processing model to match appropriate answers to questions, even when they are not phrased exactly as specified in your question definitions. Then you can use the built-in test interface in the Language Studio to test your knowledge base by submitting questions and reviewing the answers that are returned.
![test knowledge base](https://user-images.githubusercontent.com/114275507/195903404-aef81951-e6c3-48a5-9537-2472903be930.png)

## Use the knowledge base
When you're satisfied with your knowledge base, deploy it. Then you can use it over its REST interface. To access the knowledge base, client applications require:

The knowledge base ID
The knowledge base endpoint
The knowledge base authorization key
![Create a bot](https://user-images.githubusercontent.com/114275507/195905120-7de1e84c-07ef-4720-af7c-9feee48c9137.png)

## Build a bot with the Azure Bot Service
After you've created and deployed a knowledge base, you can deliver it to users through a bot.


### Configure knowledge base of the project
- Follow instructions [here][Quickstart] to create a Custom question answering project. You will need this project's name to be used as `ProjectName` in [appsettings.json](appsettings.json).
- Visit [Language Studio][LS] and open created project.
- Go to `Edit knowledge base` -> Click on `...` -> Click on `Import questions and answers` -> Click on `Import as TSV`.
- Import [SampleForCQA.tsv](CognitiveModels/SampleForCQA.tsv) file.
- You can test your knowledge base by clicking on `Test` option.
- Go to `Deploy knowledge base` and click on `Deploy`.

### Connect your bot to the project.
Follow these steps to update [appsettings.json](appsettings.json).
- In the [Azure Portal][Azure], go to your resource.
- Go to `Keys and Endpoint` under Resource Management.
- Copy one of the keys as value of `LanguageEndpointKey` and Endpoint as value of `LanguageEndpointHostName` in [appsettings.json](appsettings.json).
- `ProjectName` is the name of the project created in [Language Studio][LS].

## Create a bot for your knowledge base
You can create a custom bot by using the Microsoft Bot Framework SDK to write code that controls conversation flow and integrates with your knowledge base. However, an easier approach is to use the automatic bot creation functionality, which enables you create a bot for your deployed knowledge base and publish it as an Azure Bot Service application with just a few clicks.

![web app bot](https://user-images.githubusercontent.com/114275507/195903944-6f19ed6f-92c9-4030-83a2-114e89cb9307.png)

## To try this sample

- Install the Bot Framework Emulator version 4.14.0 or greater from [here][BFE]
- Run the bot from a terminal or from Visual Studio, choose option A or B.

  A) From a terminal

  ```bash
  # run the bot
  dotnet run
  ```

  B) Or from Visual Studio
  - Launch Visual Studio
  - File -> Open -> Project/Solution
  - Select `QnABot.csproj` file
  - Press `F5` to run the project
- Connect to the bot using Bot Framework Emulator
  1) Launch Bot Framework Emulator
  2) File -> Open Bot
  3) Enter a Bot URL of `http://localhost:3978/api/messages`

## Try Active Learning
- Try the following utterances:
  1) Surface Book
  2) Power
- In Language Studio, click on inspect to see the closeness in the scores of the returned answers.
- In [Bot Framework Emulator][BFE], a card is generated with the suggestions.
  - Clicking an option would send a [feedback record](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/questionanswering/question-answering-projects/add-feedback) which would show as suggestion under `Review suggestions` in [Language Studio][LS].
  - `ActiveLearningCardTitle`, `ActiveLearningCardNoMatchText` and `ActiveLearningCardNoMatchResponse` in the card could be changed from [QnAMakerBaseDialog.cs](Dialogs/QnAMakerBaseDialog.cs).

## Try Multi-turn prompt
- Try the following utterances:
  1) Accessibility
  2) Options
- You will notice that multi-turn prompts associated with the question are also returned in the responses.

## Try Precise Answering
- Try the following utterances:
  1) Accessibility
  2) Register
- You will notice a short answer returned along with a long answer.
- If testing in [Language Studio][LS], you might have to check `Include short answer response` at the top.
- You can disable precise answering by setting `EnablePreciseAnswer` to false in [appsettings.json](appsettings.json).
- You can set `DisplayPreciseAnswerOnly` to true in [appsettings.json](appsettings.json) to display just precise answers in the response.
- Learn more about [precise answering][PA].

## Query unstructured content
- Go to your project in [Language Studio][LS] -> In `Manage sources` click on `+ Add source`
- Click on `URLs` and add `https://www.microsoft.com/en-us/microsoft-365/blog/2022/01/27/from-empowering-frontline-workers-to-accessibility-improvements-heres-whats-new-in-microsoft-365/` and select **unstructured** in the `Classify file structure` dropdown.
- Try the following utterances:
  1) Frontline workers
  2) Hybrid work solutions
- You can observe that, answers are returned with high score.
- You can set `_includeUnstructuredSources` to false in [QnAMakerBaseDialog.cs](Dialogs/QnAMakerBaseDialog.cs) to prevent querying unstructured sources.

## Try Filters
- Go to your project in [Language Studio][LS] -> In `Edit knowledge bases` -> Under **Metadata** column click on `+ Add`
- Select a QnA to edit and add a key value pair, say `Language` : `CSharp`, and click on `Save changes`.
- Click on `Test` and select metadata that you just added(`Language : CSharp`) by clicking on **Show advanced options**.
- This will return answers with specified metadata only.
- You can filter answers using bot as well by passing metadata and/or source filters. Edit line no. 81 in [QnAMakerBaseDialog.cs](Dialogs/QnAMakerBaseDialog.cs) to something like below. [Learn more](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/questionanswering/question-answering/get-answers#queryfilters).
    ```csharp
    var filters = new Filters
    {
        MetadataFilter = new MetadataFilter()
        {
            LogicalOperation = Bot.Builder.AI.QnA.JoinOperator.AND.ToString()
        },
        LogicalOperation = Bot.Builder.AI.QnA.JoinOperator.AND.ToString()
    };
    filters.MetadataFilter.Metadata.Add(new KeyValuePair<string, string>("Language", "CSharp"));
    filters.SourceFilter.Add("SampleForCQA.tsv");
    filters.SourceFilter.Add("SampleActiveLearningImport.tsv");
    
    // Initialize Filters with filters in line No. 81
    
 ## Connect channels
When your bot is ready to be delivered to users, you can connect it to multiple channels; making it possible for users to interact with it through web chat, email, Microsoft Teams, and other common communication media.
![channels](https://user-images.githubusercontent.com/114275507/195905448-3d61621a-122a-40ca-b384-28c8272c32b1.png)

     
## Microsoft Teams channel group chat fix
When a bot (named as `HelpBot`) is added to a Teams channel or Teams group chat, you will have to refer it as `@HelpBot` `How to build a bot?` to get answers from the service.
However, bot tries to send `<at>HelpBot</at>` `How to build a bot?` as query to Custom question answering service which may not give expected results for question to bot. The following code removes `<at>HelpBot</at>` mentions of the bot from the message and sends the remaining text as query to the service.
- Goto `Bots/QnABot.cs`
- Add References
    ```csharp
    using Microsoft.Bot.Connector;
    using System.Text.RegularExpressions;
    ```
- Modify `OnTurnAsync` function as:
    ```csharp
    public override async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken = default)
        {
            // Teams group chat
            if (turnContext.Activity.ChannelId.Equals(Channels.Msteams))
            {
                turnContext.Activity.Text = turnContext.Activity.RemoveRecipientMention();
            }
            
            await base.OnTurnAsync(turnContext, cancellationToken);

            // Save any state changes that might have occurred during the turn.
            await ConversationState.SaveChangesAsync(turnContext, false, cancellationToken);
            await UserState.SaveChangesAsync(turnContext, false, cancellationToken);
        }
 ## Test the chat bot in microsoft teams

![Screenshot 2022-10-14 225521](https://user-images.githubusercontent.com/114275507/195905913-7e7df694-aa17-4d04-945f-7f37398a2c6e.png)

## Further reading
- [How bots work][90]
- [Question Answering Documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/language-service/question-answering/overview)
- [Channels and Bot Connector Service](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)
- [Active learning Documentation][AL]
- [Multi-turn Conversations][MT]
- [Precise Answering][PA]



   


