---
title: Vue d’ensemble de l’API Vérification orthographique Bing - Azure Cognitive Services | Microsoft Docs
description: L’API Vérification orthographique Bing utilise l’apprentissage automatique et la traduction automatique statistique pour vérifier l’orthographe en contexte.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.assetid: 64ABDFD4-0118-4B6C-A592-68E5EDDB8491
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: overview
ms.date: 05/03/2018
ms.author: nolachar
ms.openlocfilehash: 15c5f7eeb7d94d7e80533ee1fd12e33fa3bcd134
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43114292"
---
# <a name="what-is-bing-spell-check-api"></a>Qu’est-ce que l’API Vérification orthographique Bing ?

L’API Vérification orthographique Bing vous permet d’effectuer une vérification orthographique et grammaticale contextuelle.

Quelle est la différence entre les vérificateurs orthographiques standard et le vérificateur orthographique Bing de troisième génération ? Les vérificateurs orthographiques actuels procèdent à une vérification orthographique et grammaticale à partir d’ensembles de règles basées sur un dictionnaire, qui sont mis à jour et développés de manière périodique. En d’autres termes, les performances du vérificateur orthographique sont étroitement liées à la qualité du dictionnaire sous-jacent et aux compétences de l’équipe chargée de mettre à jour ce dictionnaire. On peut notamment citer comme exemple le vérificateur d’orthographe disponible dans Microsoft Word et d’autres outils de traitement de texte.

En revanche, Bing a développé un vérificateur orthographique basé sur le web, qui utilise l’apprentissage automatique et la traduction automatique statistique pour développer et mettre à jour en continu un algorithme en constante évolution et hautement contextuel. Le vérificateur orthographique repose sur un important recueil de documents et de recherches sur le web.

Il est capable de gérer n’importe quel scénario de traitement de texte :

- Il reconnaît l’argot et le langage informel
- Il détecte les erreurs de noms courantes en contexte
- Il corrige les erreurs de césure de mots avec une simple balise
- Il est capable de corriger les homophones en contexte et d’autres erreurs difficiles à repérer
- Il prend en charge les nouvelles marques, le nouveau contenu numérique et les dernières expressions populaires
- Mots qui se prononcent de la même façon, mais qui ont une orthographe et un sens différents, par exemple « toi » et « toit ».

## <a name="spell-check-modes"></a>Modes de vérification orthographique

L’API prend en charge deux modes de vérification, `Proof` et `Spell`.  Consultez les exemples [ici](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/).
### <a name="proof---for-documents-scenario"></a>Vérifier - pour les documents
Le mode par défaut est `Proof`. Le mode de vérification `Proof` fournit des contrôles plus complets et ajoute des fonctionnalités de mise en majuscules, de ponctuation, etc., pour faciliter la création d’un document. Malheureusement, il est uniquement disponible pour les segments en-US (Anglais-États-Unis), es-ES (Espagnol), pt-BR (Portugais) (Remarque : uniquement en version bêta pour l’espagnol et le portugais). Pour tous les autres marchés, définissez le paramètre de requête « mode » sur Corriger l’orthographe. 
<br /><br/>**REMARQUE :** si le texte de la requête contient plus de 4 096 caractères, il sera tronqué à 4 096 caractères, puis traité. 
### <a name="spell----for-web-searchesqueries-scenario"></a>Corriger l’orthographe - pour les requêtes/recherches web
La fonction `Spell` est plus agressive et renvoie de meilleurs résultats de recherche. Le mode `Spell` détecte la plupart des fautes d’orthographe, mais passe à côté de certaines erreurs grammaticales repérées par le mode `Proof`, par exemple, la mise en majuscules et les répétitions de mots.
<br /></br>**REMARQUE :** la longueur maximale de requête prise en charge est indiquée ci-dessous. Si la requête dépasse la limite, le résultat affiche une requête non modifiée.
<ul><li>130 caractères pour les codes de langue en, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, it, zh et ko. </li>
<li>65 caractères pour les autres codes de langue</li></ul>

## <a name="market-setting"></a>Paramètre de segment
Le segment doit être spécifié dans le paramètre de requête de l’URL de requête, sinon le vérificateur d’orthographe utilisera le segment par défaut obtenu via l’adresse IP.


## <a name="post-vs-get"></a>POST ou GET

L’API prend en charge les requêtes HTTP POST et HTTP GET. Le type de requête utilisé dépend de la longueur du texte que vous souhaitez vérifier. Si les chaînes sont toujours inférieures à 1 500 caractères, vous utiliserez des requêtes GET. Par contre, pour prendre en charge des chaînes contenant jusqu’à 10 000 caractères, vous utilisez des requêtes POST. La chaîne de texte peut inclure n’importe quel caractère UTF-8 valide.

L’exemple suivant montre une requête POST pour vérifier l’orthographe et la grammaire d’une chaîne de texte. L’exemple inclut le paramètre de requête [mode](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#mode) par souci d’exhaustivité (il est possible qu’il ne soit pas défini car `mode` est défini par défaut sur Vérifier). Le paramètre de requête [texte](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text) contient la chaîne à vérifier.
  
```  
POST https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?mode=proof&mkt=en-us HTTP/1.1  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
 
text=when+its+your+turn+turn,+john,+come+runing  
``` 

Si vous utilisez HTTP GET, vous devez inclure le paramètre de requête `text` dans la chaîne de requête de l’URL.
  
Voici la réponse à la requête précédente. La réponse contient un objet [SpellCheck](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#spellcheck). 
  
```  
{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  
  
Le champ [flaggedTokens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#flaggedtokens) répertorie les erreurs d’orthographe et de grammaire détectées par l’API dans la chaîne [texte](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#text). Le champ `token` contient le mot à remplacer. Vous devez utiliser le décalage de base zéro dans le champ `offset` pour trouver le jeton dans la chaîne `text`. Vous remplacez ensuite le mot à cet emplacement avec le mot indiqué dans le champ `suggestion`. 

Si le champ `type` indique RepeatedToken, vous remplacerez toujours le jeton par `suggestion`, mais vous devrez probablement aussi supprimer l’espace de fin.

## <a name="throttling-requests"></a>Demandes de limitation

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Étapes suivantes

Pour configurer rapidement votre première requête, consultez [Création de votre première requête](quickstarts/csharp.md).

Familiarisez-vous avec les [informations de référence sur l’API Vérification orthographique Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference). Vous y trouverez la liste des points de terminaison, en-têtes et paramètres de requête à utiliser pour demander les résultats de la recherche, ainsi que les définitions des objets de réponse. 

Veillez à lire les [exigences relatives à l’affichage et à l’utilisation des API Bing](./useanddisplayrequirements.md) pour respecter les règles d’utilisation des résultats.
