---
title: Azure Content Moderator - Créer des révisions à l’aide de .NET | Microsoft Docs
description: Comment créer des révisions à l’aide du kit de développement logiciel (SDK) Azure Content Moderator pour .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 6a0ff48f4ea17f9c800f3e6c096df2492699f87a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35368148"
---
# <a name="create-reviews-using-net"></a>Créer des révisions à l’aide de .NET

Cet article fournit des informations et des exemples de code qui vont vous aider à démarrer avec le kit SDK Azure Content Moderator pour .NET pour :
 
- Créer un ensemble de révisions pour les modérateurs humains
- Obtenir le statut de révisions existantes pour les modérateurs humains

En règle générale, le contenu passe par des modérations automatisées avant planification d’une révision par un opérateur humain. Cet article ne couvre que la création de révisions en vue d’une modération humaine. Pour un scénario plus complet, consultez les didacticiels [Modération du contenu Facebook](facebook-post-moderation.md) et [Modération de catalogue eCommerce](ecommerce-retail-catalog-moderation.md).

Cet article part du principe que vous connaissez déjà Visual Studio et C#.

## <a name="sign-up-for-content-moderator-services"></a>S’inscrire aux services Content Moderator

Avant de pouvoir utiliser les services Content Moderator par le biais de l’API REST ou du kit SDK, vous avez besoin d’une clé d’abonnement.
Reportez-vous au guide de [démarrage rapide](quick-start.md) pour découvrir comment obtenir la clé.

## <a name="create-your-visual-studio-project"></a>Créer votre projet Visual Studio

1. Ajoutez un nouveau projet **Console app (.NET Framework)** à votre solution.

   Dans l’exemple de code, nommez le projet **CreateReviews**.

1. Sélectionnez ce projet comme unique projet de démarrage de la solution.

1. Ajoutez une référence à l’assembly de projet **ModeratorHelper** que vous avez créé dans le guide de [démarrage rapide d’assistance du client Content Moderator](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Installer les packages nécessaires

Installez les packages NuGet suivants :

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Mettre à jour les instructions using du programme

Mettez à jour les instructions d’utilisation du programme.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>Créez une classe pour associer les informations du contenu interne à un ID de révision

Ajoutez la classe suivante à la classe **Program**.
Utilisez cette classe pour associer l’ID de révision à l’ID de votre contenu interne pour l’élément.

    /// <summary>
    /// Associates the review ID (assigned by the service) to the internal
    /// content ID of the item.
    /// </summary>
    public class ReviewItem
    {
        /// <summary>
        /// The media type for the item to review.
        /// </summary>
        public string Type;

        /// <summary>
        /// The URL of the item to review.
        /// </summary>
        public string Url;

        /// <summary>
        /// The internal content ID for the item to review.
        /// </summary>
        public string ContentId;

        /// <summary>
        /// The ID that the service assigned to the review.
        /// </summary>
        public string ReviewId;
    }

### <a name="initialize-application-specific-settings"></a>Initialiser les paramètres spécifiques de l’application

> [!NOTE]
> Votre clé de service Content Moderator a une limite de fréquence des demandes par seconde (RPS). Si vous dépassez cette limite, le SDK lève une exception avec le code d’erreur 429. 
>
> Une clé de niveau gratuit a une limite de fréquence d’une demande par seconde (RSP).

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Ajoutez les constantes suivantes à la classe **Program** dans Program.cs.
    
    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Ajoutez les constantes et champs statiques suivants à la classe **Program** dans Program.cs.

Mettez à jour ces valeurs pour contenir des informations spécifiques à votre abonnement et équipe.

> [!NOTE]
> Vous définissez la constante TeamName par le nom utilisé lorsque vous avez créé votre abonnement à l’[outil de révision Content Moderator](https://contentmoderator.cognitive.microsoft.com/). Vous récupérez la constante TeamName dans la section **Informations d'identification** dans le menu **Paramètres** (icône engrenage).
>
> Le nom de votre équipe est la valeur du champ **Id** dans la section **API**.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "{teamname}";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "{callbackUrl}";

    /// <summary>
    /// The media type for the item to review.
    /// </summary>
    /// <remarks>Valid values are "image", "text", and "video".</remarks>
    private const string MediaType = "image";

    /// <summary>
    /// The URLs of the images to create review jobs for.
    /// </summary>
    private static readonly string[] ImageUrls = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
        // add more if you want
    };

    /// <summary>
    /// The metadata key to initially add to each review item.
    /// </summary>
    private const string MetadataKey = "sc";

    /// <summary>
    /// The metadata value to initially add to each review item.
    /// </summary>
    private const string MetadataValue = "true";

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Ajoutez les champs statiques suivants à la classe **Program** dans Program.cs.

Utilisez ces champs pour suivre l’état de l'application.

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// The cached review information, associating a local content ID
    /// to the created review ID for each item.
    /// </summary>
    private static List<ReviewItem> reviewItems =
        new List<ReviewItem>();

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>Créer une méthode pour écrire des messages dans le fichier journal

Ajoutez la méthode suivante à la classe **Program**. 

    /// <summary>
    /// Writes a message to the log file, and optionally to the console.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
    private static void WriteLine(string message = null, bool echo = false)
    {
        writer.WriteLine(message ?? String.Empty);

        if (echo)
        {
            Console.WriteLine(message ?? String.Empty);
        }
    }

## <a name="create-a-method-to-create-a-set-of-reviews"></a>Créer une méthode pour créer un ensemble de révisions

En règle générale, votre logique métier pour l’identification des images entrantes, du texte, ou de vidéo doit être vérifiée. Toutefois, n’utilisez ici qu’une liste fixe d’images.

Ajoutez la méthode suivante à la classe **Program**.

    /// <summary>
    /// Create the reviews using the fixed list of images.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void CreateReviews(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Creating reviews for the following images:", true);

        // Create the structure to hold the request body information.
        List<CreateReviewBodyItem> requestInfo =
            new List<CreateReviewBodyItem>();

        // Create some standard metadata to add to each item.
        List<CreateReviewBodyItemMetadataItem> metadata =
            new List<CreateReviewBodyItemMetadataItem>(
            new CreateReviewBodyItemMetadataItem[] {
                new CreateReviewBodyItemMetadataItem(
                    MetadataKey, MetadataValue)
            });

        // Populate the request body information and the initial cached review information.
        for (int i = 0; i < ImageUrls.Length; i++)
        {
            // Cache the local information with which to create the review.
            var itemInfo = new ReviewItem()
            {
                Type = MediaType,
                ContentId = i.ToString(),
                Url = ImageUrls[i],
                ReviewId = null
            };

            WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

            // Add the item informaton to the request information.
            requestInfo.Add(new CreateReviewBodyItem(
                itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
                CallbackEndpoint, metadata));

            // Cache the review creation information.
            reviewItems.Add(itemInfo);
        }

        var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
            "application/json", TeamName, requestInfo);

        // Update the local cache to associate the created review IDs with
        // the associated content.
        var reviewIds = reviewResponse.Result.Body;
        for (int i = 0; i < reviewIds.Count; i++)
        {
            Program.reviewItems[i].ReviewId = reviewIds[i];
        }

        WriteLine(JsonConvert.SerializeObject(
        reviewIds, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Créer une méthode pour obtenir l’état des révisions existantes

Ajoutez la méthode suivante à la classe **Program**. 

> [!Note]
> En pratique, vous devriez définir l’URL de rappel `CallbackEndpoint` comme URL qui recevra les résultats de la révision manuelle (via une requête HTTP POST).
> Vous pourriez modifier cette méthode pour vérifier l’état des révisions en attente.

    /// <summary>
    /// Gets the review details from the server.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void GetReviewDetails(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Getting review details:", true);
        foreach (var item in reviewItems)
        {
            var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
                TeamName, item.ReviewId);

            WriteLine(
                $"Review {item.ReviewId} for item ID {item.ContentId} is " +
                $"{reviewDetail.Result.Body.Status}.", true);
            WriteLine(JsonConvert.SerializeObject(
                reviewDetail.Result.Body, Formatting.Indented));

            Thread.Sleep(throttleRate);
        }
    }

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>Ajouter le code pour créer un ensemble de révisions et vérifier son état

Ajoutez le code suivant à la méthode **Main**.

Ce code simule de nombreuses opérations que vous effectuez lorsque vous définissez ou gérez une liste, ainsi que lorsque vous utilisez une liste pour faire des captures d’images. Les fonctionnalités de journalisation vous permettent de consulter les objets de réponse générés par les appels du kit de développement logiciel vers le service Content Moderator.

    using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        writer = outputWriter;
        using (var client = Clients.NewClient())
        {
            CreateReviews(client);
            GetReviewDetails(client);

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
            Thread.Sleep(latencyDelay * 1000);

            GetReviewDetails(client);
        }

        writer = null;
        outputWriter.Flush();
        outputWriter.Close();
    }

    Console.WriteLine();
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();

## <a name="run-the-program-and-review-the-output"></a>Exécuter le programme et examiner la sortie

L’exemple de sortie suivante s’affiche :

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Connectez-vous à l’outil de révision Content Moderator pour consulter la révision d’images en attente avec l’étiquette **sc** définie sur **true**. Vous pouvez aussi consulter les balises par défaut **a** et **r** et toutes balises personnalisées que vous avez définies dans l’outil de révision. 

Utilisez le bouton **Suivant** pour envoyer.

![Révision d’images pour les modérateurs humains](images/moderation-reviews-quickstart-dotnet.PNG)

Appuyez sur une touche pour continuer.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Consultez la sortie suivante dans votre fichier journal.

> [!NOTE]
> Dans votre fichier de sortie, les chaînes «\{teamname} » et « \{callbackUrl} » reflètent les valeurs des champs `TeamName` et `CallbackEndpoint`, respectivement.

L’ID de révision et les URL de contenu d’images sont différents à chaque exécution de l’application, et lorsqu’une révision est effectuée, le champ `reviewerResultTags` reflète comment l’utilisateur a balisé l’élément.

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Pending",
        "reviewerResultTags": [],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="your-callback-url-if-provided-receives-this-response"></a>Si fournie, votre URL de rappel reçoit cette réponse

Vous verrez une réponse semblable à celle-ci :

    {
        "ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
        "ModifiedOn": "2018-01-06T05:04:40.5525865Z",
        "ModifiedBy": "yourusername",
        "CallBackType": "Review",
        "ContentId": "0",
        "ContentType": "Image",
        "Metadata": {
            "sc": "true"
            },
        "ReviewerResultTags": {
            "a": "False",
            "r": "False",
        }
    }


## <a name="next-steps"></a>Étapes suivantes

[Téléchargez la solution Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) pour ce guide de démarrage rapide et d’autres guides de démarrage rapide Content Moderator pour .NET, puis commencez votre intégration.
