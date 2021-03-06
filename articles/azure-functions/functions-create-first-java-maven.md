---
title: Créer votre première fonction dans Azure avec Java et Maven | Microsoft Docs
description: Créez et publiez une fonction simple déclenchée par HTTP dans Azure avec Java et Maven.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure functions, functions, traitement des événements, calcul, architecture sans serveur
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 16d6dd6a589256ad98a37465e64e847778d0cc7e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092591"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Créer votre première fonction dans Azure avec Java et Maven (aperçu)

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

Ce démarrage rapide vous guide dans la création d’un projet de fonction [sans serveur](https://azure.microsoft.com/overview/serverless-computing/) avec Maven, dans le test de ce projet localement et son déploiement sur Azure Functions. Quand vous avez terminé, vous disposez d’une application de fonction déclenchée par HTTP qui s’exécute dans Azure.

![Accéder à une fonction Hello World à partir de la ligne de commande avec cURL](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prérequis
Pour développer une application de fonction avec Java, les éléments suivants doivent être installés :

-  [Java Developer Kit (JDK)](https://www.azul.com/downloads/zulu/) version 8.
-  [Apache Maven](https://maven.apache.org) version 3.0 ou ultérieure.
-  [interface de ligne de commande Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Pour pouvoir effectuer ce démarrage rapide, vous devez définir la variable d’environnement JAVA_HOME sur l’emplacement d’installation du JDK.

## <a name="install-the-azure-functions-core-tools"></a>Installer Azure Functions Core Tools

Azure Functions Core Tools fournit un environnement de développement local pour l’écriture, l’exécution et le débogage des fonctions Azure Functions depuis le terminal ou une invite de commandes. 

Installez la [version 2 de Core Tools](functions-run-local.md#v2) sur votre ordinateur local avant de continuer.

## <a name="generate-a-new-functions-project"></a>Générer un nouveau projet Functions

Dans un dossier vide, exécutez la commande suivante pour générer le projet Functions à partir d’un [archétype Maven](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven vous invite à entrer les valeurs nécessaires pour terminer la génération du projet. Pour les valeurs de _groupId_, _artifactId_ et _version_, consultez les informations de référence sur les [conventions de nommage de Maven](https://maven.apache.org/guides/mini/guide-naming-conventions.html). La valeur de _appName_ doit être unique dans Azure : par défaut, Maven génère donc un nom d’application basé sur la valeur précédemment entrée pour _artifactId_. La valeur de _packageName_ détermine le package Java pour le code de fonction généré.

La valeur `appRegion` spécifie la [région Azure](https://azure.microsoft.com/global-infrastructure/regions/) dans laquelle vous souhaitez exécuter l’application de fonction déployée. Vous pouvez obtenir une liste de valeurs de nom de région par le biais de la commande `az account list-locations` dans l’interface de ligne de commande Azure. La valeur `resourceGroup` spécifie le groupe de ressources Azure dans lequel l’application de fonction sera créée.

Les identificateurs `com.fabrikam.functions` et `fabrikam-functions` ci-dessous sont utilisés en tant qu’exemples et pour faciliter la lecture des étapes ultérieures de ce démarrage rapide. Vous êtes invité à fournir vos propres valeurs pour Maven dans cette étape.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Define value for property 'appRegion' westus : 
Define value for property 'resourceGroup' java-functions-group: 
Confirm properties configuration: Y
```

Maven crée les fichiers projet dans un nouveau dossier avec le nom de _artifactId_, dans cet exemple `fabrikam-functions`. Le code généré et prêt à être utilisé dans le projet est une fonction simple [déclenchée par HTTP](/azure/azure-functions/functions-bindings-http-webhook) qui renvoie le corps de la requête après une chaîne « Hello ».

```java
public class Function {
    @FunctionName("HttpTrigger-Java")
    public HttpResponseMessage HttpTriggerJava(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
        } else {
            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        }
    }
}
```

## <a name="run-the-function-locally"></a>Exécuter la fonction localement

Accédez au dossier du projet nouvellement créé, puis générez et exécutez la fonction avec Maven :

```
cd fabrikam-functions
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Si vous rencontrez cette exception : `javax.xml.bind.JAXBException` avec Java 9, consultez la solution de contournement sur [GitHub](https://github.com/jOOQ/jOOQ/issues/6477).

Vous voyez cette sortie lorsque la fonction s’exécute localement sur votre système et quand elle est prête à répondre aux requêtes HTTP :

```Output
Listening on http://0.0.0.0:7071/
Hit CTRL-C to exit...

Http Functions:

        HttpTrigger-Java: http://localhost:7071/api/HttpTrigger-Java
```

Déclenchez la fonction à partir de la ligne de commande en utilisant cURL dans une nouvelle fenêtre de terminal :

```
curl -w '\n' -d LocalFunctionTest http://localhost:7071/api/HttpTrigger-Java
```

```Output
Hello, LocalFunctionTest
```

Utilisez `Ctrl-C` dans le terminal pour arrêter le code de la fonction.

## <a name="deploy-the-function-to-azure"></a>Déployer la fonction sur Azure

Le processus de déploiement sur Azure Functions utilise les informations d’identification du compte provenant d’Azure CLI. [Connectez-vous à Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) avant de continuer.

```azurecli
az login
```

Déployez votre code dans une nouvelle application de fonction en utilisant la cible Maven `azure-functions:deploy`.

```
mvn azure-functions:deploy
```

Quand le déploiement est terminé, vous voyez l’URL que vous pouvez utiliser pour accéder à votre application de fonction Azure :

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Testez l’application de fonction en cours d’exécution sur Azure avec `cURL`. Vous devez modifier l’URL de l’exemple ci-dessous pour qu’elle corresponde à l’URL déployée de votre application de fonction à l’étape précédente.

```
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

```Output
Hello, AzureFunctionsTest
```

## <a name="make-changes-and-redeploy"></a>Effectuer des modifications et redéployer

Modifiez le fichier source `src/main.../Function.java` dans le projet généré pour modifier le texte retourné par votre application de fonction. Remplacez cette ligne :

```java
return request.createResponse(200, "Hello, " + name);
```

par ce qui suit :

```java
return request.createResponse(200, "Hi, " + name);
```

Enregistrez les modifications et redéployez l’application en exécutant `azure-functions:deploy` à partir du terminal comme vous l’avez fait précédemment. L’application de fonction sera mise à jour et cette requête :

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

retournera la sortie mise à jour suivante :

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>Étapes suivantes

Vous avez créé une application de fonction Java avec un déclencheur HTTP simple et vous l’avez déployée sur Azure Functions.

- Pour plus d’informations sur le développement de fonctions Java, consultez le [Guide du développeur de fonctions Java](functions-reference-java.md).
- Ajoutez des fonctions supplémentaires avec différents déclencheurs à votre projet en utilisant la cible Maven `azure-functions:add`.
- Écrivez et déboguez des fonctions en local avec [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) et [Eclipse](functions-create-maven-eclipse.md). 
- Déboguez les fonctions déployées dans Azure avec Visual Studio Code. Pour obtenir des instructions, consultez la documentation relative aux [applications Java sans serveur](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) Visual Studio Code.
