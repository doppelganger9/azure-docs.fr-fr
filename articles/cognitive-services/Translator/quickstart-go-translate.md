---
title: 'L’API de traduction de texte Translator Text : traduction de texte avec Go | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: Dans ce démarrage rapide, vous traduisez du texte d’une langue à une autre à l’aide de l’API de traduction de texte Translator Text avec Go dans Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: nolachar
ms.openlocfilehash: 72d4a3bc7563d6427335f1536f787eab6d7a0de4
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2018
ms.locfileid: "43769790"
---
# <a name="quickstart-translate-text-with-go"></a>Démarrage rapide : Traduire du texte avec Go

Dans ce démarrage rapide, vous traduisez du texte d’une langue à une autre à l’aide de l’API de traduction de texte Translator Text.

## <a name="prerequisites"></a>Prérequis

Pour exécuter ce code, vous devez installer la [distribution Go](https://golang.org/doc/install). Cet exemple de code utilisant uniquement les bibliothèques **principales**, il n’existe aucune dépendance externe.

Pour utiliser l’API de traduction de texte Translator Text, vous avez également besoin d’une clé d’abonnement. Consultez [Comment s’inscrire à l’API de traduction de texte Translator Text](translator-text-how-to-signup.md).

## <a name="translate-request"></a>Requête Traduire

Le code suivant traduit le texte source d’une langue à une autre en utilisant la méthode [Traduire](./reference/v3-0-translate.md).

1. Créez un nouveau projet Go dans votre éditeur de code favori.
2. Ajoutez le code ci-dessous.
3. Remplacez la valeur `subscriptionKey` par une clé d’accès valide pour votre abonnement.
4. Enregistrez le fichier avec une extension .go.
5. Ouvrez une invite de commandes sur un ordinateur sur lequel Go est installé.
6. Créez le fichier, par exemple : « go build quickstart-translate.go ».
7. Exécutez le fichier, par exemple : « quickstart-translate ».

```golang
package translate

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/translate?api-version=3.0"

    // Translate to German and Italian
    const params = "&to=de&to=it"

    const uri = uriBase + uriPath + params

    const text = "Hello, world!"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## <a name="translate-response"></a>Réponse Traduire

Une réponse correcte est retournée au format JSON, comme dans l’exemple suivant :

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Étapes suivantes

Explorer des packages Go pour les API Cognitive Services à partir de [Azure SDK pour Go](https://github.com/Azure/azure-sdk-for-go) sur GitHub.

> [!div class="nextstepaction"]
> [Explorer des packages Go sur GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
