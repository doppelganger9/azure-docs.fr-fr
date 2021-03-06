---
title: Gérer les espaces de travail dans Azure Log Analytics et sur le portail OMS | Microsoft Docs
description: Vous pouvez gérer les espaces de travail dans Azure Log Analytics et sur le portail OMS avec diverses tâches administratives sur les utilisateurs, comptes, espaces de travail et comptes Azure.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: cb927c0bad69bb3b5b3001e4ba19b11acd1eb316
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389967"
---
# <a name="manage-workspaces"></a>Gestion des espaces de travail

Pour gérer l’accès à Log Analytics, effectuez diverses tâches administratives liées aux espaces de travail. Cet article fournit des conseils et des procédures pour gérer les espaces de travail. Un espace de travail est en fait un conteneur qui inclut des informations de compte et des informations de configuration simple pour le compte. Vous ou d’autres membres de votre organisation pouvez utiliser plusieurs espaces de travail pour gérer différents ensembles de données provenant de tout ou partie de votre infrastructure informatique.

Pour créer un espace de travail, vous devez :

1. J’ai un abonnement Azure.
2. Choisissez un nom d’espace de travail.
3. Associez l’espace de travail à l’un de vos abonnements et groupes de ressources.
4. Choisissez un emplacement géographique.

## <a name="determine-the-number-of-workspaces-you-need"></a>Définition du nombre d’espaces de travail nécessaires
Un espace de travail est une ressource Azure et un conteneur dans lequel les données sont collectées, agrégées, analysées et présentées dans le portail Azure.

Vous pouvez disposer de plusieurs espaces de travail par abonnement Azure et vous pouvez avoir accès à plus d’un espace de travail, avec la possibilité de pouvoir envoyer facilement des requêtes entre ceux-ci. Cette section décrit dans quelles conditions il peut être utile de créer plusieurs espaces de travail.

Aujourd'hui, un espace de travail fournit :

* un emplacement géographique pour le stockage des données ;
* une isolation des données pour définir différents droits d’accès utilisateur ;
* une étendue pour la configuration des paramètres, comme la limitation des données et la rétention.

Du point de vue de la consommation, nous vous recommandons de créer aussi peu d’espaces de travail que possible. L’expérience d’administration et de requête n’en sera que plus simple et rapide. Cependant, compte tenu des caractéristiques précédentes, vous pouvez créer plusieurs espaces de travail si :

* Vous travaillez pour une entreprise globale et vous avez besoin de stocker vos données dans des régions spécifiques pour des raisons de conformité ou de souveraineté des données.
* Vous utilisez Azure et vous souhaitez éviter les frais liés au transfert de données sortantes en configurant un espace de travail dans la même région que les ressources Azure qu’il gère.
* Vous souhaitez allouer des frais à différents départements ou groupes métier en fonction de leur utilisation en créant un espace de travail pour chaque département ou groupe métier dans son propre abonnement Azure.
* Vous êtes fournisseur de services managés et vous devez isoler les données Log Analytics des autres données de vos clients.
* Vous gérez plusieurs clients et vous souhaitez que chaque client/département/groupe métier voie uniquement ses propres données, pas celles des autres.

Lorsque vous utilisez des agents Windows pour collecter des données, vous pouvez [configurer chacun d’entre eux pour qu’il fournisse des rapports à un ou plusieurs espaces de travail](log-analytics-windows-agents.md).

Si vous utilisez System Center Operations Manager, chaque groupe d’administration Operations Manager ne peut être connecté qu’à un seul espace de travail. Vous pouvez installer Microsoft Monitoring Agent sur les ordinateurs gérés par Operations Manager et configurer l’agent pour qu’il fournisse des rapports à Operations Manager et à un espace de travail Log Analytics différent.

## <a name="workspace-information"></a>Informations sur l’espace de travail

Vous pouvez afficher des détails sur votre espace de travail dans le portail Azure. 

1. Si ce n’est pas déjà fait, connectez-vous au [portail Azure](https://portal.azure.com).

2. Dans le portail Azure, cliquez sur **Tous les services**. Dans la liste de ressources, saisissez **Log Analytics**. Au fur et à mesure de la saisie, la liste est filtrée. Sélectionnez **Log Analytics**.  

    ![Portail Azure](./media/log-analytics-quick-collect-azurevm/azure-portal-01.png)  

3. Dans le volet des abonnements Log Analytics, sélectionnez un espace de travail.

4. La page de l’espace de travail affiche des détails sur le démarrage et la configuration, ainsi que des liens vers des informations supplémentaires.  

    ![Détails de l’espace de travail](./media/log-analytics-manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Gérer les comptes et les utilisateurs
Chaque espace de travail peut être associé à plusieurs comptes et chaque compte peut également avoir accès à plusieurs espaces de travail. L’accès est géré via des [accès reposant sur un rôle Azure](../active-directory/role-based-access-control-configure.md). Ces droits d’accès s’appliquent à la fois sur le portail Azure et sur l’accès à l’API.


Les activités suivantes nécessitent également des autorisations Azure :

| Action                                                          | Autorisations Azure nécessaires | Notes |
|-----------------------------------------------------------------|--------------------------|-------|
| Ajout et suppression de solutions de gestion                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Ces autorisations doivent être accordées au niveau du groupe de ressources ou de l’abonnement. |
| Modification du niveau tarifaire                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Affichage des données dans les mosaïques de solution *Sauvegarde* et *Site Recovery* | Administrateur/coadministrateur | Accède aux ressources déployées à l’aide du modèle de déploiement Classic |
| Gestion d’un espace de travail dans le portail Azure                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Gestion de l’accès à Log Analytics à l’aide des autorisations Azure
Pour accorder l’accès à l’espace de travail Log Analytics à l’aide des autorisations Azure, suivez les étapes de la page [Utiliser les attributions de rôle pour gérer l’accès à vos ressources d’abonnement Azure](../active-directory/role-based-access-control-configure.md).

Azure intègre deux rôles utilisateur pour Log Analytics :
- Lecteur Log Analytics
- Contributeur Log Analytics

Les membres du rôle *Lecteur Log Analytics* peuvent effectuer les opérations suivantes :
- Visualisation et recherche de toutes les données d’analyse 
- Visualisation des paramètres d’analyse, notamment la configuration des diagnostics Azure sur toutes les ressources Azure

Le rôle Lecteur Log Analytics inclut les actions Azure suivantes :

| type    | Autorisation | Description |
| ------- | ---------- | ----------- |
| Action | `*/read`   | Possibilité de visualiser toutes les ressources Azure et la configuration des ressources. Inclut la visualisation des éléments suivants : <br> État d’extension de machine virtuelle <br> Configuration des diagnostics Azure sur les ressources <br> Totalité des paramètres et propriétés de l’ensemble des ressources |
| Action | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Possibilité d’exécuter des requêtes à l’aide de la fonction Recherche dans les journaux v2 |
| Action | `Microsoft.OperationalInsights/workspaces/search/action` | Possibilité d’exécuter des requêtes à l’aide de la fonction Recherche dans les journaux v1 |
| Action | `Microsoft.Support/*` | Possibilité d’ouvrir des cas de support |
|Non-action | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Interdiction de lecture de la clé d’espace de travail requise pour l’utilisation de l’API de collecte de données et pour l’installation des agents. Cela empêche l’utilisateur d’ajouter de nouvelles ressources à l’espace de travail. |


Les membres du rôle *Contributeur Log Analytics* peuvent effectuer les opérations suivantes :
- Lecture de toutes les données de surveillance avec le rôle Lecteur Log Analytics  
- Création et configuration des comptes Automation  
- Ajout et suppression de solutions de gestion    
    > [!NOTE] 
    > Pour réussir ces deux actions, cette autorisation doit être accordée au niveau du groupe de ressources ou de l’abonnement.  

- Lecture des clés de compte de stockage   
- Configuration de la collecte de journaux à partir du stockage Azure  
- Modification des paramètres d’analyse pour les ressources Azure, notamment :
  - Ajout de l’extension de machine virtuelle à des machines virtuelles
  - Configuration des diagnostics Azure sur toutes les ressources Azure

> [!NOTE] 
> La possibilité d’ajouter une extension de machine virtuelle à une machine virtuelle vous offre un contrôle total sur une machine virtuelle.

Le rôle Contributeur Log Analytics inclut les actions Azure suivantes :

| Autorisation | Description |
| ---------- | ----------- |
| `*/read`     | Possibilité de visualiser toutes les ressources et la configuration des ressources. Inclut la visualisation des éléments suivants : <br> État d’extension de machine virtuelle <br> Configuration des diagnostics Azure sur les ressources <br> Totalité des paramètres et propriétés de l’ensemble des ressources |
| `Microsoft.Automation/automationAccounts/*` | Possibilité de créer et configurer les comptes Azure Automation, et notamment d’ajouter et modifier des runbooks |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Ajout, mise à jour et suppression d’extensions de machine virtuelle, notamment l’extension Microsoft Monitoring Agent et l’extension Agent OMS pour Linux |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Visualisation de la clé du compte de stockage ; opération requise pour la configuration de Log Analytics pour la lecture des journaux à partir des comptes de stockage Azure |
| `Microsoft.Insights/alertRules/*` | Ajout, mise à jour et suppression de règles d’alerte |
| `Microsoft.Insights/diagnosticSettings/*` | Ajout, mise à jour et suppression de paramètres de diagnostic sur les ressources Azure |
| `Microsoft.OperationalInsights/*` | Ajout, mise à jour et suppression de configuration pour les espaces de travail Log Analytics |
| `Microsoft.OperationsManagement/*` | Ajout et suppression de solutions de gestion |
| `Microsoft.Resources/deployments/*` | Création et suppression de déploiements ; opération requise pour l’ajout et la suppression de solutions, d’espaces de travail et de comptes Automation |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Création et suppression de déploiements ; opération requise pour l’ajout et la suppression de solutions, d’espaces de travail et de comptes Automation |

L’ajout et la suppression d’utilisateurs au niveau d’un rôle utilisateur requièrent les autorisations `Microsoft.Authorization/*/Delete` et `Microsoft.Authorization/*/Write`.

Utilisez ces rôles pour accorder aux utilisateurs l’accès à différentes étendues :
- Abonnement : accès à tous les espaces de travail de l’abonnement
- Groupe de ressources : accès à tous les espaces de travail du groupe de ressources
- Ressource : accès uniquement à l’espace de travail spécifié

Nous vous recommandons d’effectuer des affectations au niveau de la ressource (espace de travail) afin d’assurer un contrôle d’accès précis.  Pour créer des rôles avec les autorisations spécifiques nécessaires, utilisez des [rôles personnalisés](../active-directory/role-based-access-control-custom-roles.md).

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Liaison d’un espace de travail existant à un abonnement Azure
Tous les espaces de travail créés après le 26 septembre 2016 doivent être liés à un abonnement Azure lors de la création. Vous devez lier les espaces de travail créés avant cette date lorsque vous vous connectez. Lorsque vous créez l’espace de travail à partir du portail Azure ou que vous liez votre espace de travail à un abonnement Azure, votre répertoire Azure Active Directory est lié en tant que compte d’organisation.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Pour lier un espace de travail à un abonnement Azure dans le portail Azure
1. Dans le portail Azure, cliquez sur **Tous les services**. Dans la liste de ressources, saisissez **Log Analytics**. Au fur et à mesure de la saisie, la liste est filtrée. Sélectionnez **Log Analytics**.  

2. Dans le volet des abonnements Log Analytics, cliquez sur **Ajouter**.  

    ![liste des espaces de travail](./media/log-analytics-manage-access/workspace-link-existing-01.png)

3. À partir du volet **Espace de travail Log Analytics**, cliquez sur **Lier l’existant**.  

4. Cliquez sur **Configurer les paramètres requis**.  

5. Vous voyez la liste des espaces de travail qui ne sont pas encore liés à votre compte Azure. Sélectionnez l’espace de travail.  
   
6. Si nécessaire, vous pouvez modifier les valeurs correspondant aux éléments suivants :
   * Abonnement
   * Groupe de ressources
   * Lieu
   * Niveau tarifaire  

7. Cliquez sur **OK**. L’espace de travail est maintenant lié à votre compte Azure.

> [!NOTE]
> Si l’espace de travail que vous souhaitez lier ne s’affiche pas, cela signifie que votre abonnement Azure n’a pas accès à l’espace de travail que vous avez créé sur le portail OMS.  Pour accorder l’accès à ce compte à partir du portail OMS, consultez [Ajout d’un utilisateur à un espace de travail existant](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Mise à niveau de l’espace de travail vers un plan payant
Il existe trois types de plan pour les espaces de travail OMS : **Gratuit**, **Autonome** et **OMS**.  Si vous êtes abonné au niveau *Gratuit*, vous pouvez envoyer au maximum 500 Mo de données par jour à Log Analytics.  Si vous dépassez ce volume, vous devez migrer votre espace de travail vers un plan payant pour pouvoir collecter des données au-delà de cette limite. Vous pouvez convertir votre type de plan à tout moment.  Pour en savoir plus sur la tarification d’OMS, consultez la rubrique relative aux [détails de tarification](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Utilisation de droits dans le cadre d’un abonnement OMS
Pour utiliser les droits que vous obtenez à l’achat d’un plan OMS E1, OMS E2 ou Complément OMS pour System Center, choisissez le plan *OMS* d’OMS Log Analytics.

Lorsque vous achetez un abonnement OMS, les droits sont ajoutés à votre Contrat Entreprise. Tout abonnement Azure créé dans le cadre de ce contrat peut faire valoir ces droits. Tous les espaces de travail sur ces abonnements utilisent les droits OMS.

Pour vous assurer que l’utilisation d’un espace de travail est appliquée à vos droits à partir de l’abonnement OMS, vous devez :

1. Créer votre espace de travail dans un abonnement Azure qui fait partie du Contrat Entreprise incluant l’abonnement OMS

2. Sélectionner le plan *OMS* pour l’espace de travail

> [!NOTE]
> Si votre espace de travail a été créé avant le 26 septembre 2016 et que votre plan de tarification Log Analytics est *Premium*, cet espace de travail utilise les droits du complément OMS pour System Center. Vous pouvez également utiliser vos droits en migrant au niveau de tarification *OMS*.
>
>

Les droits de l’abonnement OMS ne sont pas visibles dans le portail Azure. Vous pouvez les consulter, ainsi que l’utilisation, dans le portail Enterprise Portal.  

Si vous devez modifier l’abonnement Azure auquel votre espace de travail est lié, vous pouvez utiliser l’applet de commande Azure PowerShell [Move-AzureRMResource](https://msdn.microsoft.com/library/mt652516.aspx) .

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Utilisation de l’engagement Azure d’un Contrat Entreprise
Si vous n’avez pas d’abonnement OMS, vous payez séparément pour chaque composant OMS et l’utilisation apparaît sur votre facture Azure.

Si vous disposez d’un engagement monétaire Azure dans le cadre de l’inscription d’entreprise à laquelle vos abonnements Azure sont liés, l’utilisation de Log Analytics est automatiquement débitée de votre engagement monétaire restant.

Si vous devez modifier l’abonnement Azure auquel l’espace de travail est lié, vous pouvez utiliser l’applet de commande Azure PowerShell [Move-AzureRMResource](https://msdn.microsoft.com/library/mt652516.aspx) .  

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-azure-portal"></a>Migrer un espace de travail vers un niveau tarifaire payant dans le portail Azure
1. Dans le portail Azure, à partir du volet des abonnements Log Analytics, sélectionnez un espace de travail.

2. Dans le volet de l’espace de travail, sous **Général**, sélectionnez **Niveau tarifaire**.  

3. Sous **Niveau tarifaire**, sélectionnez un niveau tarifaire et cliquez sur **Sélectionner**.  
    ![Plan tarifaire sélectionné](./media/log-analytics-manage-access/workspace-pricing-tier-info.png)

> [!NOTE]
> Si votre espace de travail est lié à un compte Automation, avant de pouvoir sélectionner le niveau tarifaire *Autonome (par Go)*, vous devez supprimer les solutions **Automation and Control** et annuler la liaison avec le compte Automation. Dans le panneau Espace de travail sous **Général**, cliquez sur **Solutions** pour afficher et supprimer des solutions. Pour annuler la liaison avec le compte Automation, cliquez sur le nom du compte Automation dans le panneau **Niveau tarifaire**.
>
>

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-oms-portal"></a>Migrer un espace de travail vers un niveau tarifaire payant dans le portail OMS

Pour modifier le niveau tarifaire à l’aide du portail OMS, vous devez posséder un abonnement Azure.

1. Dans le portail OMS, cliquez sur la vignette **Paramètres**.

2. Cliquez sur l’onglet **Comptes**, puis sur l’onglet **Azure Subscription & Data Plan** (Forfait de données et abonnement Azure).

3. Cliquez sur le niveau tarifaire que vous souhaitez utiliser.

4. Cliquez sur **Enregistrer**.  

    ![abonnement et forfaits de données](./media/log-analytics-manage-access/subscription-tab.png)

Votre nouveau forfait de données s’affiche dans le ruban du portail OMS, en haut de votre page web.

![Ruban OMS](./media/log-analytics-manage-access/data-plan-changed.png)

## <a name="next-steps"></a>Étapes suivantes
* Consultez [Collecter des données d’ordinateurs dans un environnement avec Log Analytics](log-analytics-concept-hybrid.md) pour rassembler des données d’ordinateurs de votre centre de données ou d’un autre environnement cloud.
* Consultez [Collecter des données sur les machines virtuelles Azure](log-analytics-quick-collect-azurevm.md) pour configurer la collecte de données à partir de machines virtuelles Azure.  
* [Ajoutez des solutions Log Analytics à partir de la galerie de solutions](log-analytics-add-solutions.md) pour ajouter des fonctionnalités et collecter des données.

