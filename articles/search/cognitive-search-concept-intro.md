---
title: Recherche cognitive pour l’extraction des données et le traitement de l’IA en langage naturel dans Recherche Azure | Microsoft Docs
description: Extraction de contenu, traitement en langage naturel et traitement d’images pour créer du contenu pouvant être recherché dans l’indexation Recherche Azure à l’aide de compétences cognitives et d’algorithmes d’IA.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: heidist
ms.openlocfilehash: 72d1630ecaeada3acf8b49952a31ccd3ae8634aa
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617956"
---
# <a name="what-is-cognitive-search"></a>Qu’est-ce que la recherche cognitive ?

La recherche cognitive crée des informations consultables à partir de contenu sans possibilité de recherche en attachant des algorithmes d’intelligence artificielle à un pipeline d’indexation. L’intégration IA se fait via des *compétences cognitives* qui enrichissent les documents sources dans l’itinéraire d’un index de recherche. 

Les compétences de **traitement en langage naturel** incluent la [reconnaissance d’entité](cognitive-search-skill-named-entity-recognition.md), la détection de la langue, l’[extraction de phrases clés](cognitive-search-skill-keyphrases.md), la manipulation de texte et la détection de sentiments. Avec ces compétences, un texte non structuré devient structuré, mappé avec des champs interrogeables et filtrés dans un index.

Le **traitement d’image** inclut la [reconnaissance optique de caractères (OCR)](cognitive-search-skill-ocr.md) et l’identification des [caractéristiques visuelles](cognitive-search-skill-image-analysis.md), telles que la détection des visages, l’interprétation des images, la reconnaissance des images (monuments et personnes célèbres) ou des attributs tels que les couleurs ou l’orientation des images. Vous pouvez créer des représentations textuelles pour le contenu des images, dans lesquelles effectuer des recherches à l’aide de toutes les fonctionnalités de requête de la Recherche Azure.

![Schéma du pipeline de la recherche cognitive](./media/cognitive-search-intro/cogsearch-architecture.png "Vue d’ensemble du pipeline de la recherche cognitive")

Les compétences cognitives de la Recherche Azure reposent sur les algorithmes d’IA utilisés dans les API Cognitive Services : [API de reconnaissance d’entité nommée](cognitive-search-skill-named-entity-recognition.md), [API d’extraction de phrases clés](cognitive-search-skill-keyphrases.md) et [API OCR](cognitive-search-skill-ocr.md), pour n’en citer que quelques-unes. 

Le traitement en langage naturel et le traitement d’image sont appliqués pendant la phase d’ingestion des données, et les résultats sont intégrés à la composition d’un document sous la forme d’un index consultable dans la Recherche Azure. Les données sont fournies en tant que jeu de données Azure, puis transmises via un pipeline d’indexation à l’aide des [compétences intégrées](cognitive-search-predefined-skills.md) dont vous avez besoin. L’architecture est extensible. Par conséquent, si les compétences intégrées ne sont pas suffisantes, vous pouvez créer et attacher des [compétences personnalisées](cognitive-search-create-custom-skill-example.md) pour intégrer un traitement personnalisé. Par exemple, il peut s’agir d’un module d’entité ou d’un classifieur de documents ciblant un domaine spécifique comme la finance, les publications scientifiques ou la médecine.

> [!NOTE]
> La recherche cognitive est en préversion publique et l’exécution de compétences est proposée gratuitement à l’heure actuelle. Le prix de cette fonctionnalité sera annoncé à une date ultérieure. 

## <a name="components-of-cognitive-search"></a>Composants de la recherche cognitive

La recherche cognitive est une fonctionnalité en préversion de la [Recherche Azure](search-what-is-azure-search.md), disponible quel que soit le niveau dans les régions USA Centre Sud et Europe Ouest. 

Le pipeline de la recherche cognitive repose sur des *indexeurs* [Recherche Azure](search-indexer-overview.md) qui analysent les sources de données et traitent les index du début à la fin. Les compétences sont désormais jointes aux indexeurs, ce qui permet d’intercepter et d’enrichir les documents en fonction de l’ensemble de compétences que vous définissez. Une fois l’indexation terminée, vous pouvez accéder au contenu via des requêtes de recherche par le biais de tous les [types de requête pris en charge par Recherche Azure](search-query-overview.md).  Si vous ne connaissez pas les indexeurs, cette section vous guide tout au long des étapes.

### <a name="source-data-and-document-cracking-phase"></a>Phase de décodage du document et des données sources

Au début du pipeline, vous avez du texte non structuré ou du contenu non textuel (comme des images et des fichiers JPEG de documents numérisés). Les données doivent se trouver dans un service de stockage de données Azure accessible par un indexeur. Les indexeurs peuvent décoder les documents sources pour extraire le texte à partir des données sources.

![Phase de décodage du document](./media/cognitive-search-intro/document-cracking-phase-blowup.png "Décodage du document")

 Les sources prises en charge comprennent le stockage d’objets blob Azure, le stockage de table Azure, Azure SQL Database et Azure Cosmos DB. Le contenu textuel peut être extrait des types de fichier suivants : PDF, Word, PowerPoint et CSV. Pour obtenir la liste complète, consultez [Formats de document pris en charge](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Phase d’enrichissement et compétences cognitives

L’enrichissement se fait via les *compétences cognitives* effectuant des opérations atomiques. Par exemple, une fois que vous avez du contenu textuel issu d’un fichier PDF, vous pouvez appliquer la détection du langage de reconnaissance d’entité ou l’extraction de phrases clés pour créer des champs (qui ne sont pas disponibles en natif dans la source) dans votre index. La collection de compétences utilisée dans votre pipeline est appelée *ensemble de compétences*.  

![Phase d’enrichissement](./media/cognitive-search-intro/enrichment-phase-blowup.png "phase d’enrichissement")

Un ensemble de compétences est basé sur des [compétences cognitives prédéfinies](cognitive-search-predefined-skills.md) ou des [compétences personnalisées](cognitive-search-create-custom-skill-example.md) que vous fournissez et reliez à l’ensemble de compétences. Un ensemble de compétences peut être minimal ou très complexe, et détermine non seulement le type de traitement, mais également l’ordre des opérations. Un ensemble de compétences plus les mappages de champ définis dans un indexeur spécifient entièrement le pipeline d’enrichissement. Pour plus d’informations sur l’extraction de tous ces éléments ensemble, consultez [How to create a skillset in an enrichment pipeline](cognitive-search-defining-skillset.md) (Créer un ensemble de compétences dans un pipeline d’enrichissement).

En interne, le pipeline génère une collection de documents enrichis. Vous pouvez décider quelles parties des documents enrichis doivent être mappées aux champs indexables dans votre index de recherche. Par exemple, si vous avez appliqué les compétences de reconnaissance d’entité et d’extraction de phrases clés, ces nouveaux champs vont faire partie du document enrichi et peuvent donc être mappés aux champs sur votre index. Consultez [How to reference annotations in a cognitive search skillset](cognitive-search-concept-annotations-syntax.md) (Référencer des annotations dans un ensemble de compétences de recherche cognitive) pour en savoir plus sur les formations entrée/sortie.

### <a name="search-index-and-query-based-access"></a>Accès basé sur des requêtes et index de recherche

Lorsque le traitement est terminé, vous disposez d’un corpus de recherche comprenant des documents enrichis, pouvant faire l’objet d’une recherche textuelle dans Recherche Azure. [L’interrogation de l’index](search-query-overview.md) est la façon dont les développeurs et les utilisateurs accèdent au contenu enrichi généré par le pipeline. 

![Icône d’index avec recherche](./media/cognitive-search-intro/search-phase-blowup.png "Icône d’index avec recherche")

L’index est semblable à tous ceux que vous pouvez créer dans Recherche Azure : vous pouvez ajouter des analyseurs personnalisés, appeler des requêtes de recherche partielle, ajouter une recherche filtrée ou essayer des profils de score pour remodeler les résultats de la recherche.

Les index sont générés à partir d’un schéma d’index qui définit les champs, les attributs et d’autres constructions jointes à un index spécifique, telles que les profils de score et les cartes de synonymes. Une fois qu’un index est défini et rempli, vous pouvez effectuer des indexations incrémentielles pour sélectionner de nouveaux documents sources et des documents sources mis à jour. Certaines modifications requièrent une régénération complète. Vous devez utiliser un petit jeu de données tant que la conception du schéma n’est pas stable. Pour plus d’informations, consultez [How to rebuild an Azure Search index](search-howto-reindex.md) (Régénérer un index Recherche Azure).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Principaux concepts et fonctionnalités

| Concept | Description| Liens |
|---------|------------|-------|
| Ensemble de compétences | Une ressource nommée de niveau supérieur comprenant une collection de compétences. Un ensemble de compétences correspond au pipeline d’enrichissement. Il est appelé lors de l’indexation par un indexeur. | [How to create a skillset in an enrichment pipeline](cognitive-search-defining-skillset.md) (Créer un ensemble de compétences dans un pipeline d’enrichissement) |
| Compétence cognitive | Une transformation atomique dans un pipeline d’enrichissement. Souvent, il s’agit d’un composant qui effectue des extractions ou déduit une structure, renforçant ainsi notre compréhension des données d’entrée. La sortie est presque toujours textuelle et le traitement est de type traitement en langage naturel ou traitement d’images qui extrait ou génère du texte à partir d’entrées d’images. La sortie d’une compétence peut être mappée à un champ dans un index ou utilisée en tant qu’entrée pour un enrichissement en aval. Une compétence est soit prédéfinie et fournie par Microsoft, soit personnalisée, c’est-à-dire créée et déployée par vous. | [Compétences prédéfinies](cognitive-search-predefined-skills.md) |
| Extraction de données | Couvre une large gamme de traitements, mais dans le cadre de la recherche cognitive, la compétence de reconnaissance d’entité nommée est plus généralement utilisée pour extraire des données (une entité) d’une source qui ne fournit pas ces informations en natif. | [Named Entity Recognition cognitive skill](cognitive-search-skill-named-entity-recognition.md) (Compétence cognitive de reconnaissance d’entité nommée)| 
| Traitement des images | Déduit du texte à partir d’une image, par exemple reconnaître un repère, ou extrait du texte d’une image. Parmi les exemples courants, on trouve l’OCR qui permet de copier des caractères d’un fichier de document numérisé (JPEG) ou de reconnaître un nom de rue dans une photographie sur laquelle se trouve un panneau de rue. | [Compétence d’analyse d’image](cognitive-search-skill-image-analysis.md) ou [compétence OCR](cognitive-search-skill-ocr.md)
| Traitement en langage naturel | Traitement de texte pour des insights et des informations sur les entrées de texte. La détection de langue, l’analyse des sentiments et l’extraction de phrases clés sont des compétences qui se trouvent dans le traitement en langage naturel.  | [Key Phrase Extraction cognitive skill](cognitive-search-skill-keyphrases.md) (Compétence cognitive d’extraction de phrases clés), [Language detection cognitive skill](cognitive-search-skill-language-detection.md) (Compétence cognitive de détection de langue), [Sentiment cognitive skill](cognitive-search-skill-sentiment.md) (Compétence cognitive d’analyse des sentiments) |
| Décodage du document | Le processus d’extraction ou de création du contenu textuel à partir de sources non textuelles lors de l’indexation. La reconnaissance optique des caractères (OCR) est un exemple, mais il désigne généralement les fonctionnalités principales de l’indexeur, car l’indexeur extrait le contenu des fichiers d’application. La source de données qui fournit l’emplacement du fichier source et la définition d’indexeur qui fournit des mappages de champs, sont les deux facteurs clés du décodage de document. | Consultez [Indexeurs dans Recherche Azure](search-indexer-overview.md) |
| Modélisation | Fusionner des fragments de texte dans une structure plus importante, ou à l’inverse, diviser des blocs de texte plus volumineux afin d’obtenir une taille plus facile à gérer pour davantage de traitements en aval. | [Shaper cognitive skill](cognitive-search-skill-shaper.md) (Compétence cognitive de modélisation), [Text Merge cognitive skill](cognitive-search-skill-textmerger.md) (Compétence cognitive de fusion de texte), [Text split cognitive skill](cognitive-search-skill-textsplit.md) (Compétence cognitive de fractionnement de texte) |
| Documents enrichis | Une structure interne provisoire, pas directement accessible dans le code. Des documents enrichis sont générés au cours du traitement, mais seules les sorties finales sont conservées dans un index de recherche. Les mappages de champs déterminent quels éléments de données sont ajoutés à l’index. | Consultez [Accès au document enrichi](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Indexeur |  Un analyseur qui extrait les données et métadonnées pouvant faire l’objet d’une recherche d’une source de données externe et renseigne un index en fonction des mappages champ à champ entre l’index et votre source de données pour le décodage du document. Pour les enrichissements de recherche cognitive, l’indexeur appelle un ensemble de compétences et héberge les mappages de dossiers associant la sortie de l’enrichissement pour cibler des champs dans l’index. La définition de l’indexeur contient toutes les instructions et références pour les opérations du pipeline, et le pipeline est appelé lors de l’exécution de l’indexeur. | [Indexeurs](search-indexer-overview.md) |
| source de données  | Un objet utilisé par un indexeur pour se connecter à une source de données externes des types pris en charge sur Azure. | Consultez [Indexeurs dans Recherche Azure](search-indexer-overview.md) |
| Index | Un corpus de recherche conservé dans Recherche Azure, construit à partir d’un schéma d’index qui définit l’utilisation et la structure du champ. | [Index dans Recherche Azure](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Par où commencer ?

**Étape 1 : Créer un service de recherche dans une région qui fournit les API** 

+ USA Centre Sud
+ Europe Ouest

**Étape 2 : Exercice pratique pour maîtriser le flux de travail**

+ [Démarrage rapide (portail)](cognitive-search-quickstart-blob.md)
+ [Didacticiel (requêtes HTTP)](cognitive-search-tutorial-blob.md)
+ [Exemples de compétences personnalisées (C#)](cognitive-search-create-custom-skill-example.md)

**Étape 3 : Passer en revue l’API (REST uniquement)**

Actuellement, seules les API REST sont fournies. Utilisez `api-version=2017-11-11-Preview` pour toutes les requêtes. Utilisez les API suivantes pour créer une solution de recherche cognitive. Seules deux API sont ajoutées ou étendues pour la recherche cognitive. D’autres API ont la même syntaxe que les versions généralement disponibles.

| API REST | Description |
|-----|-------------|
| [Créer une source de données](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Une ressource identifiant une source de données externes fournissant des données sources utilisées pour créer des documents enrichis.  |
| [Créer un ensemble de compétences (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Une ressource coordonnant l’utilisation de [compétences prédéfinies](cognitive-search-predefined-skills.md) et de [compétences cognitives personnalisées](cognitive-search-custom-skill-interface.md) utilisées dans un pipeline d’enrichissement lors de l’indexation. |
| [Création d'index](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Un schéma exprimant un index Recherche Azure. Des champs dans l’index sont mappés à des champs dans les données sources ou à des champs créés lors de la phase d’enrichissement (par exemple, un champ pour les noms de l’organisation créé par la reconnaissance d’entité). |
| [Créer un indexeur (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Une ressource définissant les composants utilisés lors de l’indexation : notamment une source de données, un ensemble de compétences, des associations de champs à partir de structures de données sources et intermédiaires pour cibler l’index, et l’index lui-même. L’exécution de l’index est le déclencheur de l’ingestion des données et de l’enrichissement. La sortie est un corpus de recherche basé sur le schéma d’index, rempli avec les données sources, enrichies via des ensembles de compétences.  |

**Liste de vérification : un flux de travail typique**

1. Créez un sous-ensemble de vos données sources dans un échantillon représentatif. Étant donné que l’indexation prend un certain temps, commencez par un petit ensemble de données représentatif, puis augmentez sa taille de façon incrémentielle à mesure que votre solution grandit.

1. Créez un [objet source de données](https://docs.microsoft.com/rest/api/searchservice/create-data-source) dans Recherche Azure pour fournir une chaîne de connexion pour l’extraction de données.

1. Créez un [ensemble de compétences](https://docs.microsoft.com/rest/api/searchservice/create-skillset) avec les étapes d’enrichissement.

1. Définissez le [schéma d’index](https://docs.microsoft.com/rest/api/searchservice/create-index). La collection *Champs* inclut des champs issus des données sources. Vous devez également écraser les champs supplémentaires pour stocker des valeurs générées pour le contenu créé au cours de l’enrichissement.

1. Définissez [l’indexeur](https://docs.microsoft.com/rest/api/searchservice/create-skillset) faisant référence à la source de données, à l’ensemble de compétences et à l’index.

1. Dans l’indexeur, ajoutez *outputFieldMappings*. Cette section mappe la sortie de l’ensemble de compétences (à l’étape 3) aux champs d’entrées dans le schéma d’index (à l’étape 4).

1. Envoyez la requête *Créer un indexeur* que vous venez de créer (une requête POST avec une définition d’indexeur dans le corps de la requête) pour exprimer l’indexeur dans Recherche Azure. Cette étape correspond à la façon dont vous exécutez l’indexeur, en appelant le pipeline.

1. Exécutez des requêtes pour évaluer les résultats et modifiez le code pour mettre à jour les ensembles de compétences, le schéma ou la configuration de l’indexeur.

1. [Réinitialisez l’indexeur](search-howto-reindex.md) avant de régénérer le pipeline.

Pour plus d’informations sur des problèmes ou des questions spécifiques, consultez [Troubleshooting tips for cognitive search](cognitive-search-concept-troubleshooting.md) (Conseils de dépannage pour la recherche cognitive).

## <a name="next-steps"></a>Étapes suivantes

+ [Documentation resources for cognitive search workloads](cognitive-search-resources-documentation.md) (Ressources de documentation pour les charges de travail de recherche cognitive)
+ [Démarrage rapide : Créer un pipeline de recherche cognitive à l’aide de compétences et d’exemples de données](cognitive-search-quickstart-blob.md)
+ [Tutoriel : Appeler des API de recherche cognitive (version préliminaire)](cognitive-search-tutorial-blob.md)
