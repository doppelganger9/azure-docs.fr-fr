---
title: Démarrage rapide de l’API Émotion avec Python | Microsoft Docs
description: Obtenez des informations et des exemples de code pour une prise en main rapide de l’API Émotion avec Python dans Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/05/2018
ms.author: anroth
ms.openlocfilehash: ff1f6b2ddc872d0ee63d9885b04b1f007bc86e33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35368008"
---
# <a name="emotion-api-python-quickstart"></a>Démarrage rapide de l’API Émotion avec Python

> [!IMPORTANT]
> La préversion de l’API Vidéo n’est plus proposée depuis le 30 octobre 2017. Essayez la nouvelle [préversion de l’API Video Indexer](https://azure.microsoft.com/services/cognitive-services/video-indexer/) pour extraire facilement des insights des vidéos et améliorer les expériences de découverte de contenu, telles que les résultats de recherche, en détectant le texte parlé, les visages, les personnes et les émotions. [Plus d’informations](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview)

Cette procédure pas à pas fournit des informations et des exemples de code pour une prise en main rapide de la [méthode Recognize de l’API Émotion](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) avec Python afin de reconnaître les émotions exprimées par une ou plusieurs personnes dans une image. 

Vous pouvez exécuter cet exemple en tant que bloc-notes Jupyter Notebook sur [MyBinder](https://mybinder.org) en cliquant sur le badge de lancement de Binder : [![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Configuration requise
Obtenez votre clé d’abonnement gratuite [à cette adresse](https://azure.microsoft.com/try/cognitive-services/)

## <a name="running-the-walkthrough"></a>Exécution de la procédure pas à pas
Pour poursuivre avec cette procédure pas à pas, remplacez `subscription_key` par la clé d’API obtenue précédemment.


```python
subscription_key = None
assert subscription_key
```

Vérifiez ensuite que l’URL du service correspond à la région utilisée lors de la configuration de la clé d’API. Si vous utilisez une clé d’essai, vous n’avez rien à modifier.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

Cette procédure pas à pas utilise des images stockées sur le disque. Vous pouvez également utiliser des images qui sont disponibles via une URL accessible publiquement. Pour plus d’informations, consultez la [documentation de l’API REST](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Étant donné que les données d’image sont transmises en tant que partie du corps de la requête, notez que vous devez définir l’en-tête `Content-Type` sur `application/octet-stream`. Si vous transmettez une image via une URL, souvenez-vous de définir l’en-tête pour :
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
créer un dictionnaire contenant l’URL :
```python
data = {'url': image_url}
```
et le transmettre à la bibliothèque `requests` à l’aide de :
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

Tout d’abord, téléchargez quelques exemples d’images sur le site de l’[API Émotion](https://azure.microsoft.com/services/cognitive-services/emotion/).


```bash
%%bash
mkdir -p images
curl -Ls https://aka.ms/csnb-emotion-1 -o images/emotion_1.jpg
curl -Ls https://aka.ms/csnb-emotion-2 -o images/emotion_2.jpg
```


```python
image_path = "images/emotion_1.jpg"
image_data = open(image_path, "rb").read()
```


```python
import requests
headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
response.raise_for_status()
analysis = response.json()
analysis
```




    [{'faceRectangle': {'height': 162, 'left': 130, 'top': 141, 'width': 162},
      'scores': {'anger': 9.29041e-06,
       'contempt': 0.000118981574,
       'disgust': 3.15619363e-05,
       'fear': 0.000589638,
       'happiness': 0.06630674,
       'neutral': 0.00555004273,
       'sadness': 7.44669524e-06,
       'surprise': 0.9273863}}]



L’objet JSON retourné contient les rectangles englobants des visages qui ont été reconnus, ainsi que les émotions détectées. Chaque émotion est associée à un score compris entre 0 et 1, où un score supérieur est plus indicatif d’une émotion qu’un score inférieur. 

Les lignes de code suivantes des émotions détectées sur les visages dans l’image à l’aide de la bibliothèque `matplotlib`. Pour réduire l’encombrement, seulement les trois premières émotions sont affichées.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from io import BytesIO
from PIL import Image

plt.figure(figsize=(5,5))

image  = Image.open(BytesIO(image_data))
ax     = plt.imshow(image, alpha=0.6)

for face in analysis:
    fr = face["faceRectangle"]
    em = face["scores"]
    origin = (fr["left"], fr["top"])
    p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
    plt.text(origin[0], origin[1], ct, fontsize=20)    
_ = plt.axis("off")
```

La fonction `annotate_image` indiquée si après peut être utilisée pour superposer les émotions au-dessus d’un fichier image en fonction de son chemin d’accès sur le système de fichiers. Elle est basée sur le code pour appeler l’API Émotion vu précédemment.


```python
def annotate_image(image_path):    
    image_data = open(image_path, "rb").read()
    headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
    response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
    response.raise_for_status()
    analysis = response.json()

    plt.figure()

    image  = Image.open(image_path)
    ax     = plt.imshow(image, alpha=0.6)

    for face in analysis:
        fr = face["faceRectangle"]
        em = face["scores"]
        origin = (fr["left"], fr["top"])
        p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
        ax.axes.add_patch(p)
        ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
        plt.text(origin[0], origin[1], ct, fontsize=20, va="bottom")    
    _ = plt.axis("off")
```

Enfin, la fonction `annotate_image` peut être appelée sur un fichier image, comme indiqué dans la ligne suivante :


```python
annotate_image("images/emotion_2.jpg")
```
