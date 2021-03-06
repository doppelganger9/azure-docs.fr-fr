---
title: Détection des anomalies dans une application Java - Microsoft Cognitive Services | Microsoft Docs
description: Explorez une application Java qui utilise l’API Détection d’anomalies dans Microsoft Cognitive Services. Envoyez des points de données d’origine à l’API et obtenez la valeur attendue et les points d’anomalies.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 228d440da358eba1322e2228c54f21e925e36ecd
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597946"
---
# <a name="anomaly-detection-java-application"></a>Détection des anomalies dans une application Java

Cet article illustre l’utilisation d’une application Java simple pour appeler l’API Détection d’anomalies.  
L’exemple envoie les données de série chronologique à l’API Détection d’anomalies avec votre clé d’abonnement, puis obtient tous les points d’anomalies et la valeur attendue pour chaque point de données à partir de l’API.

## <a name="prerequisites"></a>Prérequis

### <a name="platform-requirements"></a>Plateforme requise

Ce tutoriel a été développé à l’aide d’[IntelliJ IDEA](https://www.jetbrains.com/idea). Vous devez également installer [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) version 1.8+ et un outil de build [Maven d’Apache](http://maven.apache.org/) à jour.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>S’abonner à la détection d’anomalies et obtenir une clé d’abonnement 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Télécharger le projet du tutoriel

1. Accédez au [dépôt Java](https://github.com/MicrosoftAnomalyDetection/java-sample) MicrosoftAnomalyDetection.
2. Cliquez sur le bouton Clone ou Download.
3. Cliquez sur Download ZIP pour télécharger un fichier .zip du projet du tutoriel.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Ouvrir le projet du tutoriel

1. Extrayez le fichier .zip du projet du tutoriel.
2. Dans IntelliJ IDEA, cliquez sur **File > Open**. La boîte de dialogue Open File or Project s’affiche.
3. Sélectionnez le chemin racine du projet extrait, puis cliquez sur OK.
4. Dans le panneau Projects, développez **src > main > java**.
5. Double-cliquez sur com.microsoft.cognitiveservice.anomalydetection.Main.java pour charger le fichier dans l’éditeur.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Remplacer subscriptionKey et URI region

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Générer et exécuter le projet du tutoriel

1. Affichez le menu en cliquant avec le bouton droit n’importe où sous l’onglet de code source com.microsoft.cognitiveservice.anomalydetection.Main.java. 
2. Sélectionnez Run 'Main.main()'.
3. Le résultat de l’exemple de requête est retourné et affiché dans Terminal Server.

### <a name="result-of-the-tutorial-project"></a>Résultat du projet du tutoriel

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Référence d’API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
