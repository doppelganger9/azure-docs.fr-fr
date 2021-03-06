---
title: Analyser le texte en langage naturel dans l’API Language Understanding (LUIS) à l’aide de Java - Cognitive Services - Azure Cognitive Services | Microsoft Docs
description: Dans ce démarrage rapide, utilisez une application LUIS publique disponible pour déterminer l’intention d’un utilisateur à partir du texte conversationnel. À l’aide de Java, envoyez l’intention de l’utilisateur sous forme de texte au point de terminaison de prédiction HTTP de l’application publique. Au niveau du point de terminaison, LUIS applique le modèle de l’application publique pour analyser le sens du texte en langage naturel, déterminer l’intention globale et extraire les données pertinentes pour le domaine de l’application.
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: 4dd5437940994a2f264b5a11baebcd67fdddb43d
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163088"
---
# <a name="quickstart-analyze-text-using-java"></a>Démarrage rapide : Analyser le texte à l’aide de Java

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Prérequis

* [JDK SE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Kit de développement Java, Édition Standard)
* [Visual Studio Code](https://code.visualstudio.com/)
* ID d’application publique : df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtenir la clé LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Analyser le texte avec le navigateur

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-java"></a>Analyser le texte avec Java 

Vous pouvez utiliser Java pour accéder aux résultats que vous avez vus dans la fenêtre du navigateur à l’étape précédente. 

1. Copiez le code suivant pour créer une classe dans un fichier nommé `LuisGetRequest.java` :

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. Remplacez la valeur de la variable `YOUR-KEY` par votre clé LUIS.

3. Compilez le programme java avec `javac -cp ":lib/*" LuisGetRequest.java`. 

4. Exécutez l’application avec `java -cp ":lib/*" LuisGetRequest.java`. Des valeurs identiques à celles que vous avez vues plus tôt dans la fenêtre du navigateur s’affichent.

    ![Fenêtre de console affichant le résultat JSON de l’application LUIS](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>Clés LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Supprimer les ressources

Supprimez le fichier Java. 

## <a name="next-steps"></a>Étapes suivantes
> [!div class="nextstepaction"]
> [Ajouter des énoncés](luis-get-started-java-add-utterance.md)