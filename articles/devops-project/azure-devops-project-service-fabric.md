---
title: Déployer votre application ASP.NET Core dans Azure Service Fabric à l’aide d’Azure DevOps Project | Didacticiel VSTS
description: DevOps Project facilite la prise en main d’Azure. Azure DevOps Project permet de déployer facilement votre application ASP.NET Core à l’aide d’Azure Service Fabric en quelques étapes rapides.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 2fd1fc968eb6b61d7378dbc0efa48f2f7eb2aa54
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967327"
---
# <a name="tutorial--deploy-your-aspnet-core-app-to-azure-service-fabric-with-the-azure-devops-project"></a>Didacticiel : Déployer votre application ASP.NET Core dans Azure Service Fabric à l’aide d’Azure DevOps Project

Azure DevOps Project présente une expérience simplifiée où vous apportez votre code et référentiel Git existants, ou choisissez parmi les exemples d’applications pour créer un pipeline d’intégration continue (CI) et de livraison continue (CD) vers Azure.  DevOps Project crée automatiquement des ressources Azure telles qu’Azure Service Fabric, crée et configure un pipeline de mise en production dans VSTS comprenant une définition de build pour l’intégration continue, configure une définition de mise en production pour le déploiement continu, puis crée une ressource Azure Application Insights pour la surveillance.

Vous allez effectuer les étapes suivantes :

> [!div class="checklist"]
> * Créer un projet Azure DevOps pour une application ASP.NET Core et Service Fabric
> * Configurer VSTS et un abonnement Azure 
> * Examiner la définition de build d’intégration continue (CI) VSTS
> * Examiner la définition de la gestion des mises en production du déploiement continu (CD) VSTS
> * Valider les modifications apportées à VSTS et les déployer automatiquement dans Azure
> * Supprimer les ressources

## <a name="prerequisites"></a>Prérequis

* Un abonnement Azure. Vous pouvez en obtenir un gratuit via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-an-azure-devops-project-for-an-aspnet-core-app-and-service-fabric"></a>Créer un projet Azure DevOps pour une application ASP.NET Core et Service Fabric

Azure DevOps Project crée un pipeline CI/CD dans VSTS.  Vous pouvez créer un **compte VSTS** ou utiliser un **compte existant**.  Azure DevOps Project crée également des **ressources Azure** telles qu’un cluster Service Fabric dans **l’abonnement Azure** de votre choix.

1. Connectez-vous au [portail Microsoft Azure](https://portal.azure.com).

1. Sélectionnez l’icône **Créer une ressource** dans la barre de navigation gauche, puis recherchez **Projet DevOps**.  Cliquez sur **Créer**.

    ![Démarrage de la livraison continue](_img/azure-devops-project-service-fabric/fullbrowser.png)

1. Sélectionnez **.NET**, puis **Suivant**.

1. Pour **Choisir une infrastructure d’application**, sélectionnez **ASP.NET**, puis **Suivant**.

1. Sélectionnez **Cluster Service Fabric**, puis **Suivant**.  

## <a name="configure-vsts-and-an-azure-subscription"></a>Configurer VSTS et un abonnement Azure

1. Créez un **nouveau** compte VSTS ou choisissez un compte **existant**.  Choisissez un **nom** pour votre projet VSTS.  

1. Sélectionnez votre **abonnement Azure**.

1. Sélectionnez le lien **Modifier** pour afficher d’autres paramètres de configuration Azure et identifier la **taille de la machine virtuelle du nœud** et le **système d’exploitation** pour le **cluster Service Fabric**.  Ici, différentes options permettent de configurer le type et l’emplacement des services Azure.
 
1. Quittez la zone de configuration Azure, puis sélectionnez **Terminé**.

1. Ce processus peut prendre plusieurs minutes.  Un exemple d’application ASP.NET Core est configuré dans un référentiel dans votre compte VSTS, un cluster Service Fabric est créé, une build et une mise en production sont exécutées, et votre application est déployée dans Azure.  

    Lorsque vous avez terminé, le **tableau de bord du projet** Azure DevOps est chargé dans le portail Azure.  Vous pouvez également accéder au **tableau de bord Azure DevOps Project** directement à partir de **Toutes les ressources** dans le **portail Azure**.  

    Ce tableau de bord permet de visualiser votre **référentiel de code** VSTS, votre **pipeline VSTS CI/CD** et le **cluster Service Fabric**.  Vous pouvez configurer des options supplémentaires dans VSTS.  Sur le côté droit du tableau de bord, sélectionnez **Parcourir** pour afficher votre application en cours d’exécution.

## <a name="examine-the-vsts-ci-build-definition"></a>Examiner la définition de build d’intégration continue (CI) VSTS

Azure DevOps Project configure automatiquement un pipeline VSTS CI/CD complet dans votre compte VSTS.  Vous pouvez explorer et personnaliser le pipeline.  Suivez les étapes ci-dessous pour vous familiariser avec la définition de build VSTS.

1. Accédez au **tableau de bord Azure DevOps Project**.

1. Sélectionnez **Pipelines de build** en **haut** du **tableau de bord Azure DevOps Project**.  Ce lien ouvre un onglet de navigateur ainsi que la définition de build VSTS pour votre nouveau projet.

1. Déplacez le curseur de souris vers la droite de la définition de build en regard du champ **État**. Sélectionnez les **points de suspension** affichés.  Cette action ouvre un menu où vous pouvez démarrer plusieurs activités, telles que **mettre une nouvelle build en file d’attente**, **suspendre une build**, et **modifier une définition de build**.

1. Sélectionnez **Modifier**.

1. Dans cette vue, **examinez les différentes tâches** de votre définition de build.  La build exécute différentes tâches, telles que la récupération (fetch) de sources à partir du référentiel Git VSTS, la restauration de dépendances et la publication de sorties utilisées pour les déploiements.

1. En haut de la définition de build, sélectionnez le **nom de la définition de build**.

1. Modifiez le **nom** de votre définition de build pour le remplacer par un nom plus descriptif.  Sélectionnez **Enregistrer et mettre en file d’attente**, puis **Enregistrer**.

1. Sous le nom de votre définition de build, sélectionnez **Historique**.  Vous pouvez observer une piste d’audit des modifications que vous avez apportées récemment à la build.  VSTS suit les modifications apportées à la définition de build et vous permet de comparer les versions.

1. Sélectionnez **Déclencheurs**.  Azure DevOps Project a créé automatiquement un déclencheur d’intégration continue, et chaque validation dans le référentiel démarre une nouvelle build.  Vous pouvez éventuellement choisir d’inclure ou d’exclure des branches dans le processus d’intégration continue.

1. Sélectionnez **Rétention**.  Selon votre scénario, vous pouvez spécifier des stratégies pour conserver ou supprimer un certain nombre de builds.

## <a name="examine-the-vsts-cd-release-management-definition"></a>Examiner la définition de la gestion des mises en production du déploiement continu (CD) VSTS

Azure DevOps Project crée et configure automatiquement les étapes nécessaires au déploiement à partir de votre compte VSTS vers votre abonnement Azure.  Ces étapes comprennent la configuration d’une connexion au service Azure pour authentifier VSTS auprès de votre abonnement Azure.  L’automatisation crée également une définition de mise en production VSTS. Cette mise en production apporte le déploiement continu à Azure.  Suivez les étapes ci-dessous pour examiner de plus près la définition de mise en production VSTS.

1. Sélectionnez **Build et mise en production**, puis **Versions**.  Azure DevOps Project a créé une définition de mise en production VSTS pour gérer les déploiements dans Azure.

1. Sur le côté gauche du navigateur, sélectionnez les **points de suspension** en regard de votre définition de mise en production, puis **Modifier**.

1. La définition de mise en production contient un **pipeline**, qui définit le processus de mise en production.  Sous **Artefacts**, sélectionnez **Déposer**.  La définition de build que vous avez examinée dans les étapes précédentes génère la sortie utilisée pour l’artefact. 

1. À droite de l’icône **Déposer**, sélectionnez **l’icône** **Déclencheur de déploiement continu** (qui s’affiche sous la forme d’un éclair).  Cette définition de mise en production comporte un déclencheur de déploiement continu activé.  Le déclencheur démarre un déploiement à chaque fois qu’un nouvel artefact de build est disponible.  Si vous le souhaitez, vous pouvez désactiver le déclencheur afin que vos déploiements nécessitent une exécution manuelle. 

1. À droite du navigateur, sélectionnez **Afficher les mises en production**.  Cette vue affiche un historique des mises en production.

1. Sélectionnez les **points de suspension** situés en regard de vos mises en production, puis **Ouvrir**.  Il y a plusieurs menus à explorer dans cette vue, comme un **résumé des mises en production**, les **éléments de travail associés** et les **tests**.

1. Sélectionnez **Validations**.  Cette vue présente les validations de code associées au déploiement spécifique. Vous pouvez comparer les mises en production pour afficher les différences de validation entre les déploiements.

1. Sélectionnez **Journaux**.  Les journaux contiennent des informations utiles sur le processus de déploiement.  Ils peuvent être affichés pendant et après les déploiements.

## <a name="commit-changes-to-vsts-and-automatically-deploy-to-azure"></a>Valider les modifications apportées à VSTS et les déployer automatiquement dans Azure 

 > [!NOTE]
 > Les étapes ci-dessous permettent de tester le pipeline CI/CD en modifiant simplement le texte de votre application web.

Vous êtes maintenant prêt à collaborer avec une équipe sur votre application avec un processus CI/CD qui déploie automatiquement votre dernier travail sur votre site web.  Chaque modification apportée au référentiel Git VSTS démarre une build dans VSTS, et une définition de gestion des mises en production VSTS déploie vos modifications dans Azure.  Suivez les étapes ci-dessous ou utilisez d’autres techniques pour valider les modifications apportées à votre référentiel.  Par exemple, vous pouvez **cloner** le référentiel Git dans votre outil ou votre environnement de développement intégré favori, puis envoyer (push) les modifications vers ce référentiel.

1. Dans le menu VSTS, sélectionnez **Code**, puis **Fichiers**, puis accédez à votre référentiel.
1. Accédez au répertoire **Views\Home**, sélectionnez les **points de suspension** situés en regard du fichier **Index.cshtml**, puis choisissez **Modifier**.

1. Modifiez le fichier, par exemple le texte situé dans l’une des **balises div**.  En haut à droite, sélectionnez **Valider**.  Sélectionnez de nouveau **Valider** pour envoyer (push) votre modification. 

1. En quelques instants, une **build est démarrée dans VSTS**, puis une mise en production est exécutée pour déployer les modifications.  Vous pouvez surveiller **l’état de la build** avec le tableau de bord DevOps Project ou dans le navigateur avec votre compte VSTS.

1. Une fois la mise en production terminée, **actualisez votre application** dans le navigateur pour vérifier les modifications apportées.

## <a name="clean-up-resources"></a>Supprimer les ressources

 > [!NOTE]
 > Les étapes ci-dessous vous permettront de supprimer définitivement les ressources.  Utilisez cette fonctionnalité uniquement après avoir lu attentivement les invites.

Si vous êtes en phase de test, vous pouvez nettoyer les ressources afin d’éviter une augmentation des frais de facturation.  Lorsque vous n’en avez plus besoin, vous pouvez supprimer le cluster Azure Service Fabric et les ressources associées dans ce didacticiel à l’aide de la fonctionnalité **Supprimer** sur le tableau de bord Azure DevOps Project.  **Soyez prudent**, car la fonctionnalité de suppression détruit les données créées par Azure DevOps Project dans Azure et VSTS, et vous ne serez plus en mesure de les récupérer par la suite.

1. Dans le **portail Azure**, accédez à **Azure DevOps Project**.
2. Dans la partie **supérieure droite** du tableau de bord, sélectionnez **Supprimer**.  Après avoir lu l’invite, sélectionnez **Oui** pour **supprimer définitivement** les ressources.

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez éventuellement modifier ces définitions afin qu’elles répondent aux besoins de votre équipe. Vous pouvez également utiliser ce modèle CI/CD en tant que modèle pour vos autres projets.  Vous avez appris à effectuer les actions suivantes :

> [!div class="checklist"]
> * Créer un projet Azure DevOps pour une application ASP.NET Core et Service Fabric
> * Configurer VSTS et un abonnement Azure 
> * Examiner la définition de build d’intégration continue (CI) VSTS
> * Examiner la définition de la gestion des mises en production du déploiement continu (CD) VSTS
> * Valider les modifications apportées à VSTS et les déployer automatiquement dans Azure
> * Supprimer les ressources

Pour en savoir plus sur Service Fabric et les microservices, voir ci-dessous :

> [!div class="nextstepaction"]
> [Utiliser une approche de microservices pour la conception d’applications](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
