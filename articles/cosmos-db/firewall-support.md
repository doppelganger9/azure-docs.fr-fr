---
title: Prise en charge du pare-feu Azure Cosmos DB et contrôle d’accès IP | Microsoft Docs
description: Découvrez comment utiliser les stratégies de contrôle d’accès IP pour la prise en charge du pare-feu dans les comptes de base de données Azure Cosmos DB.
keywords: contrôle d’accès IP, prise en charge du pare-feu
services: cosmos-db
author: kanshiG
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: govindk
ms.openlocfilehash: b21debdd6baa0a6587318ad861a821840ec6879c
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666695"
---
# <a name="azure-cosmos-db-firewall-support"></a>Prise en charge du pare-feu Azure Cosmos DB
Pour sécuriser les données stockées dans un compte de base de données Azure Cosmos DB, Azure Cosmos DB assure la prise en charge d’un [modèle d’autorisation](https://msdn.microsoft.com/library/azure/dn783368.aspx) basé sur une clé secrète, qui utilise un code d’authentification de message basé sur le hachage (HMAC) à forte intégrité. Outre le modèle d’autorisations basé sur un secret, Azure Cosmos DB prend désormais en charge les contrôles d’accès basés sur une stratégie IP pour la prise en charge du pare-feu entrant. Ce modèle est semblable aux règles de pare-feu d’un système de base de données classique et renforce la sécurité du compte de base de données Azure Cosmos DB. Avec ce modèle, vous pouvez désormais configurer un compte de base de données Azure Cosmos DB pour qu’il soit accessible uniquement à partir d’un ensemble d’ordinateurs et/ou de services cloud approuvés. L’accès aux ressources Azure Cosmos DB à partir de ces ensembles d’ordinateurs et de services approuvés nécessite toujours que l’appelant présente un jeton d’autorisation valide.

> [!NOTE]
> À l’heure actuelle, la prise en charge du pare-feu est disponible pour les comptes d’API SQL Azure Cosmos DB et d’API Mongo. La configuration des pare-feu pour d’autres API et clouds souverains, tels qu’Azure Allemagne ou Azure Government, sera prochainement disponible. Si vous envisagez de configurer une liste ACL du point de terminaison de service pour votre compte Azure Cosmos DB et que celui-ci est assorti d’un pare-feu IP, notez la configuration de ce dernier, supprimez-le, puis configurez la liste ACL du point de terminaison de service. Après avoir configuré le point de terminaison de service, vous pouvez réactiver le pare-feu IP si nécessaire.

## <a name="ip-access-control-overview"></a>Vue d’ensemble du contrôle d’accès IP
Par défaut, un compte de base de données Azure Cosmos DB est accessible depuis l’Internet public tant que la demande est accompagnée d’un jeton d’autorisation valide. Pour configurer le contrôle d’accès basé sur la stratégie IP, l’utilisateur doit fournir le jeu d’adresses IP ou des plages d’adresses IP au format CIDR pour être ajouté à la liste d’adresses IP clients autorisées pour un compte de base de données donné. Une fois cette configuration appliquée, toutes les demandes provenant d’ordinateurs qui ne figurent pas sur cette liste autorisée sont bloquées par le serveur.  Le flux de traitement des connexions du contrôle d’accès basé sur IP est décrit dans le diagramme suivant :

![Diagramme illustrant le processus de connexion du contrôle d’accès basé sur IP](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> Configuration de la stratégie de contrôle d’accès IP
La stratégie de contrôle d’accès IP peut être définie sur le Portail Azure ou par programme avec [Azure CLI](cli-samples.md), [Azure PowerShell](powershell-samples.md) ou [l’API REST](/rest/api/cosmos-db/) en mettant à jour la propriété **ipRangeFilter**. 

Pour définir la stratégie de contrôle d’accès IP sur le Portail Azure, accédez à la page du compte Azure Cosmos DB, cliquez sur **Pare-feu et réseaux virtuels** dans le menu de navigation, remplacez la valeur **Autoriser l’accès à partir de** par **Réseaux sélectionnés**, puis cliquez sur **Enregistrer**. 

![Capture d’écran montrant comment ouvrir la page Pare-feu sur le Portail Azure](./media/firewall-support/azure-portal-firewall.png)

Une fois le contrôle d’accès IP activé, le portail fournit la capacité de spécifier des adresses IP et des plages, ainsi que des commutateurs permettant l’accès à d’autres services Azure et au Portail Azure. Vous trouverez des informations supplémentaires sur ces commutateurs dans les sections suivantes.

> [!NOTE]
> En activant une stratégie de contrôle d’accès IP pour votre compte de base de données Azure Cosmos DB, tous les accès à votre compte de base de données Azure Cosmos DB à partir d’ordinateurs ne figurant pas sur la liste de plages d’adresses IP autorisées sont bloqués. En vertu de ce modèle, la navigation dans le plan de données à partir du portail sera également bloquée pour assurer l’intégrité du contrôle d’accès.

## <a name="connections-from-the-azure-portal"></a>Connexions à partir du Portail Azure 

Lorsque vous activez une stratégie de contrôle d’accès IP par programme, vous devez ajouter l’adresse IP du Portail Azure à la propriété **ipRangeFilter** pour conserver l’accès. Les adresses IP du portail sont :

|Région|Adresse IP|
|------|----------|
|Toutes les régions, à l’exception de celles spécifiées ci-dessous|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Allemagne|51.4.229.218|
|Chine|139.217.8.252|
|Gouvernement des États-Unis|52.244.48.71|

L’accès au Portail Azure est activé par défaut lorsque vous modifiez le paramètre du Pare-feu par **Réseaux sélectionnés** dans le Portail Azure. 

![Capture d’écran montrant comment activer l’accès au Portail Azure](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-global-azure-datacenters-or-azure-paas-services"></a>Connexions à partir de centres de données Azure ou de services PaaS Azure internationaux

Des services Azure PaaS tels qu’Azure Stream Analytics et Azure Functions sont utilisés conjointement avec Azure Cosmos DB. Pour autoriser des applications d’autres services Azure PaaS à se connecter à vos ressources Azure Cosmos DB, un paramètre de pare-feu doit être activé. Pour activer ce paramètre de pare-feu, ajoutez l’adresse IP 0.0.0.0 à la liste des adresses IP autorisées. L’adresse IP 0.0.0.0 restreint les connexions au compte Azure Cosmos DB à la plage d’adresses IP du centre de données Azure. Ce paramètre n’autorise pas l’accès au compte Azure Cosmos DB à partir d’autres plages d’adresses IP.

> [!IMPORTANT]
> Cette option configure le pare-feu pour autoriser toutes les connexions à partir d’Azure, notamment les connexions issues des abonnements d’autres clients. Lorsque vous sélectionnez cette option, vérifiez que votre connexion et vos autorisations utilisateur limitent l’accès aux seuls utilisateurs autorisés.
> 

L’accès aux connexions à partir de centres de données Azure internationaux est activé par défaut lorsque vous modifiez le paramètre du Pare-feu par **Réseaux sélectionnés** dans le Portail Azure. 

![Capture d’écran montrant comment ouvrir la page Pare-feu sur le Portail Azure](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Connexions à partir de votre adresse IP actuelle

Pour simplifier le développement, le portail Azure vous aide à identifier et à ajouter l’adresse IP de votre ordinateur client à la liste autorisée, afin que les applications qui exécutent votre machine puissent accéder au compte Azure Cosmos DB. L’adresse IP du client est ici détectée comme étant vue par le portail. Il peut s’agir de l’adresse IP client de votre ordinateur, mais il peut également s’agir de l’adresse IP de votre passerelle réseau. N’oubliez pas de la supprimer avant de passer à la production.

Pour activer votre IP actuel, sélectionnez **Ajouter mon IP actuel**, qui ajoute votre IP actuel à la liste d’IP, puis cliquez sur **Enregistrer**.

![Capture d’écran montrant comment configurer les paramètres de pare-feu pour l’adresse IP actuelle](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Connexions à partir de services cloud
Dans Azure, les services cloud sont une méthode courante d’hébergement de la logique de service de couche intermédiaire à l’aide d’Azure Cosmos DB. Pour autoriser l’accès à un compte de base de données Azure Cosmos DB à partir d’un service cloud, l’adresse IP publique de ce dernier doit être ajoutée à la liste d’adresses IP autorisées de votre compte de base de données Azure Cosmos DB en [configurant la stratégie de contrôle d’accès IP](#configure-ip-policy). Cela garantit que toutes les instances de rôle des services cloud ont accès à votre compte de base de données Azure Cosmos DB. Vous pouvez récupérer des adresses IP pour vos services cloud dans le portail Azure, comme illustré dans la capture d’écran suivante :

![Capture d’écran illustrant l’adresse IP publique pour un service cloud affichée dans le portail Azure](./media/firewall-support/public-ip-addresses.png)

Lorsque vous faites évoluer votre service cloud en ajoutant des instances de rôle supplémentaires, ces nouvelles instances auront automatiquement accès au compte de base de données Azure Cosmos DB, car ils font partie du même service cloud.

## <a name="connections-from-virtual-machines"></a>Connexions à partir de machines virtuelles
Des [machines virtuelles](https://azure.microsoft.com/services/virtual-machines/) ou des [jeux de mise à l’échelle de machine virtuelle](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) peuvent également être utilisés pour héberger les services de couche intermédiaire à l’aide d’Azure Cosmos DB.  Pour configurer le compte de base de données Azure Cosmos DB afin qu’il autorise l’accès à partir de machines virtuelles, les adresses IP publiques de la machine virtuelle ou du groupe de machines virtuelles identiques doivent figurer parmi les adresses IP autorisées de votre compte de base de données Azure Cosmos DB en [configurant la stratégie de contrôle d’accès IP](#configure-ip-policy). Vous pouvez récupérer des adresses IP pour des machines virtuelles dans le portail Azure, comme illustré dans la capture d’écran suivante.

![Capture d’écran illustrant une adresse IP publique pour une machine virtuelle affichée dans le portail Azure](./media/firewall-support/public-ip-addresses-dns.png)

Lorsque vous ajoutez des instances de machine virtuelle supplémentaires au groupe, elles disposent automatiquement d’un accès à votre compte de base de données Azure Cosmos DB.

## <a name="connections-from-the-internet"></a>Connexions à partir d’Internet
Lorsque vous accédez à un compte de base de données Azure Cosmos DB à partir d’un ordinateur sur Internet, l’adresse IP ou la plage d’adresses IP de l’ordinateur doit être ajoutée à la liste d’adresses IP autorisées pour le compte de base de données Azure Cosmos DB. 

## <a name="using-azure-resource-manager-template-to-set-up-the-ip-access-control"></a>Utilisation d’un modèle Azure Resource Manager pour configurer le contrôle d’accès IP

Ajoutez le JSON suivant à votre modèle pour configurer le contrôle d’accès IP. Un modèle Resource Manager pour un compte aura un attribut ipRangeFilter composé d’une liste de plages d’adresses IP qui devrait être sur une liste verte.

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
     "databaseAccountOfferType": "Standard",
     "name": "[parameters('databaseAccountName')]",
     "ipRangeFilter":"10.0.0.1,10.0.0.2,183.240.196.255"
   }
   }
```

## <a name="troubleshooting-the-ip-access-control-policy"></a>Dépannage de la stratégie de contrôle d’accès IP
### <a name="portal-operations"></a>Opérations du portail
En activant une stratégie de contrôle d’accès IP pour votre compte de base de données Azure Cosmos DB, tous les accès à votre compte de base de données Azure Cosmos DB à partir d’ordinateurs ne figurant pas sur la liste de plages d’adresses IP autorisées sont bloqués. Par conséquent, si vous souhaitez autoriser les opérations de plan de données du portail, par exemple, la navigation dans les conteneurs et l’interrogation des documents, vous devez autoriser explicitement l’accès au Portail Azure sur la page **Pare-feu** du portail. 

### <a name="sdk--rest-api"></a>Kit de développement logiciel (SDK) et API REST
Pour des raisons de sécurité, l’accès via le Kit de développement logiciel (SDK) ou l’API REST à partir d’ordinateurs ne figurant pas dans la liste autorisée renverra une réponse générique 404 Introuvable, ainsi que des détails supplémentaires. Consultez la liste des adresses IP autorisées qui est configurée pour votre compte de base de données Azure Cosmos DB, afin de vérifier que la configuration de la stratégie appropriée est appliquée à votre compte de base de données Azure Cosmos DB.

## <a name="next-steps"></a>Étapes suivantes
Vous trouverez des conseils sur les performances relatives au réseau sur la page [Conseils relatifs aux performances](performance-tips.md).

