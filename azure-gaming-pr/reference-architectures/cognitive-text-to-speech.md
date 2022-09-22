---
title: Text to Speech
description: In the case of players with reading disabilities, being able to hear text messages that were sent out using text to speech services may help bringing them into the conversation.
author: BrianPeek
keywords: 
ms.topic: reference-architecture
ms.date: 3/14/2019
ms.author: brpeek
ms.prod: azure-gaming
---

# Text-to-Speech Reference Architecture

Help bring everyone into the conversation by converting text messages to audio using **Text-to-Speech** for scenarios, such as game dialogue prototyping, greater accessibility, or NPC voices. Text-to-Speech includes prebuilt neural voice and custom neural voice features. Prebuilt neural voice can provide highly natural out-of-box voices with leading voice variety in terms of a large portfolio of languages and voices. Custom neural voice is an easy-to-use self-service for creating a highly natural custom voice. For details on the two features and voice samples, see [Text-to-Speech overview](/azure/cognitive-services/speech-service/text-to-speech).

Keep in mind that [this sample on GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk) about **Azure Speech** referenced in architecture below is only an example for guidance and there may be places to optimize the code before using in a production environment. You can also see the quickstart on [how to convert text to speech](/azure/cognitive-services/speech-service/get-started-text-to-speech).

## Architecture Diagram

[![Text to speech reference architecture](media/cognitive/cognitive-text-to-speech.png)](media/cognitive/cognitive-text-to-speech.png)

## Architecture Services

- [Azure Event Hub](/azure/event-hubs/event-hubs-about) - Chosen as it keeps the order of the messages received.
- [Azure Function](/azure/azure-functions/functions-overview) - Simplest way to run code on-demand in the cloud.
- [Azure Content Moderator](/azure/cognitive-services/content-moderator/overview) - Included to avoid detect profanity or other undesirable language.
- [Azure Text Analytics](/azure/cognitive-services/text-analytics/overview) - This service detects the language used by the player, which is required for the Azure Speech service. Alternatively, [Azure Content Moderator](/azure/cognitive-services/content-moderator/overview) can also detect the language.
- [Azure Speech](/azure/cognitive-services/speech-service/text-to-speech) - This service provides the Text-to-Speech functionality. With Text-to-Speech, you can convert input text into humanlike synthesized speech by using [prebuilt neural voices](/azure/cognitive-services/speech-service/language-support?tabs=speechtotext#prebuilt-neural-voices) or [custom neural voices](/azure/cognitive-services/speech-service/custom-neural-voice). 
- [Azure Premium Blob Storage](/azure/storage/blobs/storage-blob-storage-tiers) - Selected due to latency requirements, as the standard Azure Blob Storage may have limitations if the voice-file read is on demand during game play.

## Architecture Considerations

For [Azure Speech](/azure/cognitive-services/speech-service/text-to-speech), max number of transactions per second per Speech service resource is 200 transactions per second (TPS) for S0 (standard pricing tier) users. You can submit a request to increase this amount. For details, see [Text-to-speech quotas and limits per resource](/azure/cognitive-services/speech-service/speech-services-quotas-and-limits#text-to-speech-quotas-and-limits-per-resource). You can also use the [Long Audio API](/azure/cognitive-services/speech-service/long-audio-api) to asynchronously synthesize text-to-speech files longer than 10 minutes. 

When enabling this functionality in your game, keep in mind the following benefits:

- **Voices and languages supported** - A large portfolio of languages and variants are supported. For a complete list of voices, see the [prebuilt neural voice](/azure/cognitive-services/speech-service/language-support#prebuilt-neural-voices) and [custom neural voice](/azure/cognitive-services/speech-service/language-support#custom-neural-voice). You can also specify multiple languages for Text-to-Speech output. For details, see [how to adjust speaking languages](/azure/cognitive-services/speech-service/speech-synthesis-markup?tabs=csharp#adjust-speaking-languages). For custom neural voice, you can select to create [different languages](/azure/cognitive-services/speech-service/language-support?tabs=speechtotext#custom-neural-voice) from single language training data.
- **Emotional styles supported** - [Emotional tones](/azure/cognitive-services/speech-service/language-support?tabs=speechtotext#voice-styles-and-roles), such as cheerful, angry, sad, excited, hopeful, friendly, unfriendly, terrified, shouting, and whispering. You can adjust the [speaking style, style degree, and role](/azure/cognitive-services/speech-service/speech-synthesis-markup?tabs=csharp#adjust-speaking-styles) at the sentence level.
- **Low latency real-time speech synthesis supported** - Use the [Speech SDK](/azure/cognitive-services/speech-service/get-started-text-to-speech) to convert text to speech by using prebuilt neural voices or custom neural voices.
- **Visemes supported** - You can use [visemes](/azure/cognitive-services/speech-service/how-to-speech-synthesis-viseme) during real-time synthesizing to control the movement of 2D and 3D avatar models, so that the mouth movements are perfectly matched to synthetic speech. 
- **Fine-tuning Text-to-Speech output with Speech Synthesis Markup Language (SSML)** - With SSML, you can customize Text-to-Speech outputs, with richer voice tuning supports. For more details, see [how to improve synthesis with SSML](/azure/cognitive-services/speech-service/speech-synthesis-markup).
- **Audio outputs** - There is a list of supported [audio formats](/azure/cognitive-services/speech-service/rest-text-to-speech#audio-outputs). Each incorporates a bitrate and encoding type. The Speech Service supports 48-KHz on `en-US` and `zh-CN` regions. Other audio outputs lower than 48-KHz will be up-sampled to 48-KHz. 
- **Regions supported** - For information about regional availability, see the [regions](/azure/cognitive-services/speech-service/regions#text-to-speech) topic.

## Deployment Template

Click the following button to deploy the project to your Azure subscription:

[![Deploy to Azure](media/deploytoazure.svg)](https://aka.ms/arm-gaming-cognitive-services-text-to-speech)

This operation will trigger a template deployment of the [azuredeploy.json](https://github.com/Azure-Samples/gaming-cognitive-services-text-to-speech/blob/master/azuredeploy.json) ARM template file to your Azure subscription, which will create the necessary Azure resources. This may induce charges in your Azure account.

Have a look at the [general guidelines documentation](./general-guidelines.md#naming-conventions) that includes a section summarizing the naming rules and restrictions for Azure services.

>[!NOTE]
> If you're interested in how the ARM template works, review the Azure Resource Manager template documentation from each of the different services leveraged in this reference architecture:
>
> - [Create an Event Hub using Azure Resource Manager template](/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub)
> - [Automate resource deployment for your function app in Azure Functions](/azure/azure-functions/functions-infrastructure-as-code)

There are two types of Azure Cognitive Services subscriptions. The first is a subscription to a single service, such as Computer Vision or the Speech Services. A single-service subscription is restricted to just that service. The second type is a multi-service subscription. This allows you to use a single subscription for multiple Azure Cognitive Services. This option also consolidates billing. To make this reference architecture as modular as possible, the cognitive services are each setup as a single service.

Finally, add these Function [application settings](/azure/azure-functions/functions-how-to-use-azure-function-app-settings) so the sample project can connect to the Azure services:

- EVENTHUB_CONNECTION_STRING - The [connection string](/azure/event-hubs/event-hubs-get-connection-string) to the Azure Event Hub namespace that was created
- TEXTANALYTICS_KEY - The [access key](/azure/cognitive-services/cognitive-services-apis-create-account#access-your-resource) for the Azure Text Analytics cognitive service that was created
- SPEECH_KEY - The [access key](/azure/cognitive-services/cognitive-services-apis-create-account#access-your-resource) for the Azure Speech Cognitive Service that was created.

>[!TIP]
> To run the Azure Functions locally, update the *local.settings.json* file with these same app settings.

## Step by Step

1. The player's device opens a persistent and encrypted connection to the chat server in a specific region determined by **Azure Traffic Manager**. The chat server creates a process responsible for maintaining connectivity with the player along with some basic metadata.
1. The player's client sends a chat message to the chat server over the secure connection previously created. The player's process in the chat server receives the message, decrypts it, and parses it.
1. Standard validations are run, and the chat server calls the **Azure Event Hub** service, as it keeps the message order.
1. The **Azure Event Hub** serves as an input trigger for an **Azure Function**.
1. Optionally, yet a best practice, the **Azure Function** invokes the **Azure Content Moderator** Cognitive Service to review the content.
1. The **Azure Function** then invokes the **Azure Text Analytics** service to detect the language used by the player.
1. The Azure Function then uses that information to submit a request to start the conversion to audio. The **Azure Speech** service's response body is an audio file.
1. The Azure Function saves the audio file into persistent storage (**Azure Premium Blob Storage**) and gets the stored location.
1. Another **Azure Event Hub** is set as the output for the **Azure Function** and receives the pointer to the persistent storage containing the audio file.
1. The chat server receives the result from the **Azure Event Hub**.
1. The chat server reads the voice file from persistent storage using the pointer received.
1. The chat message and the audio file is sent to the recipient players' processes in the chat server. The processes run further validation on the text message, serialize both text and audio, and send them to the recipient players' devices over their secure connections. Finally the text is displayed in the chat screen and the audio file is played.

### Blob Storage Clean Up

Be diligent about cleaning up the audio files saved in persistent storage.  See the [managing Blob storage lifecycle](/azure/storage/blobs/storage-lifecycle-management-concepts) documentation for more information.

### Azure Text-to-Speech Service

To learn about the benefits and capabilities of the Text-to-Speech service, see [Text-to-Speech overview](/azure/cognitive-services/speech-service/text-to-speech).

### Azure Text Analytics

This service is required to detect the language of the chat string submitted by the player. At the moment, the service is only able to return the ISO 639-1 name ("en", "fr", etc) meaning a conversion table is going to be required as the Text to Speech language codes are a more granular, supporting specific language and dialect. For the full list see [language and region support](/azure/cognitive-services/speech-service/language-support#text-to-speech).

Alternatively, instead of the conversion table, you could let your players choose their preferred local language (i.e: Mexican Spanish instead of Argentinian Spanish) and the voice as part of the game settings.

### Alternatives

Azure Content Moderator can also detect the language of a string sent for moderation, meaning you could leverage it instead of Azure Text Analytics for that purpose, with the added benefit of having the string moderated.

## Security Considerations

Do not hard-code any Event Hub or Cognitive Services connection strings into the source of the Function.  Instead, at a minimum, leverage the [Function App Settings](/azure/azure-functions/functions-how-to-use-azure-function-app-settings#manage-app-service-settings) or, for even stronger security, use [Key Vault](/azure/key-vault/) instead. There is a tutorial explaining how to [create a Key Vault](https://blogs.msdn.microsoft.com/benjaminperkins/2018/06/13/create-an-azure-key-vault-and-secret/), how to [use a managed service identity with a Function](https://blogs.msdn.microsoft.com/benjaminperkins/2018/06/13/using-managed-service-identity-msi-with-and-azure-app-service-or-an-azure-function/) and finally how to [read the secret stored in Key Vault from a Function](https://blogs.msdn.microsoft.com/benjaminperkins/2018/06/13/how-to-connect-to-a-database-from-an-azure-function-using-azure-key-vault/).

Review the [Event Hub authentication and security model overview](/azure/event-hubs/event-hubs-authentication-and-security-model-overview) and put it into practice to ensure only your chat server can talk to the Event Hub.

## Pricing

If you don't have an Azure subscription, create a [free account](https://aka.ms/azfreegamedev) to get started with 12 months of free services. You're not charged for services included for free with Azure free account, unless you exceed the limits of these services. Learn how to check usage through the [Azure Portal](/azure/billing/billing-check-free-service-usage#check-usage-on-the-azure-portal) or through the [usage file](/azure/billing/billing-check-free-service-usage#check-usage-through-the-usage-file).

You are responsible for the cost of the Azure services used while running these reference architectures.  The total amount will vary based on usage. See the pricing webpages for each of the services that were used in the reference architecture:

- [Azure Speech Services](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)
- [Azure Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager/)
- [Azure Event Hub](https://azure.microsoft.com/pricing/details/event-hubs/)
- [Azure Function](https://azure.microsoft.com/pricing/details/functions/)
- [Azure Content Moderator](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/)
- [Azure Text Analytics](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)
- [Azure Premium Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/)

You can also use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/) to configure and estimate the costs for the Azure services that you are planning to use.
