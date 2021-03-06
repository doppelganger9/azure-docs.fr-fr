---
title: Sécurité et authentification Azure Event Grid
description: Détaille le service Azure Event Grid et ses concepts.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: babanisa
ms.openlocfilehash: ce0e766a07fd19f523f1f35b9a3cbc865cfb8c71
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42144210"
---
# <a name="event-grid-security-and-authentication"></a>Sécurité et authentification Azure Event Grid 

Azure Event Grid dispose de trois types d’authentification :

* Abonnements à des événements
* Publication d’événement
* Remise d’événement WebHook

## <a name="webhook-event-delivery"></a>Remise d’événement WebHook

Un Webhook constitue l’un des nombreux moyens de recevoir des événements provenant d’Azure Event Grid. Quand un nouvel événement est prêt, le service EventGrid envoie une requête HTTP (POST) au point de terminaison configuré avec l’événement dans le corps de la requête.

Comme de nombreux autres services qui prennent en charge les Webhooks, EventGrid vous demande de prouver que vous êtes « propriétaire » de votre point de terminaison Webhook avant de démarrer la diffusion d’événements vers ce point de terminaison. Cette exigence vise à empêcher un point de terminaison non averti de devenir la cible pour la remise des événements à partir de EventGrid. Toutefois, lorsque vous utilisez un des trois services Azure répertoriés ci-dessous, l’infrastructure Azure gère automatiquement cette validation :

* Azure Logic Apps
* Azure Automation
* Azure Functions pour le déclencheur EventGrid

Si vous utilisez un autre type de point de terminaison, comme une fonction Azure basée sur un déclencheur HTTP, le code de votre point de terminaison doit participer à l’établissement de liaison d’une validation avec EventGrid. EventGrid prend en charge deux modèles d’établissement de liaison de validation différents :

1. **Établissement de liaison ValidationCode** : au moment de la création de l’abonnement à l’événement, EventGrid envoie une requête POST d’« événement de validation d’abonnement » à votre point de terminaison. Le schéma de cet événement est semblable à n’importe quel autre EventGridEvent, et la partie données de cet événement inclut une propriété `validationCode`. Une fois que l’application a confirmé que la requête de validation concerne un abonnement d’événement attendu, votre code d’application doit répondre en renvoyant le code de validation à EventGrid. Ce mécanisme d’établissement de liaison est pris en charge dans toutes les versions d’EventGrid.

2. **Établissement de liaison ValidationURL (établissement manuel)** : dans certains cas, vous ne contrôlez pas le code source du point de terminaison et ne pouvez donc pas implémenter l’établissement de liaison en fonction de ValidationCode. Par exemple, si vous utilisez un service tiers (comme [Zapier](https://zapier.com) ou [IFTTT](https://ifttt.com/)), vous risquez de ne pas pouvoir renvoyer le code de validation par programmation. Par conséquent, à compter de la version 2018-05-01-preview, EventGrid prend désormais en charge un établissement de liaison de validation manuel. Si vous créez un abonnement d’événement à l’aide de kits de développement logiciel/outils qui utilisent cette nouvelle version d’API (2018-05-01-preview), EventGrid envoie une propriété `validationUrl` (en plus de la propriété `validationCode`) incluse dans la partie des données de la validation d’abonnement d’événement. Pour terminer l’établissement de liaison, il vous suffit d’envoyer une requête GET sur cette URL, via un client REST ou à l’aide de votre navigateur web. L’URL de validation fournie est valide uniquement pendant 10 minutes environ. Pendant ce temps, l’état d’approvisionnement de l’abonnement aux événements est `AwaitingManualAction`. Si vous n’effectuez pas la validation manuelle dans les 10 minutes, l’état d’approvisionnement est défini sur `Failed`. Vous devrez retentez la création de l’abonnement aux événements avant d’essayer d’effectuer la validation manuelle à nouveau.

Le mécanisme de validation manuelle est en préversion. Pour l’utiliser, vous devez installer [l’extension Event Grid](/cli/azure/azure-cli-extensions-list) pour [AZ CLI 2.0](/cli/azure/install-azure-cli). Vous pouvez l’installer avec `az extension add --name eventgrid`. Si vous utilisez l’API REST, assurez-vous d’utiliser `api-version=2018-05-01-preview`.

### <a name="validation-details"></a>Détails de validation

* Lors de la création/mise à jour de l’abonnement d’événement, Event Grid publie un événement de validation d’abonnement dans le point de terminaison cible. 
* L’événement contient une valeur d’en-tête « aeg-event-type: SubscriptionValidation ».
* Le corps de l’événement dispose du même schéma que les autres événements Event Grid.
* La propriété eventType de l’événement est « Microsoft.EventGrid.SubscriptionValidationEvent ».
* La propriété de données de l’événement inclut une propriété « validationCode » avec une chaîne générée de façon aléatoire, par exemple « validationCode: acb13… ».
* Si vous utilisez la version d’API 2018-05-01-preview, les données d’événement incluent également une propriété `validationUrl` avec une URL pour la validation manuelle de l’abonnement.
* Le tableau contient uniquement l’événement de validation. Les autres événements sont envoyés dans une requête distincte, une fois que vous avez renvoyé le code de validation.
* Les kits de développement logiciel DataPlane EventGrid possèdent des classes correspondant aux données d’événement de validation d’abonnement et à la réponse de validation d’abonnement.

Un exemple de SubscriptionValidationEvent est illustré ci-dessous :

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=B2E34264-7D71-453A-B5FB-B62D0FDC85EE&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1BNqCxBBSSE9OnNSfZM4%2b5H9zDegKMY6uJ%2fO2DFRkwQ%3d"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Pour prouver que vous êtes propriétaire du point de terminaison, renvoyez le code de validation dans la propriété validationResponse, comme indiqué dans l’exemple suivant :

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Sinon, vous pouvez valider manuellement l’abonnement en envoyant une requête GET à l’URL de validation. L’abonnement aux événements reste dans un état d’attente jusqu’à ce qu’il soit validé.

Vous trouverez un échantillon C# qui montre comment gérer l’établissement de liaison pour la validation d’abonnement à l’adresse https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs.

### <a name="checklist"></a>Liste de contrôle

Lors de la création de l’abonnement d’événement, si vous voyez un message d’erreur indiquant que la tentative de validation a échoué pour le point de terminaison https://your-endpoint-here indiqué, et vous invitant à rechercher plus d’informations dans https://aka.ms/esvalidation, cela signifie qu’il existe une défaillance dans l’établissement de liaison de validation. Pour résoudre cette erreur, vérifiez les points suivants :

* Contrôlez-vous le code d’application dans le point de terminaison cible ? Par exemple, si vous écrivez un déclencheur HTTP basé sur Azure Function, avez-vous accès au code d’application pour apporter des modifications ?
* Si vous avez accès au code d’application, implémentez le mécanisme d’établissement de liaison ValidationCode comme dans l’exemple ci-dessus.

* Si vous n’avez pas accès au code d’application (par exemple, si vous utilisez un service tiers qui prend en charge les Webhooks), vous pouvez utiliser le mécanisme d’établissement de liaison manuel. Pour ce faire, vérifiez que vous utilisez la version d’API 2018-05-01-preview (par exemple, à l’aide de l’extension EventGrid CLI décrite ci-dessus) pour recevoir la propriété validationUrl de l’événement de validation. Pour terminer l’établissement de liaison de validation manuel, obtenez la valeur de la propriété « validationUrl » et accédez à cette URL dans votre navigateur web. Si la validation est réussie, le navigateur web affiche un message de succès, et vous voyez que la propriété provisioningState de l’abonnement d’événement a la valeur « Réussi ». 

### <a name="event-delivery-security"></a>Sécurité de la remise des événements

Vous pouvez sécuriser votre point de terminaison Webhook en ajoutant des paramètres de requête à l’URL Webhook lorsque vous créez un abonnement à un événement. Configurez l’un de ces paramètres de requête comme un secret, par exemple, un [jeton d’accès](https://en.wikipedia.org/wiki/Access_token) que le Webhook peut utiliser pour identifier l’événement qui est envoyé par Event Grid avec des autorisations valides. Event Grid va inclure ces paramètres de requête dans chaque remise d’événement au Webhook.

Lorsque vous modifiez l’abonnement aux événements, les paramètres de requête ne sont pas affichés ni retournés, sauf si le paramètre [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) est utilisé dans [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest).

Enfin, il est important de noter qu’Azure Event Grid ne prend en charge que les points de terminaison de Webhook HTTPS.

## <a name="event-subscription"></a>Abonnement à un événement

Pour vous abonner à un événement, vous devez disposer de l’autorisation **Microsoft.EventGrid/EventSubscriptions/Write** sur la ressource nécessaire. Vous avez besoin de cette autorisation, car vous rédigez un nouvel abonnement dans la portée de la ressource. La ressource nécessaire diffère si vous vous abonnez à une rubrique du système ou à une rubrique personnalisée. Les deux types sont décrits dans cette section.

### <a name="system-topics-azure-service-publishers"></a>Rubriques du système (éditeurs du service Azure)

Dans les rubriques du système, il vous faut l’autorisation de rédiger un nouvel abonnement à un événement dans la portée de la ressource qui publie l’événement. La ressource est au format suivant : `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Par exemple, pour vous abonner à un événement sur un compte de stockage nommé **myacct**, il vous faut l’autorisation Microsoft.EventGrid/EventSubscriptions/Write sur :`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Rubriques personnalisées

Dans les rubriques personnalisées, vous avez besoin de l’autorisation de rédiger un nouvel abonnement à un événement dans la portée de la rubrique Event Grid. La ressource est au format suivant : `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Par exemple, pour vous abonner à une rubrique personnalisée nommée **mytopic**, il vous faut l’autorisation Microsoft.EventGrid/EventSubscriptions/Write sur :`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Publication d’une rubrique

Les rubriques utilisent une Signature d’accès partagé (SAP) ou une authentification par clé. Nous vous recommandons la SAP, mais l’authentification par clé propose une programmation simple et est compatible avec de nombreux éditeurs de webhook existants. 

Vous incluez la valeur d’authentification dans l’en-tête HTTP. Pour une SAP, utilisez **aeg-sas-token** en valeur de l’en-tête. Pour une authentification par clé, utilisez **aeg-sas-key** en valeur de l’en-tête.

### <a name="key-authentication"></a>Authentification par clé

L’authentification par clé est la plus simple. Utilisez le format : `aeg-sas-key: <your key>`

Par exemple, vous transmettez une clé avec :

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Jetons SAS

Les jetons SAP pour Event Grid incluent la ressource, un délai d’expiration et une signature. Le jeton SAP est au format suivant : `r={resource}&e={expiration}&s={signature}`.

La ressource représente le chemin de la rubrique Event Grid à laquelle vous envoyez des événements. Par exemple, un chemin d’accès de ressource valide est :`https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Vous générez la signature à partir d’une clé.

Par exemple, une valeur **aeg-sas-token** valide est :

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

L’exemple suivant crée un jeton SAP utilisable avec Event Grid :

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Contrôle d’accès à la gestion

Azure Event Grid vous permet de contrôler le niveau d’accès offert aux utilisateurs, leur permettant d’effectuer différentes opérations de gestion telles que répertorier et créer des abonnements aux événements et générer des clés. Event Grid utilise le contrôle d’accès en fonction du rôle (RBAC) d’Azure.

### <a name="operation-types"></a>Types d’opération

Azure Event Grid prend en charge les actions suivantes :

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Les trois dernières opérations retournent des informations potentiellement confidentielles qui sont filtrées lors d’opérations de lecture normales. Nous vous recommandons de restreindre l’accès à ces opérations. Il est possible de créer des rôles personnalisés à l’aide [d’Azure PowerShell](../role-based-access-control/role-assignments-powershell.md), de [l’interface CLI Azure](../role-based-access-control/role-assignments-cli.md) et de [l’API REST](../role-based-access-control/role-assignments-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Application de la vérification d’accès par rôle (RBAC)

Pour appliquer la vérification d’accès par rôle, pour différents utilisateurs, procédez comme suit :

#### <a name="create-a-custom-role-definition-file-json"></a>Créer un fichier de définition de rôle personnalisé (.json)

Voici des exemples de définitions de rôle dans Event Grid permettant aux utilisateurs d’effectuer différentes actions.

**EventGridReadOnlyRole.json** : pour autoriser uniquement les opérations en lecture seule.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json** : pour autoriser des actions de publication limitées, et interdire les actions de suppression.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: pour autoriser toutes les actions dans Event Grid.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

#### <a name="create-and-assign-custom-role-with-azure-cli"></a>Créer et affecter un rôle personnalisé avec l’interface de ligne de commande Azure

Pour créer un rôle personnalisé, utilisez :

```azurecli
az role definition create --role-definition @<file path>
```

Pour affecter le rôle à un utilisateur, utilisez :

```azurecli
az role assignment create --assignee <user name> --role "<name of role>"
```

## <a name="next-steps"></a>Étapes suivantes

* Pour une présentation d’Event Grid, consultez [Event Grid](overview.md)
