---
title: Préparer des serveurs VMware locaux à la récupération d’urgence de machines virtuelles VMware vers Azure | Microsoft Docs
description: Découvrez comment préparer des serveurs VMware à la récupération d’urgence vers Azure avec le service Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: facf8895770f890bfbbef946a32cc681f685e998
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915200"
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Préparer des serveurs VMware locaux à la récupération d’urgence vers Azure

[Azure Site Recovery](site-recovery-overview.md) contribue à votre stratégie de récupération d’urgence et de continuité d’activité en garantissant le bon fonctionnement et la disponibilité de vos applications métier pendant les interruptions planifiées et non planifiées. Site Recovery gère et orchestre la récupération d’urgence des machines locales et des machines virtuelles Azure, notamment la réplication, le basculement et la récupération.

- Il s’agit du deuxième tutoriel dans une série qui vous montre comment configurer la récupération d’urgence sur Azure pour des machines virtuelles VMware locales. Dans le premier tutoriel, nous [avons configuré les composants Azure](tutorial-prepare-azure.md) nécessaires pour la récupération d’urgence de VMware.
- Les tutoriels sont conçus pour vous montrer le chemin de déploiement le plus simple pour un scénario. Ils utilisent les options par défaut lorsque cela est possible et n’affichent pas tous les paramètres et chemins d’accès possibles. 

Dans cet article, nous vous montrons comment préparer votre environnement VMware local lorsque vous souhaitez répliquer des machines virtuelles VMware vers Azure à l’aide d’Azure Site Recovery. Vous allez apprendre à effectuer les actions suivantes :

> [!div class="checklist"]
> * Préparer un compte sur le serveur vCenter ou sur l’hôte vSphere ESXi pour automatiser la détection de machines virtuelles
> * Préparer un compte pour l’installation automatique du service Mobilité sur les machines virtuelles VMware
> * Vérifier les exigences des serveurs et machines virtuelles VMware
> * Préparer la connexion aux machines virtuelles Azure après le basculement



## <a name="prepare-an-account-for-automatic-discovery"></a>Préparer un compte pour la découverte automatique

Site Recovery doit pouvoir accéder aux serveurs VMware pour :

- Détectez automatiquement les machines virtuelles. Au moins un compte en lecture seule est nécessaire.
- Orchestrer la réplication, le basculement et la restauration automatique. Vous avez besoin d’un compte pouvant exécuter des opérations telles que la création et la suppression de disques ou le démarrage de machines virtuelles.

Créez le compte comme suit :

1. Pour utiliser un compte dédié, créez un rôle au niveau du vCenter. Donnez un nom au rôle, tel que **Azure_Site_Recovery**.
2. Attribuez au rôle les autorisations résumées dans le tableau ci-dessous.
3. Créez un utilisateur sur le serveur vCenter ou sur l’hôte vSphere. Affectez le rôle à l’utilisateur.

### <a name="vmware-account-permissions"></a>Autorisations du compte VMware

**Tâche** | **Rôle/Autorisations** | **Détails**
--- | --- | ---
**Détection des machines virtuelles** | Au moins un utilisateur en lecture seule<br/><br/> Objet de centre de données -> Propager vers l’objet enfant, rôle = lecture seule | L’utilisateur est affecté au niveau du centre de données et a accès à tous les objets dans le centre de données.<br/><br/> Pour restreindre l’accès, attribuez le rôle **Aucun accès** avec l’objet **Propager vers enfant** aux objets enfants (hôtes vSphere, banques de données, machines virtuelles et réseaux).
**Réplication complète, basculement, restauration automatique** |  Créez un rôle (Azure_Site_Recovery) avec les autorisations nécessaires, puis attribuez le rôle à un utilisateur ou à un groupe d’utilisateurs VMware<br/><br/> Objet de centre de données -> Propager vers l’objet enfant, rôle = Azure_Site_Recovery<br/><br/> Banque de données -> Allouer de l’espace, parcourir la banque de données, opérations de fichier de bas niveau, supprimer le fichier, mettre à jour les fichiers de machine virtuelle<br/><br/> Réseau -> Attribution de réseau<br/><br/> Ressource -> Affecter les machines virtuelles au pool de ressources, migrer des machines virtuelles hors tension, migrer des machines virtuelles sous tension<br/><br/> Tâches -> Créer une tâche, Mettre à jour une tâche<br/><br/> Machine virtuelle -> Configuration<br/><br/> Machine virtuelle -> Interagir -> répondre à la question, connexion d’appareil, configurer un support de CD, configurer une disquette, mettre hors tension, mettre sous tension, installation des outils VMware<br/><br/> Machine virtuelle -> Stock -> Créer, inscrire, désinscrire<br/><br/> Machine virtuelle -> Approvisionnement -> Autoriser le téléchargement de machines virtuelles, autoriser le chargement de fichiers de machine virtuelle<br/><br/> Machine virtuelle -> Instantanés -> Supprimer les instantanés | L’utilisateur est affecté au niveau du centre de données et a accès à tous les objets dans le centre de données.<br/><br/> Pour restreindre l’accès, attribuez le rôle **Aucun accès** avec l’objet **Propager vers enfant** aux objets enfants (hôtes vSphere, banques de données, machines virtuelles et réseaux).

## <a name="prepare-an-account-for-mobility-service-installation"></a>Préparer un compte pour l’installation du service Mobilité

Le service Mobilité doit être installé sur les machines virtuelles que vous souhaitez répliquer. Site Recovery peut effectuer une installation de type push de ce service lorsque vous activez la réplication pour un ordinateur, ou vous pouvez l’installer manuellement, ou à l’aide des outils d’installation.

- Dans ce tutoriel, nous allons installer le service Mobilité avec l’installation de type push.
- Pour cette installation de type push, vous devez préparer un compte dont Site Recovery pourra se servir pour accéder à la machine virtuelle. Vous devez spécifier ce compte quand vous configurez la récupération d’urgence dans la console Azure.

Préparez le compte comme suit :

Préparez un domaine ou un compte local avec les autorisations nécessaires pour l’installation sur la machine virtuelle.

- **Machines virtuelles Windiows** : Pour une installation sur des machines virtuelles Windows, si vous n’utilisez pas un compte de domaine, désactivez le contrôle d’accès des utilisateurs distants sur la machine locale. Pour ce faire, dans le Registre > **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, ajoutez l’entrée DWORD **LocalAccountTokenFilterPolicy** avec une valeur de 1.
- **Machines virtuelles Linux** : Pour une installation sur des machines virtuelles Linux, préparez un compte racine sur le serveur Linux source.


## <a name="check-vmware-requirements"></a>Vérifier les conditions requises VMware

Vérifiez que les machines virtuelles et serveurs VMware respectent les conditions requises.

1. [Vérifiez](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers) les conditions requises pour les serveurs VMware.
2. Pour les machines virtuelles Linux, [vérifiez](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) les conditions requises en matière de système de fichiers et de stockage. 
3. Vérifiez la prise en charge du [réseau](vmware-physical-azure-support-matrix.md#network) et du [stockage](vmware-physical-azure-support-matrix.md#storage) locaux. 
4. Vérifiez ce qui est pris en charge pour la [mise en réseau Azure](vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), le [stockage](vmware-physical-azure-support-matrix.md#azure-storage) et le [calcul](vmware-physical-azure-support-matrix.md#azure-compute) après le basculement.
5. Les machines virtuelles locales que vous répliquez vers Azure doivent respecter les [exigences relatives aux machines virtuelles Azure](vmware-physical-azure-support-matrix.md#azure-vm-requirements).


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Préparer la connexion aux machines virtuelles Azure après le basculement

Après le basculement, imaginons que vous souhaitiez vous connecter à des machines virtuelles Azure à partir de votre réseau local.

Pour vous connecter à des machines virtuelles Windows à l’aide de RDP après le basculement, effectuez les opérations suivantes :

- **Accès à Internet**. Activez RDP sur la machine virtuelle locale avant le basculement. Vérifiez que des règles TCP et UDP sont ajoutées pour le profil **Public** et que RDP est autorisé dans **Pare-feu Windows** > **Applications autorisées** pour tous les profils.
- **Accès VPN de site à site** :
    - Activez RDP sur la machine virtuelle locale avant le basculement.
    - RDP doit être autorisé dans **Pare-feu Windows** -> **Applications et fonctionnalités autorisées** pour les réseaux **Domaine et Privé**.
    - Vérifiez que la stratégie SAN du système d’exploitation est définie sur **OnlineAll**. [Plus d’informations](https://support.microsoft.com/kb/3031135)
- Aucune mise à jour de Windows ne doit être en attente sur la machine virtuelle quand vous déclenchez un basculement. S’il y en a, vous ne pouvez pas vous connecter à la machine virtuelle avant la fin de la mise à jour.
- Sur la machine virtuelle Azure Windows, après le basculement, vérifiez les **Diagnostics de démarrage** pour afficher une capture d’écran de la machine virtuelle. Si vous ne pouvez pas vous connecter, vérifiez que la machine virtuelle est en cours d’exécution et lisez ces [conseils de résolution des problèmes](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Pour vous connecter à des machines virtuelles Linux à l’aide de SSH après le basculement, effectuez les opérations suivantes :

- Sur la machine locale, avant le basculement, vérifiez que le service Secure Shell est configuré pour démarrer automatiquement au démarrage du système.
- Vérifiez que les règles de pare-feu autorisent une connexion SSH.
- Sur la machine virtuelle Azure, après le basculement, autorisez les connexions entrantes au port SSH pour les règles des groupes de sécurité réseau sur la machine virtuelle basculée, et pour le sous-réseau Azure auquel elle est connectée.
- [Ajoutez une adresse IP publique](site-recovery-monitoring-and-troubleshooting.md) pour la machine virtuelle.
- Vous pouvez vérifier les **Diagnostics de démarrage** pour afficher une capture d’écran de la machine virtuelle.

## <a name="useful-links"></a>Liens utiles

Si vous répliquez plusieurs machines virtuelles, vous devez planifier la capacité et le déploiement avant de commencer. [Plus d’informations](site-recovery-deployment-planner.md)



## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Configurer la récupération d’urgence vers Azure pour des machines virtuelles VMware](vmware-azure-tutorial.md)
