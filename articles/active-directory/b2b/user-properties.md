---
title: Propriétés d’un utilisateur Azure Active Directory B2B Collaboration | Microsoft Docs
description: Les propriétés de l’utilisateur Azure Active Directory B2B Collaboration sont configurables
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5f999a17cd375a3338aa936e2f405c36f6021ebc
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984807"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Propriétés d’un utilisateur Azure Active Directory B2B Collaboration

Un utilisateur Azure Active Directory (Azure AD) B2B (interentreprises) Collaboration présente la propriété UserType = Invité. Cet utilisateur invité appartient généralement à une organisation partenaire et dispose par défaut de privilèges limités dans l’annuaire qui invite.

Selon les besoins de l’organisation qui émet l’invitation, un utilisateur Azure AD B2B Collaboration peut présenter l’un des états de compte suivants :

- État 1 : hébergé dans une instance externe d’Azure AD et représenté sous la forme d’un utilisateur invité de l’organisation à l’origine de l’invitation. Dans ce cas, l’utilisateur B2B se connecte avec un compte Azure AD qui appartient au locataire invité. Même si l’organisation partenaire n’utilise pas Azure AD, l’utilisateur invité d’Azure AD est créé. Les conditions requises sont les suivantes : les utilisateurs se servent de leur invitation et Azure AD vérifie leur adresse e-mail. Dans ce cas, on parle également de location juste-à-temps (JIT) ou de location « virale ».

- État 2 : hébergé dans un compte Microsoft et représenté en tant qu’utilisateur invité dans l’organisation hôte. Dans ce cas, l’utilisateur invité se connecte avec un compte Microsoft. L’identité sociale de l’utilisateur invité (google.com ou similaire), qui n’est pas un compte Microsoft, est créée en tant que compte Microsoft au cours de l’échange d’offre.

- État 3 : hébergé dans l’instance Active Directory locale de l’organisation hôte et synchronisé avec l’instance Azure AD de l’organisation hôte. Dans cette version, vous devez utiliser PowerShell pour modifier manuellement la valeur UserType de ces utilisateurs dans le cloud.

- État 4 : hébergé dans l’instance Azure AD de l’organisation hôte avec la propriété UserType = Invité et des informations d’identification gérées par l’organisation hôte.

  ![Affichage des initiales de l’inviteur](media/user-properties/redemption-diagram.png)


À présent, voyons à quoi ressemble un utilisateur Azure AD B2B Collaboration à l’état 1 dans Azure AD.

### <a name="before-invitation-redemption"></a>Avant l'utilisation de l'invitation

![Avant l’utilisation de l’offre](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Après l'utilisation de l'invitation

![Après l’utilisation de l’offre](media/user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Propriétés clés de l’utilisateur Azure AD B2B collaboration
### <a name="usertype"></a>UserType
Cette propriété indique la relation de l’utilisateur avec la location hôte. Cette propriété peut avoir deux valeurs :
- Membre : cette valeur désigne un employé de l’organisation hôte et un utilisateur du service de paie de l’organisation. Par exemple, cet utilisateur ne peut accéder qu’à des sites internes. Cet utilisateur ne peut pas être considéré comme collaborateur externe.

- Invité : cette valeur indique un utilisateur qui n’est pas considéré comme interne à l’entreprise, tel qu’un collaborateur externe, un partenaire, un client ou un utilisateur similaire. Un tel utilisateur n’est pas censé recevoir de mémo interne du PDG ou bénéficier des avantages de la société, par exemple.

  > [!NOTE]
  > La valeur UserType n’a aucun lien avec le mode de connexion de l’utilisateur, le rôle d’annuaire de l’utilisateur, etc. Cette propriété indique simplement la relation de l’utilisateur avec l’organisation hôte et permet à l’organisation d’appliquer des stratégies qui dépendent de cette propriété.

### <a name="source"></a>Source
Cette propriété indique le mode de connexion de l’utilisateur.

- Utilisateur invité : cet utilisateur a été invité, mais n’a pas encore utilisé une invitation.

- Active Directory externe : cet utilisateur est hébergé dans une organisation externe et s’authentifie à l’aide d’un compte Azure AD qui appartient à l’autre organisation. Ce type de connexion correspond à l’état 1.

- Compte Microsoft : cet utilisateur est hébergé dans un compte Microsoft et s’authentifie à l’aide d’un compte Microsoft. Ce type de connexion correspond à l’état 2.

- Windows Server Active Directory : cet utilisateur est connecté à partir de l’instance Active Directory locale qui appartient à cette organisation. Ce type de connexion correspond à l’état 3.

- Azure Active Directory : cet utilisateur s’authentifie à l’aide d’un compte Azure AD qui appartient à cette organisation. Ce type de connexion correspond à l’état 4.
  > [!NOTE]
  > Les propriétés Source et UserType sont indépendantes. Une valeur Source donnée n’implique pas une valeur UserType spécifique.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Des utilisateurs Azure AD B2B peuvent-ils être ajoutés en tant que membres plutôt qu’en tant qu’invités ?
En règle générale, un utilisateur Azure AD B2B et un utilisateur invité sont synonymes. Par conséquent, un utilisateur Azure AD B2B Collaboration est ajouté par défaut en tant qu’utilisateur avec la propriété UserType = Invité. Toutefois, dans certains cas, l’organisation partenaire est un membre d’une organisation plus vaste à laquelle appartient également l’organisation hôte. Il est alors possible que l’organisation hôte veuille traiter les utilisateurs de l’organisation partenaire comme membres plutôt que comme invités. Utilisez les API du Gestionnaire d’invitations Azure AD B2B pour ajouter ou inviter un utilisateur de l’organisation partenaire dans l’organisation hôte en tant que membre.

## <a name="filter-for-guest-users-in-the-directory"></a>Filtrer les utilisateurs invités dans l’annuaire

![Filtrer les utilisateurs invités](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Convertir la propriété UserType
Actuellement, les utilisateurs peuvent convertir la valeur Membre de la propriété UserType en valeur Invité, et inversement, en utilisant PowerShell. Toutefois, la propriété UserType est supposée représenter la relation de l’utilisateur avec l’organisation. Par conséquent, la valeur de cette propriété ne doit être modifiée que si la relation de l’utilisateur avec l’organisation change. La modification de cette relation implique-t-elle l’obligation de procéder à d’autres opérations, telles que le changement du nom d’utilisateur principal (UPN) ? L’utilisateur doit-il continuer à avoir accès aux mêmes ressources ? Une boîte aux lettres doit-elle être attribuée ? Nous déconseillons donc la modification de la valeur UserType à l’aide de PowerShell sous la forme d’une activité atomique. En outre, si cette propriété devient non modifiable par le biais de PowerShell, nous déconseillons l’utilisation d’une dépendance sur cette valeur.

## <a name="remove-guest-user-limitations"></a>Supprimer des limitations pour les utilisateurs invités
Dans certains cas, vous souhaiterez peut-être donner aux utilisateurs invités des privilèges plus élevés. Vous pouvez ajouter un utilisateur invité à un rôle quelconque et même supprimer les restrictions d’utilisateur invité par défaut dans le répertoire afin d’attribuer à l’utilisateur les mêmes privilèges que les membres.

Il est possible de désactiver les limitations des utilisateurs invités par défaut afin qu’un utilisateur invité dans le répertoire de la société reçoive les mêmes autorisations qu’un utilisateur membre.

![Supprimer des limitations pour les utilisateurs invités](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Puis-je rendre les utilisateurs invités visibles dans la liste d’adresses globale Exchange ?
Oui. Par défaut, les objets invités ne sont pas visibles dans la liste d’adresses globale de votre organisation, mais vous pouvez utiliser Azure Active Directory PowerShell pour les rendre visibles. Pour plus d’informations, consultez **Puis-je rendre les objets invités visibles dans la liste d’adresses globale ?** dans [Accès invité dans les groupes Office 365](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ). 

## <a name="next-steps"></a>Étapes suivantes

* [Qu'est-ce que la collaboration B2B d'Azure AD ?](what-is-b2b.md)
* [Jetons utilisateur B2B Collaboration](user-token.md)
* [Mappage des revendications utilisateur B2B Collaboration](claims-mapping.md)
