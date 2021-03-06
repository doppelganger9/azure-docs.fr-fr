---
title: Listing de votre application dans la galerie d’applications Azure Active Directory | Microsoft Docs
description: Apprendre à lister une application qui prend en charge l’authentification unique dans la galerie d’applications Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/31/2018
ms.author: celested
ms.reviewer: elisol, bryanla
ms.custom: aaddev
ms.openlocfilehash: d5c00e9df9c1bfee0c665cafc763c52a36f98052
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345843"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Lister votre application dans la galerie d’applications Azure Active Directory


##  <a name="what-is-the-azure-ad-application-gallery"></a>Qu’est-ce que la galerie d’applications Azure AD ?

Azure Active Directory (Azure AD) est un service d’identité basé sur le cloud. La [galerie d’applications Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) se trouve dans l’app store de la Place de marché Microsoft Azure, où tous les connecteurs d’applications sont publiés pour l’authentification unique et le provisionnement des utilisateurs. Les clients qui utilisent Azure AD comme fournisseur d’identité y trouvent les connecteurs d’applications SaaS publiés. Les administrateurs informatiques ajoutent des connecteurs à partir de la galerie d’applications, puis configurent et utilisent ces connecteurs pour l’authentification unique et le provisionnement. Azure AD prend en charge tous les principaux protocoles de fédération pour l’authentification unique, notamment SAML 2.0, OpenID Connect, OAuth et WS-Fed.

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Quels sont les avantages du listing d’une application dans la galerie ?

*  Les clients bénéficient de l’expérience d’authentification unique la meilleure possible.

*  La configuration de l’application est simple et minime.

*  Une recherche rapide permet de trouver votre application dans la galerie.

*  Les clients Azure AD des abonnements Gratuit, De base et Premium peuvent tous utiliser cette intégration.

*  Les clients mutuels obtiennent un tutoriel de configuration pas à pas.

*  Les clients qui utilisent SCIM peuvent utiliser le provisionnement pour la même application.

## <a name="prerequisites"></a>Prérequis

- Pour des applications fédérées (Open ID et SAML/WS-Fed), l’application doit prendre en charge le modèle SaaS pour être répertorié dans la galerie Azure AD. Les applications de galerie d’entreprise doivent prendre en charge plusieurs configurations de client, non un client spécifique.

- Pour Open ID Connect, l’application doit être mutualisée et l’[infrastructure de consentement Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md#overview-of-the-consent-framework) doit être correctement implémentée pour l’application. L’utilisateur peut envoyer la demande de connexion à un point de terminaison commun de façon à ce que tout client puisse donner son consentement à l’application. Vous pouvez contrôler l’accès utilisateur en fonction de l’ID de locataire et de l’UPN de l’utilisateur, reçus dans le jeton.

- Pour SAML 2.0/WS-Fed : votre application doit pouvoir effectuer l’intégration de l’authentification unique SAML/WS-Fed en mode SP ou IDP. Vérifiez que cela fonctionne correctement avant d’envoyer la demande.

- Pour une authentification unique par mot de passe, assurez-vous que votre application prend en charge l’authentification par formulaire de telle sorte que le mot de passe puisse être stocké dans le coffre pour que le travail d’authentification unique soit effectué comme prévu.

- Pour les demandes d’attribution automatique d’utilisateurs, l’application doit être répertoriée dans la galerie avec une fonctionnalité d’authentification unique activée utilisant tout protocole de fédération décrit ci-dessus. Vous pouvez demander l’authentification unique et l’attribution d’utilisateur ensemble sur le portail, si elles ne sont pas répertoriées.

##  <a name="implementing-sso-using-federation-protocol"></a>Implémentation de l’authentification unique à l’aide d’un protocole de fédération

Pour lister une application dans la galerie d’applications Azure AD, vous devez tout d’abord implémenter l’un des protocoles de fédération suivants pris en charge par Azure AD, puis accepter les conditions générales d’utilisation de la galerie d’applications Azure AD. Lisez les conditions générales d’utilisation de la galerie d’applications Azure AD [ici](https://azure.microsoft.com/en-us/support/legal/active-directory-app-gallery-terms/).

*   **OpenID Connect** : pour intégrer votre application avec Azure AD en utilisant le protocole Open ID Connect, suivez les [instructions destinées aux développeurs](authentication-scenarios.md).

    ![Chronologie d’énumération des applications OpenID Connect dans la galerie](./media/howto-app-gallery-listing/openid.png)

    * Si vous souhaitez ajouter votre application à la liste dans la galerie à l’aide d’OpenID Connect, sélectionnez **OpenID Connect & OAuth 2.0** comme indiqué ci-dessus.

    * En cas de problème d’accès, contactez l’[équipe d’intégration de l’authentification unique Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **SAML 2.0** ou **WS-Fed** : si votre application prend en charge SAML 2.0, vous pouvez l’intégrer directement avec un locataire Azure AD en suivant les [instructions pour ajouter une application personnalisée](../manage-apps/configure-single-sign-on-non-gallery-applications.md).

    ![Chronologie de l’énumération des applications SAML 2.0 ou WS-Fed dans la galerie](./media/howto-app-gallery-listing/saml.png)

    * Si vous souhaitez ajouter votre application à la liste dans la galerie à l’aide de **SAML 2.0** ou **WS-Fed**, sélectionnez **SAMl 2.0/WS-Fed** comme indiqué ci-dessus.

    * En cas de problème d’accès, contactez l’[équipe d’intégration de l’authentification unique Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-password-sso"></a>Implémentation de l’authentification unique à l’aide d’une authentification unique du mot de passe

Créez une application web qui a une page de connexion HTML pour configurer l’[authentification unique par mot de passe](../manage-apps/what-is-single-sign-on.md). L'authentification unique basée sur un mot de passe, également appelée  archivage de mot de passe, vous permet de gérer l'accès utilisateur et les mots de passe pour les applications Web qui ne prennent pas en charge la fédération d'identité. Elle est également utile pour les scénarios où plusieurs utilisateurs doivent partager un seul compte, par exemple les comptes d’applications de médias sociaux de votre organisation.

![Chronologie de l’énumération des applications avec authentification unique par mot de passe dans la galerie](./media/howto-app-gallery-listing/passwordsso.png)

* Si vous souhaitez ajouter votre application à la liste dans la galerie à l’aide de l’authentification unique par mot de passe, sélectionnez **Authentification unique par mot de passe** comme indiqué ci-dessus.

* En cas de problème d’accès, contactez l’[équipe d’intégration de l’authentification unique Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

##  <a name="updateremove-existing-listing"></a>Mettre à jour/supprimer un listing existant

Pour mettre à jour ou supprimer une application existante dans la galerie d’applications Azure AD, vous devez tout d’abord envoyer la requête dans le [Portail du réseau d’applications](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Si vous avez un compte Office 365, utilisez-le pour vous connecter à ce portail. Dans le cas contraire, utilisez votre compte Microsoft (comme Outlook ou Hotmail) pour vous connecter.

* Sélectionnez l’option appropriée dans l’image ci-dessous

    ![Chronologie de l’énumération des applications SAML dans la galerie](./media/howto-app-gallery-listing/updateorremove.png)

    * Si vous souhaitez mettre à jour une application existante, sélectionnez **Mettre à jour le listing des applications existantes**.

    * Si vous souhaitez supprimer une application existante à partir de la galerie Azure AD, sélectionnez **Supprimer le listing des applications existantes**

    * En cas de problème d’accès, contactez l’[équipe d’intégration de l’authentification unique Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

## <a name="submit-the-request-in-the-portal"></a>Envoyer la demande dans le portail

Une fois que vous avez testé le bon fonctionnement de l’intégration de votre application dans Azure AD, envoyez votre demande d’accès à notre [portail Application Network](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Si vous avez un compte Office 365, utilisez-le pour vous connecter à ce portail. Dans le cas contraire, utilisez votre compte Microsoft (comme Outlook ou Hotmail) pour vous connecter.

Une fois connecté, la page suivante s’affiche. Justifiez votre besoin d’accès dans la zone de texte, puis sélectionnez **Demander l’accès**. Notre équipe examine ces informations et vous octroie l’accès en conséquence. Ensuite, vous pouvez vous connecter au portail et envoyer votre demande d’application détaillée.

En cas de problème d’accès, contactez l’[équipe d’intégration de l’authentification unique Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Requête d’accès sur le portail SharePoint](./media/howto-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Chronologies
    
La durée du processus de listing d’une application SAML 2.0 ou WS-Fed dans la galerie est de 7 à 10 jours ouvrables.

   ![Chronologie de l’énumération des applications SAML dans la galerie](./media/howto-app-gallery-listing/timeline.png)

La durée du processus de listing d’une application OpenID Connect dans la galerie est de 2 à 5 jours ouvrables.

   ![Chronologie de l’énumération des applications SAML dans la galerie](./media/howto-app-gallery-listing/timeline2.png)

La durée du processus de listing d’une application dans la galerie avec l’approvisionnement d’utilisateurs est de 40 à 45 jours ouvrables.

   ![Chronologie de l’énumération des applications SAML dans la galerie](./media/howto-app-gallery-listing/provisioningtimeline.png)

## <a name="escalations"></a>Escalades

Pour faire remonter un problème, envoyez un e-mail à [l’équipe d’intégration SSO Azure AD](mailto:SaaSApplicationIntegrations@service.microsoft.com) qui est SaaSApplicationIntegrations@service.microsoft.com et nous vous répondrons dès que possible.