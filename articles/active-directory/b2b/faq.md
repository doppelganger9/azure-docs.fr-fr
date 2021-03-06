---
title: FAQ sur Azure Active Directory B2B Collaboration | Microsoft Docs
description: Trouvez les réponses aux questions les plus fréquentes sur Azure Active Directory B2B Collaboration.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: reference
ms.date: 05/11/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b4407ac6b7a0d9fdbf52b84fb94223c32868f0c5
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983850"
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>FAQ sur Azure Active Directory B2B Collaboration

Cette foire aux questions (FAQ) sur la collaboration interentreprises (B2B) Azure Active Directory (Azure AD) est régulièrement complétée par de nouvelles rubriques.

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Peut-on personnaliser sa page de connexion afin qu’elle soit plus intuitive pour les utilisateurs invités B2B Collaboration ?
Absolument ! Consultez notre [billet de blog sur cette fonctionnalité](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Pour plus d’informations sur la personnalisation de la page de connexion d’une organisation, consultez la page [Ajouter la personnalisation de l’entreprise aux pages de connexion et du volet d’accès](../fundamentals/customize-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Les utilisateurs de B2B Collaboration peuvent-ils accéder à SharePoint Online et OneDrive ?
Oui. La fonctionnalité de recherche d’utilisateurs invités existants avec le sélecteur de personnes dans SharePoint Online est cependant **désactivée** par défaut. Pour activer l’option de recherche d’utilisateurs invités, définissez **ShowPeoplePickerSuggestionsForGuestUsers** sur **Activé**. Vous pouvez activer ce paramètre au niveau du client ou bien de la collection de sites. Vous pouvez modifier ce paramètre à l’aide des applets de commande Set-SPOTenant et Set-SPOSite. Grâce à ces applets de commande, les membres peuvent rechercher parmi tous les utilisateurs invités existants du répertoire. Les modifications apportées à l’étendue du client n’affectent pas les sites SharePoint Online déjà configurés.

### <a name="is-the-csv-upload-feature-still-supported"></a>La fonctionnalité de chargement CSV est-elle toujours prise en charge ?
Oui. Pour plus d’informations sur la fonctionnalité de chargement de fichiers .csv, consultez [cet exemple PowerShell](code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Comment puis-je personnaliser mes e-mails d’invitation ?
Vous pouvez personnaliser pratiquement tous les éléments du processus de l’inviteur à l’aide des [API d’invitation B2B](customize-invitation-api.md).

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Les utilisateurs invités peuvent-ils réinitialiser leur méthode d’authentification multifacteur ?
Oui. Les utilisateurs invités peuvent réinitialiser leur méthode d’authentification multifacteur de la même manière que les utilisateurs normaux.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Quelle est l’organisation responsable des licences de l’authentification multifacteur ?
L’organisation à l’origine de l’invitation effectue l’authentification multifacteur. Elle doit s’assurer que l’organisation dispose d’un nombre suffisant de licences pour ses utilisateurs B2B qui utilisent l’authentification multifacteur.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Que se passe-t-il si une organisation partenaire a déjà configuré l’authentification multifacteur ? Pouvons-nous faire confiance à son authentification multifacteur et ne pas utiliser notre propre authentification multifacteur ?
Cette fonctionnalité est planifiée pour une version ultérieure : vous pourrez alors sélectionner des partenaires à exclure de votre authentification multifacteur (celle de l’organisation à l’origine de l’invitation).

### <a name="how-can-i-use-delayed-invitations"></a>Comment utiliser les invitations différées ?
Une organisation peut souhaiter ajouter des utilisateurs B2B Collaboration et les approvisionner pour les applications au fil des besoins, avant d'envoyer des invitations. Vous pouvez utiliser l’API d’invitation B2B Collaboration pour personnaliser le workflow d’intégration.

### <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Puis-je rendre les utilisateurs invités visibles dans la liste d’adresses globale Exchange ?
Oui. Par défaut, les objets invités ne sont pas visibles dans la liste d’adresses globale de votre organisation, mais vous pouvez utiliser Azure Active Directory PowerShell pour les rendre visibles. Pour plus d’informations, consultez **Puis-je rendre les objets invités visibles dans la liste d’adresses globale ?** dans [Accès invité dans les groupes Office 365](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6#PickTab=FAQ).

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Peut-on faire d’un utilisateur invité un administrateur limité ?
Absolument. Pour plus d’informations, consultez [Ajouter des utilisateurs invités à un rôle](add-guest-to-role.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Azure AD B2B Collaboration permet-il aux utilisateurs B2B d’accéder au Portail Azure ?
À moins qu’un rôle Administrateur limité ou Administrateur général ne leur soit assigné, les utilisateurs B2B Collaboration n’ont pas besoin d’accéder au portail Azure. Toutefois, les utilisateurs B2B Collaboration qui ont le rôle Administrateur limité ou Administrateur général peuvent accéder au portail. En outre, si un utilisateur invité qui ne possède aucun de ces rôles Administrateur accède au portail, il peut être en mesure d’accéder à certaines parties de l’expérience. Le rôle utilisateur invité possède quelques autorisations dans le répertoire.

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>Puis-je bloquer l’accès au portail Azure pour les utilisateurs invités ?
Oui. Lorsque vous configurez cette stratégie, faites attention à ne pas bloquer accidentellement l’accès aux membres et aux administrateurs.
Pour bloquer l’accès d’un utilisateur invité au [portail Azure](https://portal.azure.com), utilisez une stratégie d’accès conditionnel dans l’API Modèle de déploiement Azure Classic :
1. Modifiez le groupe **Tous les utilisateurs** pour qu’il contienne uniquement les membres.
  ![capture d’écran - modifier le groupe](media/faq/modify-all-users-group.png)
2. Créez un groupe dynamique contenant des utilisateurs invités.
  ![capture d’écran - créer un groupe](media/faq/group-with-guest-users.png)
3. Configurez une stratégie d’accès conditionnel pour empêcher les utilisateurs invités d’accéder au portail, comme le montre la vidéo suivante :
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Azure AD B2B Collaboration prend-il en charge l’authentification multifacteur et les comptes de messagerie grand public ?
Oui. Les comptes de messagerie grand public et l’authentification multifacteur sont tous deux pris en charge par Azure AD B2B Collaboration.

### <a name="do-you-plan-to-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>Prévoyez-vous de prendre en charge la réinitialisation de mot de passe pour les utilisateurs Azure AD B2B Collaboration ?
Oui. Voici les informations importantes concernant la réinitialisation de mot de passe en libre-service (SSPR) pour un utilisateur B2B invité à partir d’une organisation partenaire :
 
* La réinitialisation de mot de passe en libre-service ne s’effectue que dans le locataire d’identité de l’utilisateur B2B.
* Si le locataire d’identité est un compte Microsoft, le mécanisme de réinitialisation de mot de passe en libre-service du compte Microsoft est utilisé.
* Si le locataire d’identité est un locataire « viral » ou juste-à-temps (JIT), un e-mail de réinitialisation de mot de passe est envoyé.
* Concernant les autres locataires, le processus de réinitialisation standard est suivi pour les utilisateurs B2B. Comme pour la réinitialisation de mot de passe libre-service des utilisateurs B2B membres, la location est bloquée dans le contexte de la ressource. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>La réinitialisation du mot de passe est-elle disponible pour les utilisateurs invités dans un locataire « viral » ou juste-à-temps (JIT) et qui ont accepté des invitations par le biais d’une adresse e-mail scolaire ou professionnelle, mais qui n’avaient pas de compte Azure AD préexistant ?
Oui. Un e-mail de réinitialisation de mot de passe peut être envoyé pour permettre à un utilisateur de réinitialiser son mot de passe dans la location JIT.

### <a name="does-microsoft-dynamics-365-provide-online-support-for-azure-ad-b2b-collaboration"></a>Microsoft Dynamics 365 fournit-il un support en ligne pour Azure AD B2B Collaboration ?
Oui, Dynamics 365 (en ligne) prend en charge Azure AD B2B Collaboration. Pour plus d’informations, consultez l’article Dynamics 365 [Inviter des utilisateurs avec Azure Active Directory B2B Collaboration](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration).

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Quelle est la durée de vie d’un mot de passe initial pour un utilisateur B2B Collaboration nouvellement créé ?
Azure AD dispose d’un ensemble fixe d'exigences en matière de nombre de caractères, de force du mot de passe et de verrouillage de compte qui s’appliquent à tous les comptes d’utilisateur cloud Azure AD. Les comptes d’utilisateur cloud sont des comptes qui ne sont pas fédérés avec un autre fournisseur d’identité, comme 
* Compte Microsoft
* Facebook
* Services de fédération Active Directory (AD FS)
* Un autre locataire cloud (pour B2B Collaboration)

Pour les comptes fédérés, la stratégie de mot de passe dépend de la stratégie qui s’applique dans la location locale et des paramètres de compte Microsoft de l’utilisateur.

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>Une organisation peut avoir différentes expériences dans ses applications pour les utilisateurs clients et les utilisateurs invités. Une assistance standard est-elle disponible pour cela ? La présence de la revendication de fournisseur d’identité représente-t-elle le bon modèle ?
 Un utilisateur invité peut utiliser n’importe quel fournisseur d’identité pour s’authentifier. Pour plus d’informations, consultez la page [Propriétés d’un utilisateur B2B Collaboration](user-properties.md). Utilisez la propriété **UserType** pour déterminer l’expérience utilisateur. La revendication **UserType** n’est pour le moment pas incluse dans le jeton. Les applications doivent utiliser l’API Graph pour interroger le répertoire de l’utilisateur et obtenir le UserType.

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>Où puis-je trouver une communauté B2B Collaboration pour partager des solutions et envoyer des idées ?
Nous sommes constamment à l’écoute de vos commentaires afin d’améliorer B2B Collaboration. Nous vous invitons à partager vos scénarios utilisateur, vos meilleures pratiques et ce que vous appréciez par rapport à Azure AD B2B Collaboration. Participez à la discussion dans la [Communauté Microsoft Tech](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Nous vous invitons également à soumettre vos idées et à voter pour les prochaines fonctionnalités sur la page [Idées B2B Collaboration](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>Peut-on envoyer une invitation utilisée automatiquement, afin que l’utilisateur soit simplement « prêt » ? L’utilisateur doit-il toujours cliquer sur l’URL d’utilisation ?
Un inviteur peut inviter d’autres utilisateurs dans l’organisation partenaire à l’aide de l’interface utilisateur, de scripts PowerShell ou d’API. Ensuite, l’inviteur peut envoyer à l’utilisateur invité un lien direct vers une application partagée. Dans la plupart des cas, il n’est plus nécessaire d’ouvrir l’invitation par e-mail et de cliquer sur une URL d’échange. Pour plus d’informations, consultez [Utilisation d'invitations Azure Active Directory B2B Collaboration](redemption-experience.md).

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>Comment fonctionne la collaboration B2B lorsque le partenaire invité utilise la fédération pour ajouter sa propre authentification locale ?
Si le partenaire a un client Azure AD qui est fédéré à l’infrastructure d’authentification locale, l’authentification unique (SSO) locale s’effectue automatiquement. Si le partenaire n’a pas de client Azure AD, un compte Azure AD est créé pour les nouveaux utilisateurs. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Je pensais qu’Azure AD B2B n’acceptait pas les adresses de messagerie gmail.com et outlook.com, et que B2C était utilisé pour ces types de comptes ?
Nous supprimons les différences entre B2B et B2C (des entreprises aux particuliers) Collaboration en ce qui concerne la prise en charge des identités. L’identité utilisée n’est pas un bon critère de choix entre B2B et B2C. Pour plus d’informations sur le choix de l’option de collaboration, consultez la page [Comparer B2C et B2B Collaboration dans Azure Active Directory](compare-with-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Quelles applications et services prennent en charge les utilisateurs invités Azure B2B ?
Toutes les applications intégrées à Azure AD prennent en charge les utilisateurs invités Azure B2B. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Pouvons-nous forcer l’authentification multifacteur pour les utilisateurs invités B2B si nos partenaires n’ont pas l’authentification multifacteur ?
Oui. Pour plus d’informations, consultez la page [Accès conditionnel pour les utilisateurs B2B Collaboration](conditional-access.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>Dans SharePoint, il est possible de définir une liste « allow (autoriser) » ou « deny (refuser) » pour les utilisateurs externes. Peut-on faire cela dans Azure ?
Oui. Azure AD B2B Collaboration prend en charge les listes autorisées et refusées. 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>Quelles licences faut-il avoir pour utiliser Azure AD B2B ?
Pour plus d’informations sur les licences dont votre organisation a besoin pour utiliser Azure AD B2B, consultez la page [Aide sur les licences Azure Active Directory B2B Collaboration](licensing-guidance.md).

### <a name="next-steps"></a>Étapes suivantes

- [Qu'est-ce que la collaboration B2B d'Azure AD ?](what-is-b2b.md)

