---
title: Notes de publication du service API Visage | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Les notes de publication du service API Visage intègrent un historique des modifications apportées aux différentes versions.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 918b3ea5dbaaa44e4eee1a679354c7becffd4686
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35370308"
---
# <a name="face-api-release-notes"></a>Notes de publication d’API Visage

Cet article fait référence à la version 1.0 du service API Visage de Microsoft.

### <a name="release-changes-in-may-2018"></a>Modifications apportées à la version de mai 2018

* Nous avons considérablement amélioré l’attribut `gender` ainsi que nous avons amélioré les attributs `age`, `glasses`, `facialHair`, `hair` et `makeup`. Utilisez-les avec le paramètre [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes`. 

* Nous avons augmenté la taille maximale des fichiers d’image d’entrée la faisant passer de 4 à 6 Mo dans [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) et [LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Modifications apportées à la version de mars 2018

* Nous avons ajouté un conteneur à l’échelle un million : [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) et [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Vous trouverez davantage d’informations dans la section [Comment utiliser la fonctionnalité à grande échelle](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Nous avons augmenté le paramètre [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` le faisant passer de [1, 5] à [1, 100], 10 étant la valeur par défaut.

### <a name="release-changes-in-may-2017"></a>Modifications apportées à la version de mai 2017

* Nous avons ajouté les attributs `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure` et `noise` dans le paramètre [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes`.

* Les paramètres PersonGroup et [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) acceptent désormais 10 000 personnes.

* Nous prenons en charge la pagination dans [PersonGroup Person - List](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) avec comme paramètres facultatifs `start` et `top`.

* Nous prenons en charge l’accès concurrentiel pour ajouter et supprimer des visages concernant les listes FaceLists et les personnes dans PersonGroup.

### <a name="release-changes-in-march-2017"></a>Modifications apportées à la version de mars 2017
* Nous avons ajouté l’attribut `emotion` dans le paramètre [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes`.

* Nous avons résolu le problème qui entraînait une absence de détection et le renvoi d’un rectangle depuis [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) en tant que `targetFace` dans [FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) et [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Nous avons résolu le problème affectant la taille du visage détectable pour garantir qu’elle se trouvait bien entre 36 x 36 et 4 096 x 4096 pixels.

### <a name="release-changes-in-november-2016"></a>Modifications apportées à la version de novembre 2016
* Nous avons ajouté un abonnement Stockage Visage standard qui permet de stocker des visages persistants supplémentaires lorsque vous utilisez [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ou [FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) pour l’identification ou la mise en correspondance des similitudes. Les images stockées sont facturées au tarif de 0,5 $ tous les 1 000 visages (tarif journalier au prorata). Les abonnements de niveau gratuit sont toujours limités à un total de 1 000 personnes.

### <a name="release-changes-in-october-2016"></a>Modifications apportées à la version d’octobre 2016
* Nous avons modifié le message d’erreur lorsque plusieurs visages apparaissent dans targetFace. Auparavant, il indiquait « There are more than one face in the image ». Maintenant, il indique « There is more than one face in the image » dans [FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) et [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Modifications apportées à la version de juillet 2016
* Nous prenons en charge l’authentification d’objet Face to Person dans [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Nous avons ajouté un paramètre `mode` facultatif qui permet de sélectionner deux modes de fonctionnement : `matchPerson` et `matchFace` dans [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), le paramètre par défaut étant `matchPerson`.

* Nous avons ajouté un paramètre `confidenceThreshold` facultatif qui permet à l’utilisateur de définir le seuil indiquant si un visage appartient à l’objet Person dans [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Nous avons ajouté les paramètres `start` et `top` facultatifs dans [PersonGroup - List](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) pour permettre à l’utilisateur d’indiquer le point de départ et le nombre total de PersonGroups dans la liste.

### <a name="v10-changes-from-v0"></a>Changements apportés entre la version 0 et la version 1.0
* Nous avons mis à jour le point de terminaison de la racine du service le faisant passer de ```https://westus.api.cognitive.microsoft.com/face/v0/``` à ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Nous avons apporté des modifications à : [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) et [Face - Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* Nous avons mis à jour la taille minimale pour la détection des visages la faisant passer à 36 x 36 pixels. Les visages dont la taille est inférieure à 36 x 36 pixels ne seront pas détectés.

* Nous avons abandonné les données PersonGroup et Person qui étaient disponibles dans la version 0 de Visage. Ces données ne sont pas accessibles dans la version 1.0 de Visage.

* Nous avons abandonné le point de terminaison de la version 0 de l’API Visage au 30 juin 2016.
