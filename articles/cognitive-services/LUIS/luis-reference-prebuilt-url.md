---
title: Documentation de référence pour l’entité prédéfinie URL de LUIS - Azure | Microsoft Docs
titleSuffix: Azure
description: Cet article contient des informations sur l’entité prédéfinie URL dans Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 86989abab1dcf64384b8b26b9484bc508f2ce31f
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236786"
---
# <a name="url-entity"></a>Entité URL
L’entité URL extrait des URL avec des noms de domaine ou des adresses IP. Étant donné que cette entité est déjà formée, vous n’avez pas besoin d’ajouter d’exemples d’énoncés contenant des URL à l’application. L’entité URL est prise en charge uniquement dans la culture `en-us`. 

## <a name="types-of-urls"></a>Types d’entités URL
L’entité URL est gérée à partir du dépôt GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml)

## <a name="resolution-for-prebuilt-url-entity"></a>Résolution pour une entité prédéfinie URL
L’exemple suivant montre la résolution de l’entité **builtin.url**.

```JSON
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les entités [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) et [temperature](luis-reference-prebuilt-temperature.md).