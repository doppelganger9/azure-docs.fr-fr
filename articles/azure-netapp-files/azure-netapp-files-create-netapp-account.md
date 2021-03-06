---
title: Créer un compte NetApp pour accéder à Azure NetApp Files | Microsoft Docs
description: Ce document décrit comment accéder à Azure NetApp Files et créer un compte NetApp afin de pouvoir configurer un pool de capacité et créer un volume.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: ad8cc550ce69e4dc4c19a569718fa873a65b3620
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010342"
---
# <a name="create-a-netapp-account"></a>Créer un compte NetApp
La création d’un compte NetApp vous permet de configurer un pool de capacité et par la suite de créer un volume. Le panneau Azure NetApp Files permet de créer un nouveau compte NetApp.

## <a name="before-you-begin"></a>Avant de commencer
Vous devez figurer sur la liste verte pour accéder au fournisseur de ressources Microsoft.NetApp Azure et avoir été configuré de manière à utiliser le service Azure NetApp Files.  

[Page d’inscription à la préversion publique du service Azure NetApp Files](https://aka.ms/nfspublicpreview). 

## <a name="steps"></a>Étapes 

1. Recherchez L’URL du portail Azure en préversion à partir de votre invitation à la préversion et connectez-vous au portail. 
2.  Accédez à Azure NetApp Files à l’aide d’une des méthodes suivantes :  
  * Recherchez **Azure NetApp Files** dans la zone de recherche du portail Azure.  
  * Cliquez sur **Tous les services** dans la navigation, puis filtrez sur Azure NetApp Files.  

  Vous pouvez placer le panneau Azure NetApp Files dans vos Favoris en cliquant sur l’icône représentant une étoile en regard de celui-ci. 

3. Cliquez sur **+Ajouter** pour créer un nouveau compte NetApp.  
  La nouvelle fenêtre de compte NetApp s’affiche.  

4. Fournissez les informations suivantes pour votre compte NetApp : 
  * **Nom du compte**  
    Indiquez un nom unique pour l’abonnement.
  *  **Abonnement**  
    Sélectionnez un abonnement à partir de vos abonnements existants.
  * **Groupe de ressources**   
    Utilisez un groupe de ressources existant ou créez-en un.
  * **Lieu**  
    Sélectionnez la région dans laquelle vous souhaitez que le compte et ses ressources enfants soient situés.  
    Actuellement, le service Azure NetApp Files est pris en charge uniquement dans la région USA Est.  

    ![Nouveau compte NetApp](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Cliquez sur **Créer**.     
  Le compte NetApp que vous avez créé apparaît désormais dans le panneau Azure NetApp Files. 

## <a name="next-steps"></a>Étapes suivantes  

1. [Configurer un pool de capacité](azure-netapp-files-set-up-capacity-pool.md)
2. [Créer un volume pour Azure NetApp Files](azure-netapp-files-create-volumes.md)
3. [Configurer une stratégie d’exportation pour un volume (facultatif)](azure-netapp-files-configure-export-policy.md)
