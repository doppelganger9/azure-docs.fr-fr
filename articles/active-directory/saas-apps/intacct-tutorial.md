---
title: 'Didacticiel : Intégration d’Azure Active Directory avec Intacct | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Intacct.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: d834ca75085878350e257cc1c50e60fc1bf28484
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447337"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Didacticiel : Intégration d’Azure Active Directory à Intacct

Dans ce didacticiel, vous allez apprendre à intégrer Intacct à Azure Active Directory (Azure AD).

L’intégration d’Intacct à Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Intacct
- Vous pouvez autoriser vos utilisateurs à se connecter automatiquement à Intacct (via l’authentification unique) avec leur compte Azure AD
- Vous pouvez gérer vos comptes à partir d’un emplacement central : le portail Azure.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Intacct, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement Intacct pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout d’Intacct à partir de la galerie
1. Configuration et test de l’authentification unique Azure AD

## <a name="adding-intacct-from-the-gallery"></a>Ajout d’Intacct à partir de la galerie
Pour configurer l’intégration d’Intacct avec Azure AD, vous devez ajouter Intacct à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Intacct à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Active Directory][1]

1. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![APPLICATIONS][2]
    
1. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![APPLICATIONS][3]

1. Dans la zone de recherche, entrez **Intacct**.

    ![Création d’un utilisateur de test Azure AD](./media/intacct-tutorial/tutorial_intacct_search.png)

1. Dans le volet de résultats, sélectionnez **Intacct**, puis cliquez sur **Ajouter** pour ajouter l’application.

    ![Création d’un utilisateur de test Azure AD](./media/intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuration et test de l’authentification unique Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Intacct avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Intacct équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur Intacct associé doit être établie.

Dans Intacct, affectez la valeur de **nom d’utilisateur** dans Azure AD comme valeur **Nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec Intacct, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
1. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
1. **[Création d’un utilisateur de test Intacct](#creating-an-intacct-test-user)** : pour avoir un équivalent de l’utilisateur Britta Simon dans Intacct, qui soit lié à la représentation de l’utilisateur Azure AD.
1. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** : permet à Britta Simon d’utiliser l’authentification unique Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuration de l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application Intacct.

**Pour configurer l’authentification unique Azure AD avec Intacct, procédez comme suit :**

1. Dans le portail Azure, sur la page d’intégration de l’application **Intacct**, cliquez sur **Authentification unique**.

    ![Configurer l'authentification unique][4]

1. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Configurer l'authentification unique](./media/intacct-tutorial/tutorial_intacct_samlbase.png)

1. Dans la section **Domaine et URL Intacct**, procédez comme suit :

    ![Configurer l'authentification unique](./media/intacct-tutorial/tutorial_intacct_url.png)

    Dans la zone de texte **URL de réponse** , tapez une URL au format suivant :
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Cette valeur n’est pas la valeur réelle. Mettez à jour la valeur avec l’URL de réponse réelle. Pour obtenir ces valeurs, contactez [l’équipe de support technique Intacct](https://us.intacct.com/support).

1. Dans la section **Certificat de signature SAML**, cliquez sur **Téléchargez le certificat (Base64)** puis enregistrez le fichier du certificat sur votre ordinateur.

    ![Configure Single Sign-On](./media/intacct-tutorial/tutorial_intacct_certificate.png) 

1. Cliquez sur le bouton **Enregistrer** .

    ![Configurer l'authentification unique](./media/intacct-tutorial/tutorial_general_400.png)

1. Dans la section **Configuration de Intacct**, cliquez sur **Configurer Intacct** pour ouvrir la fenêtre **Configurer l’authentification**. Copiez l’**ID d’entité SAML et l’URL du service d’authentification unique SAML** à partir de la **section Référence rapide.**

    ![Configurer l'authentification unique](./media/intacct-tutorial/tutorial_intacct_configure.png) 

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre site d’entreprise Intacct en tant qu’administrateur.

1. Cliquez sur l’onglet **Company**, puis sur **Company Info**.

    ![Company](./media/intacct-tutorial/ic790037.png "Company")

1. Cliquez sur l’onglet **Security**, puis sur **Edit**.

    ![Sécurité](./media/intacct-tutorial/ic790038.png "Sécurité")

1. Dans la section **Single sign on (SSO)** , procédez comme suit :

    ![Single sign On](./media/intacct-tutorial/ic790039.png "Single sign On")

    a. Sélectionnez **Enable Single Sign-On**.

    b. Pour **Identity provider type**, sélectionnez **SAML 2.0**.

    c. Dans la zone de texte **URL émettrice**, collez la valeur **ID d’entité SAML** que vous avez copiée dans le portail Azure.
   
    d. Dans la zone de texte **URL de connexion**, collez la valeur **URL du service d’authentification unique SAML** que vous avez copiée dans portail Azure.

    e. Ouvrez votre certificat codé en**base-64** dans le bloc-notes, copiez son contenu dans le presse-papiers, puis collez-le dans la zone **Certificat**.
   
    f. Cliquez sur **Enregistrer**.

> [!TIP]
> Vous pouvez maintenant lire une version concise de ces instructions dans le [portail Azure](https://portal.azure.com), pendant que vous configurez l’application.  Après avoir ajouté cette application à partir de la section **Active Directory > Applications d’entreprise**, cliquez simplement sur l’onglet **Authentification unique** et accédez à la documentation incorporée par le biais de la section **Configuration** en bas. Vous pouvez en savoir plus sur la fonctionnalité de documentation incorporée ici : [Documentation incorporée Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Création d’un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

![Créer un utilisateur Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **portail Azure**, cliquez sur l’icône **Azure Active Directory**.

    ![Création d’un utilisateur de test Azure AD](./media/intacct-tutorial/create_aaduser_01.png) 

1. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.
    
    ![Création d’un utilisateur de test Azure AD](./media/intacct-tutorial/create_aaduser_02.png) 

1. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue.
 
    ![Création d’un utilisateur de test Azure AD](./media/intacct-tutorial/create_aaduser_03.png) 

1. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :
 
    ![Création d’un utilisateur de test Azure AD](./media/intacct-tutorial/create_aaduser_04.png) 

    a. Dans la zone de texte **Nom**, entrez **BrittaSimon**.

    b. Dans la zone de texte **Nom d’utilisateur**, tapez **l’adresse e-mail** de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur du **mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="creating-an-intacct-test-user"></a>Création d’un utilisateur de test Intacct

Pour configurer les utilisateurs Azure AD afin qu’ils puissent se connecter à Intacct, ils doivent être approvisionnés dans Intacct. Pour Intacct, l’approvisionnement est une tâche manuelle.

**Pour approvisionner un compte d’utilisateur, procédez comme suit :**

1. Connectez-vous à votre locataire **Intacct**.

1. Cliquez sur l’onglet **Company**, puis sur **Users**.

    ![Utilisateurs](./media/intacct-tutorial/ic790041.png "Utilisateurs")
1. Cliquez sur l’onglet **Ajouter**.

    ![Add](./media/intacct-tutorial/ic790042.png "Add")
1. Dans la section **User Information** , procédez comme suit :

    ![User Information](./media/intacct-tutorial/ic790043.png "User Information")

    a. Tapez **l’ID utilisateur**, le **nom**, le **prénom**, **l’adresse de messagerie**, le **titre** et le **numéro de téléphone** d’un compte Azure AD que vous souhaitez approvisionner dans la section **Informations utilisateur**.

    b. Sélectionnez les **privilèges d’administrateur** d’un compte Azure AD que vous voulez approvisionner.
   
    c. Cliquez sur **Enregistrer**. Le détenteur du compte Azure AD reçoit un message électronique et suit un lien pour confirmer le compte avant qu’il ne soit activé.

>[!NOTE]
>Pour approvisionner des comptes utilisateur Azure AD, vous pouvez utiliser d’autres outils de création de compte utilisateur Intacct ou des API fournies par Intacct.
        
### <a name="assigning-the-azure-ad-test-user"></a>Affectation de l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Intacct.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à Intacct, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

1. Dans la liste des applications, sélectionnez **Intacct**.

    ![Configurer l'authentification unique](./media/intacct-tutorial/tutorial_intacct_app.png) 

1. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Affecter des utilisateurs][202] 

1. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Affecter des utilisateurs][203]

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

1. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

1. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="testing-single-sign-on"></a>Test de l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la vignette Intacct dans le volet d’accès, vous devez être connecté automatiquement à votre application Intacct.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/intacct-tutorial/tutorial_general_01.png
[2]: ./media/intacct-tutorial/tutorial_general_02.png
[3]: ./media/intacct-tutorial/tutorial_general_03.png
[4]: ./media/intacct-tutorial/tutorial_general_04.png

[100]: ./media/intacct-tutorial/tutorial_general_100.png

[200]: ./media/intacct-tutorial/tutorial_general_200.png
[201]: ./media/intacct-tutorial/tutorial_general_201.png
[202]: ./media/intacct-tutorial/tutorial_general_202.png
[203]: ./media/intacct-tutorial/tutorial_general_203.png

