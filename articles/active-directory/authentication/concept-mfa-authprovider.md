---
title: Quand et comment utiliser un fournisseur Azure Multi-Factor Auth ?
description: Quand devriez-vous utiliser un fournisseur d’authentification avec Azure MFA ?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: b601a3d23b23faa16925881a54e2ceba85c800f8
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669063"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Quand utiliser un fournisseur Azure Multi-Factor Authentication

La vérification en deux étapes est disponible par défaut pour les administrateurs généraux disposant d’Azure Active Directory et les utilisateurs Office 365. Toutefois, si vous souhaitez tirer parti des [fonctionnalités avancées](howto-mfa-mfasettings.md), vous devez acheter la version complète d’Azure Multi-Factor Authentication (MFA).

Un fournisseur d’authentification multifacteur Azure permet de tirer parti des fonctionnalités fournies par l’authentification multifacteur Azure pour les utilisateurs qui **n’ont pas de licence**. 

Si vous avez des licences qui couvrent tous les utilisateurs de votre organisation, vous n’avez donc pas besoin d’un fournisseur Azure Multi-Factor Auth. Créez un fournisseur Azure Multi-Factor Authentication uniquement si vous avez également besoin de fournir la vérification en deux étapes pour certains utilisateurs dépourvus de licences.

> [!NOTE]
> À partir du 1er septembre 2018, il ne sera plus possible de créer des fournisseurs d’authentification. Il restera possible d’utiliser et de mettre à jour des fournisseurs d’authentification existants. L’authentification multifacteur restera une fonctionnalité disponible dans les licences Azure AD Premium.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Mises en garde liées au Kit de développement logiciel (SDK) Azure MFA

Il sera en revanche nécessaire si vous souhaitez télécharger le Kit de développement logiciel (SDK). Remarque que le Kit de développement logiciel a été déconseillé et n’est plus pris en charge pour les nouveaux clients et continueront à fonctionner uniquement jusqu’au 14 novembre 2018. Passée cette date, les appels au Kit de développement logiciel (SDK) échoueront.

Pour télécharger le Kit de développement logiciel (SDK), créez un fournisseur Azure Multi-Factor Auth, même si vous disposez d’Azure MFA, AAD Premium, ou d’autres licences groupées. Si vous créez un fournisseur d’authentification multifacteur Azure à cet effet et que vous avez déjà des licences, veillez à créer le fournisseur avec le modèle **Par utilisateur activé**. Ensuite, liez le fournisseur au répertoire qui contient les licences Azure MFA, Azure AD Premium, ou d’autres licences groupées. Cette configuration garantit que vous n’êtes facturé que si vous avez plus d’utilisateurs uniques utilisant la vérification en deux étapes que le nombre de licences en votre possession.

## <a name="what-is-an-mfa-provider"></a>Qu’est-ce qu’un fournisseur MFA ?

Si vous n’avez pas de licences pour Azure Multi-Factor Authentication, vous pouvez créer un fournisseur d’authentification qui exige une vérification en deux étapes pour les utilisateurs.

Il existe deux types de fournisseurs d’authentification, qui se distinguent par la façon dont votre abonnement Azure est facturé. L’option par authentification calcule le nombre d’authentifications effectuées pour votre locataire en un mois. Cette option est recommandée si de nombreux utilisateurs ne s’authentifient qu’occasionnellement. L’option par utilisateur calcule le nombre de personnes sur votre client qui effectuent la vérification en deux étapes au cours d’un mois. Cette option est recommandée si vous disposez de certains utilisateurs dotés de licences, mais que vous avez besoin d’étendre MFA à davantage d’utilisateurs au-delà des limites de vos licences.

## <a name="create-an-mfa-provider"></a>Créer un fournisseur MFA

Suivez les étapes ci-dessous pour créer un fournisseur Azure Multi-Factor Authentication dans le portail Azure :

1. Connectez-vous au [portail Azure](https://portal.azure.com) en tant qu’administrateur.
2. Sélectionnez **Azure Active Directory** > **Serveur MFA** > **Fournisseurs**.

   ![Fournisseurs][Providers]

3. Sélectionnez **Ajouter**.
4. Renseignez les champs suivants et sélectionnez **Ajouter** :
   - **Nom** : nom complet du fournisseur.
   - **Modèle d’utilisation** : vous pouvez choisir l’une des deux options suivantes :
      * Par authentification – modèle d'achat facturé par authentification. Généralement utilisé pour les scénarios qui utilisent Azure Multi-Factor Authentication dans une application orientée client.
      * Par utilisateur activé – modèle d'achat facturé par utilisateur activé. Généralement utilisé pour l’accès des employés aux applications telles qu’Office 365. Choisissez cette option si vous avez des utilisateurs qui disposent déjà d’une licence pour l’authentification multifacteur Azure.
   - **Abonnement** : abonnement Azure facturé pour une vérification en deux étapes via le fournisseur.
   - **Répertoire** : locataire Azure Active Directory auquel le fournisseur est associé.
      * Vous n'avez pas besoin d’un répertoire Azure AD pour créer un fournisseur. Laissez ce champ vide si vous prévoyez uniquement de télécharger le serveur Azure Multi-Factor Authentication.
      * Le fournisseur doit être associé avec un répertoire Azure AD pour tirer parti des fonctionnalités avancées.
      * Un seul fournisseur peut être associé à un répertoire Azure AD.

## <a name="manage-your-mfa-provider"></a>Gestion de votre fournisseur MFA

Vous ne pouvez pas modifier le modèle d’utilisation (par utilisateur activé ou par authentification) après avoir créé un fournisseur MFA. Toutefois, vous pouvez supprimer le fournisseur MFA, puis en créer un avec un autre modèle d’utilisation.

Si le fournisseur Multi-Factor Auth actuel est associé à un répertoire Azure AD (également appelé locataire Azure AD), vous pouvez en toute sécurité le supprimer et en créer un associé au même locataire Azure AD. Si vous avez acheté suffisamment de licences pour couvrir tous les utilisateurs activés pour l’authentification multifacteur, vous pouvez également supprimer complètement le fournisseur d’authentification multifacteur.

Si votre fournisseur MFA n’est pas associé à un locataire Azure AD, ou si vous associez le nouveau fournisseur MFA à un autre locataire Azure AD, les paramètres utilisateur et les options de configuration ne sont pas transférés. Par ailleurs, les serveurs Azure MFA existants doivent être réactivés à l’aide des informations d’identification d’activation générées via le nouveau fournisseur MFA. Le fait de réactiver les serveurs MFA afin de les lier au nouveau fournisseur MFA n’a pas de conséquence sur l’authentification des appels téléphoniques et des SMS. Toutefois, les notifications d’applications mobiles cessent de fonctionner pour tous les utilisateurs jusqu’à ce qu’ils réactivent l’application mobile.

## <a name="next-steps"></a>Étapes suivantes

[Configurer les paramètres d’Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)

[Providers]: ./media/concept-mfa-authprovider/add-providers.png "Ajouter des fournisseurs MFA"
