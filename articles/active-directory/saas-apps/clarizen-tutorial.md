---
title: 'Didacticiel : Intégration d’Azure Active Directory à Clarizen | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Clarizen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: jeedes
ms.openlocfilehash: 855f147b0622ecc0831f2bc464e83d245af9e574
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158669"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Didacticiel : Intégration d’Azure Active Directory à Clarizen

Dans ce didacticiel, vous allez apprendre à intégrer Azure Active Directory (Azure AD) à Clarizen. Cette intégration vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Clarizen.
- Vous pouvez permettre aux utilisateurs de se connecter automatiquement à Clarizen (par le biais de l’authentification unique) avec leur compte Azure AD.
- Vous pouvez centraliser la gestion de vos comptes à un seul emplacement : le Portail Azure.

Selon le scénario considéré dans ce didacticiel, vous allez exécuter deux tâches principales :

1. Ajoutez Clarizen à partir de la galerie.
1. Configurez et testez l’authentification unique Azure AD.

Pour plus d’informations sur l’intégration d’applications SaaS (software as a service) à Azure AD, consultez l’article [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis
Pour configurer l’intégration d’Azure AD à Clarizen, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement Clarizen activé pour l’authentification unique

Pour tester la procédure de ce didacticiel, suivez les recommandations ci-dessous :

- Testez l’authentification unique Azure AD dans un environnement de test. N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous ne disposez pas d’un environnement de test Azure AD, vous pouvez [vous inscrire pour un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Ajouter Clarizen à partir de la galerie
Pour configurer l’intégration de Clarizen à Azure AD, ajoutez Clarizen à partir de la galerie à votre liste d’applications SaaS gérées.

1. Dans le volet gauche du [Portail Azure](https://portal.azure.com), cliquez sur l’icône **Azure Active Directory**.

    ![Icône Azure Active Directory][1]

1. Cliquez sur **Applications d’entreprise**. Puis cliquez sur **Toutes les applications**.

    ![Sélection des options « Applications d’entreprise » et « Toutes les applications »][2]

1. Cliquez sur le bouton **Ajouter** au bas de la boîte de dialogue.

    ![Bouton « Ajouter »][3]

1. Dans la zone de recherche, tapez **Clarizen**.

    ![Saisie de « Clarizen » dans la zone de recherche](./media/clarizen-tutorial/tutorial_clarizen_000.png)

1. Dans le volet de résultats, sélectionnez **Clarizen**, puis cliquez sur **Ajouter** pour ajouter l’application.

    ![Sélection de Clarizen dans le volet de résultats](./media/clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD
Dans les sections suivantes, vous allez configurer et tester l’authentification unique Azure AD avec Clarizen pour l’utilisateur de test nommé Britta Simon.

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Clarizen équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur Clarizen associé doit être établie. Pour cela, affectez la valeur du **nom d’utilisateur** dans Azure AD comme valeur du champ **Username** (Nom d’utilisateur) dans Clarizen.

Pour configurer et tester l’authentification unique Azure AD avec Clarizen, suivez les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
1. **[Créer un utilisateur de test Clarizen](#create-a-clarizen-test-user)** pour disposer d’un équivalent de Britta Simon dans Clarizen lié à la représentation Azure AD associée.
1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
1. **[Tester l’authentification unique](#test-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD
Activez l’authentification unique Azure AD dans le Portail Azure et configurez l’authentification unique dans votre application Clarizen.

1. Dans le Portail Azure, sur la page d’intégration de l’application **Clarizen**, cliquez sur **Authentification unique**.

    ![Sélection de l’option « Authentification unique »][4]

1. Dans la boîte de dialogue **Authentification unique**, au niveau de la zone **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.

    ![Sélection de l’option « Authentification basée sur SAML »](./media/clarizen-tutorial/tutorial_clarizen_01.png)

1. Dans la section **Domaine et URL Clarizen**, procédez comme suit :

    ![Zones d’identificateur et d’URL de réponse](./media/clarizen-tutorial/tutorial_clarizen_02.png)

    a. Dans la zone **Identificateur**, tapez la valeur **Clarizen**.

    b. Dans la zone **URL de réponse**, tapez une URL en utilisant le modèle suivant : **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Il ne s’agit pas des valeurs réelles. Vous devrez utiliser les véritables valeurs d’identificateur et d’URL de réponse. Nous vous suggérons d’utiliser ici la valeur unique d’une chaîne en guise d’identificateur. Pour obtenir les valeurs réelles, contactez [l’équipe de support technique Clarizen](https://success.clarizen.com/hc/en-us/requests/new).

1. Dans la section **Certificat de signature SAML**, cliquez sur **Créer un certificat**.

    ![Sélection de l’option « Créer un certificat »](./media/clarizen-tutorial/tutorial_clarizen_03.png)    

1. Dans la boîte de dialogue **Créer un certificat**, cliquez sur l’icône de calendrier et sélectionnez une date d’expiration. Cliquez ensuite sur **Enregistrer**.

    ![Sélection et enregistrement d’une date d’expiration](./media/clarizen-tutorial/tutorial_general_300.png)

1. Dans la section **Certificat de signature SAML**, sélectionnez **Activer le nouveau certificat**, puis cliquez sur **Enregistrer**.

    ![Sélection de la case à cocher pour activer le nouveau certificat](./media/clarizen-tutorial/tutorial_clarizen_04.png)

1. Dans la boîte de dialogue **Certificat de substitution**, cliquez sur **OK**.

    ![Sélection du bouton « OK » pour confirmer l’activation du certificat](./media/clarizen-tutorial/tutorial_general_400.png)

1. Dans la section **Certificat de signature SAML**, cliquez sur **Certificat (Base64)**, puis enregistrez le fichier du certificat sur votre ordinateur.

    ![Sélection de l’option « Certificat (Base64) » pour démarrer le téléchargement](./media/clarizen-tutorial/tutorial_clarizen_05.png)

1. Dans la section **Configuration de Clarizen** , cliquez sur **Configurer Clarizen** pour ouvrir la fenêtre **Configurer l’authentification**.

    ![Sélection de l’option « Configurer Clarizen »](./media/clarizen-tutorial/tutorial_clarizen_06.png)

    ![Fenêtre « Configurer l’authentification », incluant les URL et les fichiers](./media/clarizen-tutorial/tutorial_clarizen_07.png)

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre site d’entreprise Clarizen en tant qu’administrateur.

1. Cliquez sur votre nom d’utilisateur, puis sur **Settings** (Paramètres).

    ![Sélection de l’option « Settings » (Paramètres) sous votre nom d’utilisateur](./media/clarizen-tutorial/tutorial_clarizen_001.png "Settings") (Paramètres)

1. Cliquez sur l’onglet **Global Settings** (Paramètres globaux). En regard de la zone **Federated Authentication** (Authentification fédérée), cliquez sur **edit** (modifier).

    ![Onglet « Global Settings » (Paramètres globaux)](./media/clarizen-tutorial/tutorial_clarizen_002.png "Global Settings") (Paramètres globaux)

1. Dans la boîte de dialogue **Federated Authentication** (Authentification fédérée), procédez comme suit :

    ![Boîte de dialogue « Federated Authentication » (Authentification fédérée)](./media/clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication") (Authentification fédérée)

    a. Sélectionnez **Activer l'authentification fédérée**.

    b. Cliquez sur **Upload** pour charger votre certificat téléchargé.

    c. Dans la zone **Sign-in URL** (URL de connexion), entrez la valeur de la zone **URL du service d’authentification unique SAML** indiquée dans la fenêtre Configuration de l’application Azure AD.

    d. Dans la zone **Sign-in URL** (URL de connexion), entrez la valeur de la zone **URL de déconnexion** indiquée dans la fenêtre Configuration de l’application Azure AD.

    e. Sélectionnez **Use POST**.

    f. Cliquez sur **Enregistrer**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD
Dans le Portail Azure, créez un utilisateur de test appelé Britta Simon.

![Nom et adresse e-mail de l’utilisateur de test Azure AD][100]

1. Dans le volet gauche du Portail Azure, cliquez sur l’icône **Azure Active Directory**.

    ![Icône Azure Active Directory](./media/clarizen-tutorial/create_aaduser_01.png)

1. Cliquez sur **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs** pour afficher la liste des utilisateurs.

    ![Sélection des options « Utilisateurs et groupes » et « Tous les utilisateurs »](./media/clarizen-tutorial/create_aaduser_02.png)

1. En haut de la boîte de dialogue, cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Utilisateur**.

    ![Bouton « Ajouter »](./media/clarizen-tutorial/create_aaduser_03.png)

1. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :

    ![Boîte de dialogue « Utilisateur » renseignée avec le nom, l’adresse e-mail et le mot de passe](./media/clarizen-tutorial/create_aaduser_04.png)

    a. Dans la zone **Nom**, tapez **BrittaSimon**.

    b. Dans la zone **Nom d’utilisateur** , tapez l’adresse e-mail du compte de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur de la zone **Mot de passe**.

    d. Cliquez sur **Créer**.

### <a name="create-a-clarizen-test-user"></a>Créer un utilisateur de test Clarizen

L'objectif de cette section est de créer un utilisateur appelé Britta Simon dans Clarizen.

**Si vous avez besoin de créer un utilisateur manuellement, procédez comme suit :**

Pour permettre aux utilisateurs Azure AD de se connecter à Clarizen, vous devez approvisionner des comptes d’utilisateurs. Dans le cas de Clarizen, l’approvisionnement est une tâche manuelle.

1. Connectez-vous à votre site d’entreprise Clarizen en tant qu’administrateur.

2. Cliquez sur **People**.

    ![Sélection de l’option « People » (Contacts)](./media/clarizen-tutorial/create_aaduser_001.png "People") (Contacts)

3. Cliquez sur **Invite User**.

    ![Bouton « Invite User » (Inviter un utilisateur)](./media/clarizen-tutorial/create_aaduser_002.png "Invite Users") (Inviter un utilisateur)

1. Dans la boîte de dialogue **Invite People** (Inviter un contact), procédez comme suit :

    ![Boîte de dialogue « Invite People » (Inviter un contact)](./media/clarizen-tutorial/create_aaduser_003.png "Invite People") (Inviter un contact)

    a. Dans la zone **Email** (E-mail), tapez l’adresse e-mail du compte de Britta Simon.

    b. Cliquez sur **Invite**.

    > [!NOTE]
    > Le titulaire du compte Azure Active Directory reçoit un message électronique contenant un lien à suivre pour confirmer son compte et l’activer.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD
Autorisez Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Clarizen.

![Utilisateur de test affecté][200]

1. Dans le Portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, cliquez sur **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Sélection des options « Applications d’entreprise » et « Toutes les applications »][201]

1. Dans la liste des applications, sélectionnez **Clarizen**.

    ![Sélection de Clarizen dans la liste](./media/clarizen-tutorial/tutorial_clarizen_50.png)

1. Dans le volet gauche, cliquez sur **Utilisateurs et groupes**.

    ![Sélection de l’option « Utilisateurs et groupes »][202]

1. Cliquez sur le bouton **Add** . Ensuite, dans la boîte de dialogue **Ajouter une attribution**, sélectionnez **Utilisateurs et groupes**.

    ![Bouton « Ajouter » et boîte de dialogue « Ajouter une attribution »][203]

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

1. Dans la boîte de dialogue **Utilisateurs et groupes**, cliquez sur le bouton **Sélectionner**.

1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="test-single-sign-on"></a>Tester l’authentification unique
Testez la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la vignette Clarizen dans le volet d’accès, vous devez être automatiquement connecté à votre application Clarizen.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/clarizen-tutorial/tutorial_general_01.png
[2]: ./media/clarizen-tutorial/tutorial_general_02.png
[3]: ./media/clarizen-tutorial/tutorial_general_03.png
[4]: ./media/clarizen-tutorial/tutorial_general_04.png

[100]: ./media/clarizen-tutorial/tutorial_general_100.png

[200]: ./media/clarizen-tutorial/tutorial_general_200.png
[201]: ./media/clarizen-tutorial/tutorial_general_201.png
[202]: ./media/clarizen-tutorial/tutorial_general_202.png
[203]: ./media/clarizen-tutorial/tutorial_general_203.png