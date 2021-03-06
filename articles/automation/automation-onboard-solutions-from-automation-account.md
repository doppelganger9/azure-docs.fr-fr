---
title: Découvrez comment intégrer les solutions Update Management, Change Tracking et Inventory dans Azure Automation.
description: Découvrez comment intégrer une machine virtuelle Azure avec Update Management, Change Tracking et Inventory qui font partie d’Azure Automation.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 8649b96c9cf95e4a25b24dedf447aef133ef299a
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865401"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Intégrer les solutions Update Management, Change Tracking et Inventory

Azure Automation fournit des solutions pour gérer les mises à jour de sécurité du système d’exploitation, le suivi des modifications et l’inventaire de ce qui est installé sur vos ordinateurs. Vous pouvez intégrer des machines de plusieurs façons : vous pouvez intégrer la solution [à partir d’une machine virtuelle](automation-onboard-solutions-from-vm.md), [par la navigation sur plusieurs machines](automation-onboard-solutions-from-browse.md), à partir de votre compte Automation ou par [runbook](automation-onboard-solutions.md). Cet article traite de l’intégration de ces solutions à partir de votre compte Automation.

## <a name="log-in-to-azure"></a>Connexion à Azure

Connectez-vous à Azure à l’adresse https://portal.azure.com.

## <a name="enable-solutions"></a>Activer des solutions

Accédez à votre compte Automation et sélectionnez **Inventory** ou **Change Tracking** sous **GESTION DE LA CONFIGURATION**.

Choisissez l’espace de travail Log Analytics et un compte Automation, puis cliquez sur **Activer** pour activer la solution. L’activation de la solution prend jusqu’à 15 minutes.

![Intégrer la solution Inventory](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

La solution Change Tracking et Inventory fournit la possibilité de [suivre les changements](automation-vm-change-tracking.md) et de [faire l’inventaire](automation-vm-inventory.md) de vos machines virtuelles. Dans cette étape, vous activez la solution sur une machine virtuelle.

Quand vous recevez la notification d’intégration de la solution Change Tracking et Inventory, cliquez sur **Update Management** sous **GESTION DE LA CONFIGURATION**.

La solution Update Management vous permet de gérer les mises à jour et les correctifs pour vos machines virtuelles Microsoft Azure. Vous pouvez évaluer l’état des mises à jour disponibles, planifier l’installation des mises à jour obligatoires et examiner les résultats des déploiements pour vérifier que les mises à jour ont été appliquées à la machine virtuelle. Cette action a permis d’activer la solution pour votre machine virtuelle.

Sélectionnez **Update Management** sous **UPDATE MANAGEMENT**. L’espace de travail Log Analytics sélectionné est le même que celui utilisé à l’étape précédente. Cliquez sur **Activer** pour intégrer la solution Update Management. L’activation de la solution prend jusqu’à 15 minutes.

![Intégrer la solution Update](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Configuration d’étendue

Chaque solution utilise une configuration d’étendue au sein de l’espace de travail pour cibler les ordinateurs qui obtiennent la solution. La configuration d’étendue est un groupe de recherches enregistrées utilisé pour limiter l’étendue de la solution à des ordinateurs spécifiques. Pour accéder aux configurations d’étendue, dans votre compte Automation sous **RESSOURCES CONNEXES**, sélectionnez **Espace de travail**. Ensuite, dans l’espace de travail sous **SOURCES DE DONNÉES DE L’ESPACE DE TRAVAIL**, sélectionnez **Configurations des étendues**.

Si l’espace de travail sélectionné n’a pas encore les solutions de gestion des mises à jour ou de suivi des modifications, les configurations de portée suivantes sont créées :

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Si l’espace de travail sélectionné a déjà la solution. La solution n’est pas de nouveau déployée et la configuration d’étendue ne lui est pas ajoutée à.

## <a name="saved-searches"></a>Recherches enregistrées

Quand un ordinateur est ajouté aux solutions Update Management, Change Tracking et Inventaire, il est ajouté à l’une des deux recherches enregistrées dans votre espace de travail. Ces recherches enregistrées sont des requêtes qui contiennent les ordinateurs ciblés pour ces solutions.

Accédez à votre compte Automation et sélectionnez **Recherches enregistrées** sous **Ggénéral**. Les deux recherches enregistrées utilisées par ces solutions sont présentées dans le tableau suivant :

|NOM     |Catégorie  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Mises à jour        | Updates__MicrosoftDefaultComputerGroup         |

Sélectionnez l’une des deux recherches enregistrées pour afficher la requête utilisée pour remplir le groupe. L’image suivante montre la requête et ses résultats :

![Recherches enregistrées](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Intégrer des machines virtuelles Azure

À partir de votre compte Automation, sélectionnez **Inventory** ou **Change Tracking** sous **GESTION DE LA CONFIGURATION**, ou **Update Management** sous **UPDATE MANAGEMENT**.

Cliquez sur **+ Ajouter des machines virtuelles Azure**, sélectionnez une ou plusieurs machines virtuelles dans la liste. Les machines virtuelles qui ne peuvent pas être activées sont grisées et ne peuvent pas être sélectionnées. Dans la page **Activer Update Management**, cliquez sur **Activer**. Cette opération ajoute les machines virtuelles sélectionnées à la recherche enregistrée de groupe d’ordinateurs pour la solution.

![Activer les machines virtuelles Azure](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Intégrer une machine non-Azure

Les machines qui ne sont pas dans Azure doivent être ajoutées manuellement. À partir de votre compte Automation, sélectionnez **Inventory** ou **Change Tracking** sous **GESTION DE LA CONFIGURATION**, ou **Update Management** sous **UPDATE MANAGEMENT**.

Cliquez sur **Ajouter une machine virtuelle non-Azure**. Une nouvelle fenêtre de navigateur s’ouvre et donne des [instructions pour installer et configurer Microsoft Monitoring Agent sur la machine](../log-analytics/log-analytics-concept-hybrid.md) afin que celle-ci puisse commencer à créer des rapports dans la solution. Si vous intégrez une machine actuellement gérée par System Center Operations Manager, l’installation d’un nouvel agent n’est pas nécessaire, car les informations de l’espace de travail sont entrées dans l’agent existant.

## <a name="onboard-machines-in-the-workspace"></a>Intégrer des machines dans l’espace de travail

Les machines installées manuellement ou les machines déjà connectées à votre espace de travail doivent être ajoutées à Azure Automation pour que la solution soit activée. À partir de votre compte Automation, sélectionnez **Inventory** ou **Change Tracking** sous **GESTION DE LA CONFIGURATION**, ou **Update Management** sous **UPDATE MANAGEMENT**.

Sélectionnez **Gérer des machines**. La page **Gérer des machines** s’ouvre. Cette page vous permet soit d’activer la solution sur un ensemble sélectionné de machines ou sur toutes les machines disponibles, soit d’activer la solution pour toutes les machines actuelles et de l’activer sur toutes les futures machines.

![Recherches enregistrées](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Toutes les machines disponibles

Pour activer la solution sur toutes les machines disponibles, sélectionnez **Activer sur toutes les machines disponibles**. La commande permettant d’ajouter des machines individuellement est alors désactivée. Cette tâche ajoute tous les noms des machines envoyant des informations à l’espace de travail dans la requête de recherche enregistrée de groupe d’ordinateurs.

### <a name="all-available-and-future-machines"></a>Toutes les machines disponibles et futures

Pour activer la solution sur toutes les machines disponibles et toutes les futures machines, sélectionnez **Activer sur toutes les machines disponibles et futures**. Cette option supprime les recherches enregistrées et les configurations d’étendue de l’espace de travail. Elle ouvre la solution à toutes les machines Azure et non-Azure qui envoient des informations à l’espace de travail.

### <a name="selected-machines"></a>Machines sélectionnées

Pour activer la solution sur une ou plusieurs machines, sélectionnez **Activer sur les machines sélectionnées** et cliquez sur **Ajouter** en regard de chaque machine à ajouter à la solution. Cette tâche ajoute les noms de machine sélectionnés à la requête de recherche enregistrée de groupe d’ordinateurs pour la solution.

## <a name="next-steps"></a>Étapes suivantes

Poursuivez avec les tutoriels sur les solutions pour apprendre à les utiliser.

* [Tutoriel - Gérer les mises à jour pour votre machine virtuelle](automation-tutorial-update-management.md)

* [Tutoriel - Identifier les logiciels sur une machine virtuelle](automation-tutorial-installed-software.md)

* [Tutoriel - Résoudre les modifications sur une machine virtuelle](automation-tutorial-troubleshoot-changes.md)