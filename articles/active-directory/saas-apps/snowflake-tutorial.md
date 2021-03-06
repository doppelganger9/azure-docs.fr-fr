---
title: 'Tutoriel : Intégration d’Azure Active Directory avec Snowflake | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Snowflake.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3488ac27-0417-4ad9-b9a3-08325fe8ea0d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2018
ms.author: jeedes
ms.openlocfilehash: 3ad3f42563878d829f900d5cddb0c6866d2deab5
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43310941"
---
# <a name="tutorial-azure-active-directory-integration-with-snowflake"></a>Tutoriel : Intégration d’Azure Active Directory avec Snowflake

Ce tutoriel vous montre comment intégrer Snowflake avec Azure Active Directory (Azure AD).

L’intégration de Snowflake avec Azure AD vous offre les avantages suivants :

- Vous pouvez contrôler dans Azure AD qui a accès à Snowflake.
- Vous pouvez autoriser les utilisateurs à se connecter automatiquement à Snowflake (via l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec Snowflake, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement avec authentification unique à Snowflake

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de Snowflake depuis la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="adding-snowflake-from-the-gallery"></a>Ajout de Snowflake depuis la galerie
Pour configurer l’intégration de Snowflake dans Azure AD, vous devez ajouter Snowflake à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Snowflake depuis la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Bouton Azure Active Directory][1]

2. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![Panneau Applications d’entreprise][2]
    
3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application][3]

4. Dans la zone de recherche, tapez **Snowflake**, sélectionnez **Snowflake** dans le panneau de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Snowflake dans la liste des résultats](./media/snowflake-tutorial/tutorial_snowflake_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Snowflake avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir quel utilisateur dans Snowflake équivaut à un utilisateur dans Azure AD. En d’autres termes, une relation entre l’utilisateur Azure AD et l’utilisateur Snowflake associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Snowflake, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Créer un utilisateur de test Snowflake](#create-a-snowflake-test-user)** pour avoir un équivalent de Britta Simon dans Snowflake lié à la représentation Azure AD de l’utilisateur.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application Snowflake.

**Pour configurer l’authentification unique Azure AD avec Snowflake, procédez comme suit :**

1. Dans le portail Azure, sur la page d’intégration de l’application **Snowflake**, cliquez sur **Authentification unique**.

    ![Lien Configurer l’authentification unique][4]

2. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Boîte de dialogue Authentification unique](./media/snowflake-tutorial/tutorial_snowflake_samlbase.png)

3. Dans la section **Domaine et URL Snowflake**, suivez les étapes ci-dessous pour configurer l’application en mode initié par **IDP**:

    ![Informations d’authentification unique dans Domaine et URL Snowflake](./media/snowflake-tutorial/tutorial_snowflake_url.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://<SNOWFLAKE-URL>`

    b. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant : `https://<SNOWFLAKE-URL>/fed/login`

4. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de service**, cochez **Afficher les paramètres d’URL avancés**, puis effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL Snowflake](./media/snowflake-tutorial/tutorial_snowflake_url1.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<SNOWFLAKE-URL>`
     
    > [!NOTE] 
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur, l’URL de réponse et l’URL de connexion réels. Pour obtenir ces valeurs, contactez l’[équipe de support technique Snowflake](https://support.snowflake.net/s/snowflake-support). 

5. Dans la section **Certificat de signature SAML**, cliquez sur **Téléchargez le certificat (Base64)** puis enregistrez le fichier du certificat sur votre ordinateur.

    ![Lien Téléchargement de certificat](./media/snowflake-tutorial/tutorial_snowflake_certificate.png) 

6. Cliquez sur le bouton **Enregistrer** .

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/snowflake-tutorial/tutorial_general_400.png)
    
7. Dans la section **Configuration de Snowflake**, cliquez sur **Configurer Snowflake** pour ouvrir la fenêtre **Configurer l’authentification**. Copiez l **’URL du service d’authentification unique SAML** à partir de la **section Référence rapide.**

    ![Configuration de Snowflake](./media/snowflake-tutorial/tutorial_snowflake_configure.png) 

8. Pour configurer l’authentification unique côté **Snowflake**, vous devez envoyer le **Certificat (Base64)** téléchargé et l’**URL du service d’authentification unique SAML** à l’[équipe de support technique Snowflake](https://support.snowflake.net/s/snowflake-support). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

   ![Créer un utilisateur de test Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet gauche du Portail Azure, cliquez sur le bouton **Azure Active Directory**.

    ![Bouton Azure Active Directory](./media/snowflake-tutorial/create_aaduser_01.png)

2. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](./media/snowflake-tutorial/create_aaduser_02.png)

3. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue **Tous les utilisateurs**.

    ![Bouton Ajouter](./media/snowflake-tutorial/create_aaduser_03.png)

4. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :

    ![Boîte de dialogue Utilisateur](./media/snowflake-tutorial/create_aaduser_04.png)

    a. Dans la zone **Nom**, tapez **BrittaSimon**.

    b. Dans la zone **Nom d’utilisateur** , tapez l’adresse e-mail de l’utilisateur Britta Simon.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="create-a-snowflake-test-user"></a>Créer un utilisateur de test Snowflake

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Snowflake. Collaborez avec l’[équipe du support technique Snowflake](https://support.snowflake.net/s/snowflake-support) pour ajouter les utilisateurs dans la plateforme Snowflake. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Snowflake.

![Attribuer le rôle utilisateur][200] 

**Pour affecter Britta Simon à Snowflake, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

2. Dans la liste des applications, sélectionnez **Snowflake**.

    ![Lien Snowflake dans la liste des applications](./media/snowflake-tutorial/tutorial_snowflake_app.png)  

3. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »][202]

4. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Volet Ajouter une attribution][203]

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

6. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

7. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette Snowflake dans le volet d’accès, vous devez être connecté automatiquement à votre application Snowflake.
Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/snowflake-tutorial/tutorial_general_01.png
[2]: ./media/snowflake-tutorial/tutorial_general_02.png
[3]: ./media/snowflake-tutorial/tutorial_general_03.png
[4]: ./media/snowflake-tutorial/tutorial_general_04.png

[100]: ./media/snowflake-tutorial/tutorial_general_100.png

[200]: ./media/snowflake-tutorial/tutorial_general_200.png
[201]: ./media/snowflake-tutorial/tutorial_general_201.png
[202]: ./media/snowflake-tutorial/tutorial_general_202.png
[203]: ./media/snowflake-tutorial/tutorial_general_203.png

