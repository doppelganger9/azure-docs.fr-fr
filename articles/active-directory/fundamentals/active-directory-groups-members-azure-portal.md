---
title: Gérer les membres d’un groupe dans Azure AD | Microsoft Docs
description: Comment ajouter ou supprimer des utilisateurs et des appareils dans un groupe d’Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 947b0c11aba211530e3ae25d6617079bcaf2995f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860397"
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Gérer l’appartenance à un groupe des utilisateurs dans votre client Azure Active Directory
Cet article explique comment gérer les membres d’un groupe dans Azure Active Directory (Azure AD).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Comment trouver les membres et les gérer ?
1. Connectez-vous au [portail Azure](https://portal.azure.com) en utilisant un compte d’administrateur général pour le répertoire.
2. Sélectionnez **Tous les services**, entrez **Utilisateurs et groupes** dans la zone de texte, puis sélectionnez **Entrée**.

   ![Ouvrir la gestion des utilisateurs](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Dans le panneau **Utilisateurs et groupes**, sélectionnez **Tous les groupes**.

   ![Ouvrir le panneau de groupes](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Dans le panneau **Utilisateurs et groupes - Tous les groupes** , sélectionnez un groupe.
5. Dans le panneau **Groupe - *NomGroupe*** , sélectionnez **Membres**.

   ![Ouverture du panneau Membres](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Pour ajouter des membres au groupe, dans le panneau **Groupe - Membres**, sélectionnez **Ajouter des membres**.

   ![Commande Ajouter des membres](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Dans le panneau **Membres**, sélectionnez un ou plusieurs utilisateurs ou appareils à ajouter au groupe, puis cliquez sur le **Sélectionner** en bas du panneau pour les ajouter au groupe. La zone **Utilisateur** filtre l’affichage en fonction de la correspondance de votre entrée avec une partie ou l’intégralité d’un nom d’utilisateur ou d’appareil. Dans cette zone aucun caractère générique n’est accepté.
8. Pour supprimer des membres du groupe, dans le panneau **Groupe - Membres** , sélectionnez un membre.
9. Dans le panneau ***NomMembre***, sélectionnez la commande **Supprimer**, puis confirmez votre choix dans l’invite de commandes.

   ![Commande Supprimer des membres](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Lorsque vous avez terminé la modification des membres du groupe, sélectionnez **Enregistrer**.

## <a name="additional-information"></a>Informations supplémentaires
Ces articles fournissent des informations supplémentaires sur Azure Active Directory.

* [Consulter les groupes existants](active-directory-groups-view-azure-portal.md)
* [Création d’un nouveau groupe et ajout de membres](active-directory-groups-create-azure-portal.md)
* [Gérer les paramètres d’un groupe](active-directory-groups-settings-azure-portal.md)
* [Gérer l’appartenance à un groupe](active-directory-groups-membership-azure-portal.md)
* [Gérer les règles dynamiques pour les utilisateurs dans un groupe](../users-groups-roles/groups-dynamic-membership.md)
