---
title: Styles de cartes pris en charge dans Azure Maps | Microsoft Docs
description: Styles de cartes pris en charge par Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 08/28/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 8f0910e9040c962bae30a33b91a93e71e692dfdb
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713440"
---
# <a name="azure-maps-supported-map-styles"></a>Styles de cartes pris en charge dans Azure Maps
Azure Maps prend en charge quatre styles de cartes intégrées. Ces styles sont listés ci-dessous, avec leur description.

## <a name="road"></a>Route
Une carte **routière** est une carte standard qui affiche les routes, les éléments cartographiques naturels et créés par l’homme, ainsi que le nom de ces éléments cartographiques.

![Route](./media/supported-map-styles/road.png)

**API applicables :**
* [Image de la carte](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Vignette de la carte](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Commandes de la carte JS

## <a name="satellite"></a>Satellite 
Le style **satellite** est une combinaison d’imagerie satellite et d’imagerie aérienne.

![Satellite](./media/supported-map-styles/satellite.png)

**API applicables :**
* [Vignette Satellite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Commandes de la carte JS

## <a name="satelliteroadlabels"></a>Satellite_Road_Labels
Ce style de carte est un hybride qui montre des routes et des noms superposés à une imagerie satellite et aérienne.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**API applicables :**
* Commandes de la carte JS

## <a name="grayscaledark"></a>Grayscale_Dark
**Nuances de gris foncé** est une version plus sombre du style feuille de route.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**API applicables :**
* Commandes de la carte JS 