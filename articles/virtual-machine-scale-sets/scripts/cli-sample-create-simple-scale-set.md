---
title: 'Exemples Azure CLI 2.0 : créer un groupe de machines virtuelles identiques | Microsoft Docs'
description: Exemples Azure CLI 2.0
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8946a55eb2307957f95372aae93d7f9788b8f952
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696653"
---
# <a name="create-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Créer un groupe de machines virtuelles identiques avec Azure CLI 2.0
Ce script permet de créer un groupe de machines virtuelles identiques Azure avec un système d’exploitation Ubuntu et les ressources réseau associées, y compris un équilibreur de charge. Une fois que vous avez exécuté le script, vous pouvez accéder aux instances de la machine virtuelle sur SSH.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exemple de script
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/simple-scale-set/simple-scale-set.sh "Create a simple virtual machine scale set")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement
Exécutez la commande suivante pour supprimer le groupe de ressources, le groupe identique et toutes les ressources associées.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explication du script
Ce script utilise les commandes suivantes pour créer un groupe de ressources, un groupe de machines virtuelles identiques, ainsi que toutes les ressources associées. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Crée le groupe de machines virtuelles identiques et le connecte au réseau virtuel, au sous-réseau et au groupe de sécurité réseau. Un équilibreur de charge est également créé pour distribuer le trafic vers les différentes instances de machine virtuelle. Cette commande spécifie aussi l’image de machine virtuelle à utiliser ainsi que les informations d’identification d’administration.  |
| [az group delete](/cli/azure/ad/group#delete) | Supprime un groupe de ressources, y compris toutes les ressources imbriquées. |

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur Azure CLI 2.0, consultez la [documentation sur Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).

Vous trouverez des exemples supplémentaires de scripts Azure CLI 2.0 de groupes de machines virtuelles identiques dans la [documentation relative aux groupes de machines virtuelles identiques Azure](../cli-samples.md).