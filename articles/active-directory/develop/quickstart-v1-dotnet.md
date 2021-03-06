---
title: Bien démarrer avec Azure AD .NET Desktop (WPF) | Microsoft Docs
description: Création d’une application de bureau Windows .NET qui s’intègre à Azure AD pour la connexion et appelle des API protégées par Azure AD à l’aide d’OAuth.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c4bc777c36608ff9aeeec3428af0103fa3ae29c4
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578960"
---
# <a name="azure-ad-net-desktop-wpf-getting-started"></a>Bien démarrer avec Azure AD .NET Desktop (WPF)
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Si vous développez une application de bureau, Azure AD facilite l’authentification de vos utilisateurs avec leurs comptes Active Directory. Il permet également à votre application d’utiliser en toute sécurité une API web protégée par Azure AD, telle que l’API Office 365 ou Azure.

Pour les clients natifs .NET qui doivent accéder à des ressources protégées, Azure AD fournit la bibliothèque d’authentification Active Directory (bibliothèque ADAL). Le seul objectif de cette bibliothèque est de faciliter l’obtention des jetons d’accès pour votre application. Pour illustrer sa facilité d’utilisation, nous allons créer une application To-Do List WPF .NET qui effectue les actions suivantes :

* obtention de jetons d’accès pour appeler l’API Azure AD Graph à l’aide du [protocole d’authentification OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx);
* recherche, dans un répertoire, d’utilisateurs correspondant à un alias donné ;
* déconnexion des utilisateurs.

Pour générer l’application fonctionnelle complète, vous devez :

1. inscrire votre application auprès d’Azure AD ;
2. installer et configurer la bibliothèque ADAL ;
3. utiliser la bibliothèque ADAL pour obtenir des jetons à partir d’Azure AD.

Pour commencer, téléchargez [la structure de l’application](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) ou [l’exemple terminé](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Vous avez également besoin d’un client Azure AD dans lequel vous pouvez créer des utilisateurs et inscrire une application. Si vous ne disposez pas encore d’un client, [découvrez comment en obtenir un](quickstart-create-new-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>1. Inscription de l’application DirectorySearcher
Pour autoriser votre application à obtenir des jetons, vous devez tout d’abord l’enregistrer dans votre client Azure AD et lui accorder l’autorisation d’accéder à l’API Graph Azure AD :

1. Connectez-vous au [Portail Azure](https://portal.azure.com).
2. Dans la barre supérieure, cliquez sur votre compte et, dans la liste **Répertoire**, choisissez le locataire Active Directory auprès duquel vous voulez inscrire votre application.
3. Cliquez sur **Tous les services** dans le volet de navigation de gauche, puis choisissez **Azure Active Directory**.
4. Cliquez sur **Inscriptions des applications**, puis sur **Ajouter**.
5. Suivez les invites et créez une **application cliente native**.
  * Le **nom** de l’application doit décrire votre application aux utilisateurs finaux.
  * L’ **URI de redirection** est une combinaison de schémas et de chaînes qu’Azure AD utilise pour renvoyer des réponses concernant les jetons. Entrez une valeur spécifique de votre application, par exemple, `http://DirectorySearcher`.
6. Une fois l’inscription terminée, AAD affecte un ID d’application unique à votre application. Copiez cette valeur à partir de la page de l’application, car vous en aurez besoin dans les sections suivantes.
7. Sur la page **Paramètres**, choisissez **Autorisations requises** et **Ajouter**. Sélectionnez l’API **Microsoft Graph** et ajoutez l’autorisation **Lire les données de l’annuaire** sous **Autorisations déléguées**. Cela permet à votre application d’interroger l’API Graph concernant les utilisateurs.

## <a name="2-install--configure-adal"></a>2. Installez et configurez ADAL
Maintenant que vous disposez d’une application dans Azure AD, vous pouvez installer la bibliothèque ADAL et écrire votre code lié à l’identité. Pour permettre à ADAL de communiquer avec Azure AD, vous devez lui fournir des informations sur l’inscription de votre application.

* Commencez par ajouter ADAL au projet DirectorySearcher à l’aide de la console du gestionnaire de package.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Dans le projet DirectorySearcher, ouvrez `app.config`. Remplacez les valeurs des éléments de la section `<appSettings>` afin qu’elles reflètent les valeurs que vous avez saisies dans le portail Azure. Votre code se réfère à ces valeurs chaque fois qu’il utilise la bibliothèque ADAL.
  * `ida:Tenant` est le domaine de votre client Azure AD, par exemple, contoso.onmicrosoft.com.
  * `ida:ClientId` est l’ID client de votre application que vous avez copié à partir du portail.
  * `ida:RedirectUri` est l’URL de redirection que vous avez inscrite dans le portail.

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Utilisation de la bibliothèque ADAL pour obtenir des jetons à partir d’AAD
Le principe de base de la bibliothèque ADAL consiste simplement à appeler `authContext.AcquireTokenAsync(...)` chaque fois que votre application a besoin d’un jeton d’accès, et la bibliothèque ADAL s’occupe du reste. 

* Dans le projet `DirectorySearcher`, ouvrez `MainWindow.xaml.cs` et recherchez la méthode `MainWindow()`. La première étape est d’initialiser `AuthenticationContext` pour votre application (classe principale de la bibliothèque ADAL). C’est à ce moment-là que vous fournissez à la bibliothèque ADAL les coordonnées dont elle a besoin pour communiquer avec Azure AD et lui indiquer comment mettre en cache des jetons.

```csharp
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Recherchez maintenant la méthode `Search(...)` qui est appelée lorsque l’utilisateur clique sur le bouton « Rechercher » dans l’interface utilisateur de l’application. Cette méthode effectue une demande GET auprès de l’API Graph Azure AD pour l’interroger à propos d’utilisateurs dont l’UPN commence par le terme de recherche donné. Cependant, pour interroger l’API Graph, vous devez inclure un jeton d’accès (access_token) dans l’en-tête `Authorization` de la demande ; c’est à ce moment qu’intervient la bibliothèque ADAL.

```csharp
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* Lorsque votre application demande un jeton en appelant `AcquireTokenAsync(...)`, la bibliothèque ADAL tente de renvoyer un jeton sans demander à l’utilisateur ses informations d’identification. Si la bibliothèque ADAL détermine que l’utilisateur doit se connecter pour obtenir un jeton, elle affiche une boîte de dialogue de connexion, récupère les informations d’identification de l’utilisateur et renvoie un jeton après une authentification réussie. Si la bibliothèque ADAL ne peut pas renvoyer un jeton pour une raison quelconque, `AdalException`est renvoyé.
* Notez que l’objet `AuthenticationResult` contient un objet `UserInfo` qui peut être utilisé pour collecter des informations dont votre application peut avoir besoin. Dans DirectorySearcher, `UserInfo` est utilisé pour personnaliser l’interface utilisateur de l’application avec l’ID de l’utilisateur.
* Lorsque l’utilisateur clique sur le bouton Déconnexion, nous voulons vérifier que l’appel suivant vers `AcquireTokenAsync(...)` demande à l’utilisateur de se connecter. Avec la bibliothèque ADAL, c’est aussi simple que d’effacer le cache de jeton :

```csharp
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Toutefois, si l’utilisateur ne clique pas sur le bouton Déconnexion, vous devez maintenir la session de l’utilisateur pour sa prochaine exécution de DirectorySearcher. Lorsque l’application démarre, vous pouvez rechercher dans le cache de jetons de la bibliothèque ADAL un jeton existant et mettre à jour l’interface utilisateur en conséquence. Dans la méthode `CheckForCachedToken()`, passez un autre appel à `AcquireTokenAsync(...)`, cette fois en transmettant le paramètre `PromptBehavior.Never`. `PromptBehavior.Never` indiquera à ADAL que l’utilisateur ne doit pas être invité à se connecter et que la bibliothèque ADAL doit à la place lever une exception s’il est impossible de renvoyer un jeton.

```csharp
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user. If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Félicitations ! Vous disposez d’une application .NET WPF fonctionnelle capable d’authentifier les utilisateurs, d’appeler en toute sécurité les API web à l’aide d’OAuth 2.0 et d’obtenir des informations de base concernant l’utilisateur. Si vous ne l’avez pas encore fait, il est maintenant temps de remplir votre client avec quelques utilisateurs. Exécutez votre application DirectorySearcher et connectez-vous avec l’un de ces utilisateurs. Recherchez d’autres utilisateurs en fonction de leur UPN. Fermez l’application et exécutez-la de nouveau. Observez que la session utilisateur reste identique. Déconnectez-vous et reconnectez-vous sous le nom d’un autre utilisateur.

La bibliothèque ADAL facilite l’intégration de toutes ces fonctionnalités d’identité communes dans votre application. Elle effectue les tâches ingrates pour vous : gestion du cache, prise en charge du protocole OAuth, présentation d’une interface utilisateur de connexion à l’utilisateur, actualisation des jetons expirés et bien plus encore. La seule chose que vous devez vraiment connaître est un appel unique d’API : `authContext.AcquireTokenAsync(...)`.

Pour référence, l’exemple terminé (sans vos valeurs de configuration) est fourni [ici](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). Vous pouvez à présent aborder d’autres scénarios. Par exemple :

[Sécurisation d’une API web .NET avec Azure AD &gt;&gt;](quickstart-v1-dotnet-webapi.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

