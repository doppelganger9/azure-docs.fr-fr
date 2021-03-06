---
title: Microsoft Azure Multi-Factor Authentication - État utilisateur
description: Découvrez les états utilisateurs dans Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: c39b78995aaa7e6754b180142c03cf3aa25199a5
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574269"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Comment exiger la vérification en deux étapes pour un utilisateur

Deux approches sont possibles pour exiger une vérification en deux étapes. La première option consiste à activer Azure Multi-Factor Authentication (MFA) pour chaque utilisateur. S’il est activé individuellement, l’utilisateur effectue la vérification en deux étapes chaque fois qu’il se connecte (à quelques exceptions près, notamment lorsqu’il se connecte à partir d’adresses IP approuvées ou que la fonctionnalité de _mémorisation des appareils_ est activée). La seconde option consiste à définir une stratégie d’accès conditionnel qui requiert une vérification en deux étapes sous certaines conditions.

> [!TIP]
> Choisissez une de ces deux méthodes pour exiger la vérification en deux étapes, mais pas les deux. L’activation d’Azure Multi-Factor Authentication pour un utilisateur remplace toutes les stratégies d’accès conditionnel.

## <a name="choose-how-to-enable"></a>Choisir comment activer

**Activée en modifiant l’état de l’utilisateur** : il s’agit de la méthode traditionnelle pour exiger une vérification en deux étapes et elle est analysée dans cet article. Elle fonctionne avec Azure MFA dans le cloud et le serveur Azure MFA. Cette méthode nécessite que les utilisateurs effectuent la vérification en deux étapes **chaque fois** qu’ils se connectent, puis remplace les stratégies d’accès conditionnel.

Activée par la stratégie d’accès conditionnel : il s’agit de la méthode la plus souple pour activer la vérification en deux étapes pour vos utilisateurs. Activer à l’aide de la stratégie d’accès conditionnel ne fonctionne que pour Azure MFA dans le cloud, et c’est une fonctionnalité payante d’Azure AD. Vous trouverez plus d’informations sur cette méthode dans [déployer Azure Multi-Factor Authentication basé sur le cloud](howto-mfa-getstarted.md).

Activée par Azure AD Identity Protection : cette méthode utilise la stratégie des risques Azure AD Identity Protection pour imposer la vérification en deux étapes basée uniquement sur le risque de connexion pour toutes les applications cloud. Cette méthode requiert une licence Azure Active Directory P2. Vous pourrez trouver plus d’informations sur cette méthode dans [Azure Active Directory Identity Protection](../identity-protection/howto-sign-in-risk-policy.md)

> [!Note]
> Vous trouverez plus d’informations sur les licences et la tarification sur les pages de tarification [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) et [Authentification multifacteur](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Activer Azure MFA en modifiant l’état de l’utilisateur

Les comptes d'utilisateur dans Azure Multi-Factor Authentication peuvent présenter les trois états suivants :

| Statut | Description | Applications affectées (autres que des navigateurs) | Applications du navigateur affectées | Authentification moderne affectée |
|:---:|:---:|:---:|:--:|:--:|
| Désactivé |État par défaut d’un nouvel utilisateur non inscrit à Azure MFA. |Non  |Non  |Non  |
| activé |L’utilisateur a été inscrit dans l’authentification multifacteur Azure, mais n’a pas été enregistré. Il sera invité à s’inscrire la prochaine fois qu’il se connectera. |Non.  Ils continuent de fonctionner jusqu’à ce que le processus d’inscription soit terminé. | Oui. Après expiration de la session, l’inscription à Azure MFA est nécessaire.| Oui. Après expiration du jeton d’accès, l’inscription à Azure MFA est nécessaire. |
| Appliquée |L’utilisateur a été inscrit et a terminé le processus d’inscription pour utiliser l’authentification multifacteur Azure. |Oui. Les applications requièrent des mots de passe d'application. |Oui. Azure MFA est requis lors de la connexion. | Oui. Azure MFA est requis lors de la connexion. |

L’état d’un utilisateur indique si un administrateur l’a inscrit dans l’authentification multifacteur Azure et s’il a terminé le processus d’inscription.

Tous les utilisateurs commencent avec l’état *Désactivé*. Dès lors qu’ils sont inscrits à Azure MFA, leur état devient *Activé*. Lorsque les utilisateurs activés se connectent et suivent le processus d’inscription, leur état passe à *Appliqué*.  

### <a name="view-the-status-for-a-user"></a>Afficher l’état d’un utilisateur

Pour accéder à la page où vous pouvez afficher et gérer les états des utilisateurs, procédez comme suit :

1. Connectez-vous au [portail Azure](https://portal.azure.com) en tant qu’administrateur.
2. Accédez à **Azure Active Directory** > **Utilisateurs et groupes** > **Tous les utilisateurs**.
3. Sélectionnez **Multi-Factor Authentication**.
   ![Sélectionnez Multi-Factor Authentication](./media/howto-mfa-userstates/selectmfa.png)
4. Une nouvelle page, qui affiche les états utilisateurs, s’ouvre.
   ![État utilisateur pour l’authentification multifacteur - capture d’écran](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Modifier l’état d’un utilisateur

1. Suivez les étapes précédentes pour accéder à la page **utilisateurs** d’Azure Multi-Factor Authentication.
2. Recherchez l’utilisateur pour lequel vous souhaitez activer Azure MFA. Vous devrez peut-être modifier l’affichage en haut de la page.
   ![Rechercher un utilisateur - capture d’écran](./media/howto-mfa-userstates/enable1.png)
3. Cochez la case en regard du nom de l’utilisateur.
4. À droite, sous **étapes rapides**, cliquez sur **Activer** ou **Désactiver**.
   ![Activer l’utilisateur sélectionné - capture d’écran](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > Les utilisateurs *activés* basculent automatiquement vers l’état *Appliqué* quand ils s’inscrivent à Azure MFA. Ne définissez pas manuellement l’état utilisateur *Appliqué*.

5. Confirmez votre sélection dans la fenêtre contextuelle qui s’ouvre.

Dès que vous avez activé les utilisateurs, informez-les-en par e-mail. Informez-les qu’ils seront invités à s’inscrire la prochaine fois qu’ils se connectent. Par ailleurs, si votre organisation utilise des applications sans navigateur qui ne prennent pas en charge l’authentification moderne, vos utilisateurs devront créer des mots de passe d’application. Vous pouvez également inclure un lien vers le [guide de l’utilisateur final d’Azure MFA](../user-help/multi-factor-authentication-end-user.md) pour les aider à commencer.

### <a name="use-powershell"></a>Utiliser PowerShell

Pour modifier l’état utilisateur avec [Azure AD PowerShell](/powershell/azure/overview), modifiez `$st.State`. Il existe trois états possibles :

* activé
* Appliquée
* Désactivé  

Ne basculez pas les utilisateurs directement vers l’état *Appliquée*. Sinon, les applications sans navigateur cesseront de fonctionner, car l’utilisateur n’a pas effectué l’enregistrement Azure MFA et obtenu un [mot de passe d’application](howto-mfa-mfasettings.md#app-passwords).

PowerShell est une bonne option si vous devez activer de nombreux utilisateurs à la fois. Créez un script PowerShell qui effectue une itération sur une liste d’utilisateurs et active ces utilisateurs :

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Voici un exemple de script :

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="next-steps"></a>Étapes suivantes

Pour configurer des paramètres supplémentaires tels que les adresses IP approuvées, les messages vocaux personnalisés et les alertes de fraude, consultez l’article [Configurer les paramètres d’Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)

Vous pouvez trouver des informations sur la gestion des paramètres utilisateur pour Azure Multi-Factor Authentication dans l’article [Gestion des paramètres utilisateur avec Azure Multi-Factor Authentication dans le cloud](howto-mfa-userdevicesettings.md)
