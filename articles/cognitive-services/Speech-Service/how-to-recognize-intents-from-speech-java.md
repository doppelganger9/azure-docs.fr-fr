---
title: Effectuer une reconnaissance des intentions vocales à l’aide du kit de développement logiciel (SDK) Speech pour Java
titleSuffix: Microsoft Cognitive Services
description: >
  Apprenez à reconnaître des intentions vocales à partir d’un fichier ou à partir d’un micro à l’aide du kit de développement logiciel (SDK) Speech pour Java.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 80c672526c0a93f9ee346415298a25b25ae3fb77
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144294"
---
# <a name="recognize-intents-from-speech-by-using-the-speech-sdk-for-java"></a>Effectuer une reconnaissance des intentions vocales à l’aide du kit de développement logiciel (SDK) Speech pour Java

[!INCLUDE [Article selector](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-intro.md)]

[!INCLUDE [Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-microphone.md)]

[!code-java[Intent recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithMicrophone)]

[!INCLUDE [Introduction to using a microphone and language](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-microphone-language.md)]

[!code-java[Intent recognition by using a microphone in a specified language](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithLanguage)]

[!INCLUDE [Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-continuous.md)]

[!code-java[Intent recognition by using events from a file](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentContinuousRecognitionWithFile)]

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Recherchez le code utilisé dans cet article au sein du dossier samples/java/jre/console.

## <a name="next-steps"></a>Étapes suivantes

- [Comment effectuer une reconnaissance vocale](how-to-recognize-speech-java.md)
- [Comment traduire une entrée vocale](how-to-translate-speech-java.md)
