---
title: Présentation d'Azure Data Science Virtual Machine pour Linux et Windows | Microsoft Docs
description: Scénarios et composants d’analytique clés pour Azure Data Science Virtual Machine Windows et Linux.
keywords: outils de science des données, machine virtuelle science des données, outils pour la science des données, science des données linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 10/27/2017
ms.author: gokuma
ms.openlocfilehash: 661c757cda2f70b03d136f3726c7afe633fbbe04
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42022920"
---
# <a name="introduction-to-azure-data-science-virtual-machine-for-linux-and-windows"></a>Présentation d'Azure Data Science Virtual Machine pour Linux et Windows

La machine virtuelle Science des données (DSVM) est une image de machine virtuelle personnalisée sur le cloud Microsoft Azure spécialement conçue pour la science des données. Elle inclut de nombreux outils de science des données populaires et d’autres outils sont préinstallés et préconfigurés afin d’accélérer la création d’applications intelligentes à des fins d’analyse avancée. Elle est disponible sur Windows Server et sur Linux. Nous vous proposons l’édition Windows de la DSVM sous Server 2016 et Server 2012. Nous proposons des éditions de Linux de la DSVM sur Ubuntu 16.04 LTS et CentOS 7.4.

Cette rubrique explique ce que vous pouvez faire avec la machine virtuelle Science des données, présente les principaux scénarios d’utilisation de la machine Virtuelle, détaille les principales fonctionnalités disponibles dans les versions de Windows et Linux, et fournit des instructions sur la façon de commencer à les utiliser.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Que puis-je faire avec la machine virtuelle Science des données ?
L’objectif de la machine virtuelle Science des données (DSVM) est de fournir aux experts en données, tous niveaux de compétence et rôles confondus, un environnement de science des données préconfiguré et totalement intégré. Au lieu de déployer votre propre espace de travail, vous pouvez approvisionner un DSVM, ce qui vous permet d’économiser des jours, voire des _semaines_, sur les processus d’installation, de configuration et de gestion des packages. Une fois votre DSVM alloué, vous pouvez commencer immédiatement à travailler sur votre projet de science des données.

DSVM (Data Science Virtual Machine) a été conçu et configuré pour fonctionner dans une grande variété de scénarios. Vous pouvez faire évoluer ou réduire votre environnement en fonction des besoins de votre projet. Vous pouvez utiliser votre langage par défaut pour programmer les tâches de science des données. Vous pouvez installer d’autres outils et personnaliser le système en fonction de vos besoins exacts.

## <a name="key-scenarios"></a>Principaux scénarios
Cette section propose des scénarios principaux dans lesquels la machine virtuelle Science des données peut être déployée.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Bureau d’analyse préconfiguré dans le cloud
La machine virtuelle Science des données fournit une configuration de base pour les équipes de science des données cherchant à remplacer leurs postes de travail locaux par un bureau de cloud géré. Cette base garantit que tous les scientifiques de données de l’équipe disposent d’une configuration cohérente permettant de vérifier les expériences et encourageant la collaboration. Cela réduit également les coûts en réduisant les efforts pour l’administrateur système et en permettant de réduire le temps nécessaire à l’évaluation, à l’installation et la à gestion des divers packages logiciels nécessaires à l’analyse avancée.  

### <a name="data-science-training-and-education"></a>Formation et éducation de la science des données
Les formateurs d’entreprise et les instructeurs qui enseignent la science des données fournissent généralement une image de machine virtuelle pour s’assurer que leurs étudiants disposent d’une installation cohérente et que les exemples fonctionnent comme prévu. La machine virtuelle Science des données crée un environnement à la demande avec une configuration cohérente qui simplifie le support et les défis liés à la compatibilité. Ceci est d’autant plus bénéfique dans les cas où ces environnements doivent être créés fréquemment, en particulier lors de formations plus courtes.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Flexibilité à la demande pour les projets d’envergure
Les concours de science des données ou la modélisation et l’exploration de données à grande échelle nécessitent une augmentation de la capacité matérielle, généralement sur une courte durée. La machine virtuelle Science des données permet de répliquer rapidement et à la demande l’environnement de science des données, sur des serveurs montés offrant ainsi des expériences d’exécution des ressources informatique hautes performances.

### <a name="short-term-experimentation-and-evaluation"></a>Expérimentation et évaluation à court terme
La machine virtuelle Science des données peut être utilisée pour évaluer ou découvrir des outils tels que Microsoft ML Server, SQL Server, Visual Studio, Jupyter, kits de ressources de formation approfondie / ML et de nouveaux outils populaires dans la communauté, avec un minimum d’efforts d’installation. Étant donné que la machine virtuelle Science des données peut être configurée rapidement, elle peut être appliquée dans d’autres scénarios d’utilisation à court terme comme la réplication d’expériences publiées, l’exécution de démonstrations, le suivi de procédures pas-à-pas dans des sessions en ligne ou des didacticiels de conférence.

### <a name="deep-learning"></a>Apprentissage approfondi
La machine virtuelle de Science des données peut être utilisée pour le modèle d’apprentissage, grâce à des algorithmes d’apprentissage approfondis sur le matériel basé sur les processeurs graphiques (GPU). En utilisant les fonctionnalités de mise à l’échelle de machine virtuelle du cloud Azure, la machine virtuelle DSVM vous permet d’utiliser du matériel basé sur GPU dans le cloud, selon vos besoins. Vous pouvez basculer vers une machine virtuelle basée sur GPU lorsque vous formez de grands modèles ou lorsque vous avez besoin d’une grande rapidité de calcul, mais souhaitez garder le même disque de système d’exploitation.  L’édition Windows Server 2016 de la DSVM est préinstallée avec les pilotes GPU, les infrastructures et les versions GPU de frameworks de deep learning. Sur Linux, le deep learning sur GPU est activé sur les DSVM CentOS et Ubuntu. Vous pouvez déployer l’édition DSVM Ubuntu, CentOS ou Windows 2016 sur une machine virtuelle Azure non basée sur GPU. Dans ce cas, tous les frameworks de deep learning passent en mode UC. 

## <a name="whats-included-in-the-data-science-vm"></a>Qu’est-ce qui est inclus dans la machine virtuelle Science des données ?
De nombreux outils de science des données et d’apprentissage approfondi populaires sont déjà installés et configurés sur la machine virtuelle Science des données. Elle inclut également des outils simplifiant l’utilisation de différents produits de données et d’analyse Azure. Vous pouvez explorer et créer des modèles prédictifs sur des jeux de données d’envergure à l’aide de Microsoft ML Server (R, Python) ou de SQL Server 2017. D’autres outils encore de la communauté open source et de Microsoft sont également inclus, ainsi qu’un exemple de code et des notebooks. Le tableau suivant détaille et compare les principaux composants inclus dans les éditions Windows et Linux de la machine virtuelle Science des données.


| **Outil**                                                           | **Édition Windows** | **Édition Linux** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) avec packages courants préinstallés   |O                      | O             |
| [Microsoft ML Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Developer Edition inclut, <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) infrastructure hautes performances parallèle et distribuée (R & Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp;*   [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package) – nouveaux algorithmes ML de pointe de Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp;* [Opérationnalisation R et Python](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |O                      | O |
| [Microsoft Office](https://products.office.com/en-us/business/office-365-proplus-business-software) Pro-Plus avec activation partagée : Excel, Word et PowerPoint   |O                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7, 3.5 avec packages populaires préinstallés    |O                      |O              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) avec packages populaires préinstallés                         |O                      |O              |
| Bases de données relationnelles                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Outils de base de données                                                       | * SQL Server Management Studio <br/>* SQL Server Integration Services<br/>* [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br /> * Pilotes ODBC/JDBC| * [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (outil de requête), <br /> * bcp, sqlcmd <br /> * Pilotes ODBC/JDBC|
| Analytiques en base de données évolutives avec services SQL Server ML (R, Python) | O     |N              |
| **[Serveur Bloc-notes Jupyter](http://jupyter.org/) avec noyaux suivants,**                                  | O     | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* R | O | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Python | O | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* Julia | O | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;* PySpark | O | O |
|     &nbsp;&nbsp;&nbsp;&nbsp;*   [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | O (Ubuntu uniquement) |
|     &nbsp;&nbsp;&nbsp;&nbsp;* SparkR     | N | O |
| JupyterHub (serveur de bloc-notes multi-utilisateur)| N | O |
| JupyterLab (serveur de bloc-notes multi-utilisateur) | N | O (Ubuntu uniquement) |
| **Outils de développement, éditeurs de code et EDI**| | |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Visual Studio 2017 (Community Edition)](https://www.visualstudio.com/community/) avec plug-in Git, Azure HDInsight (Hadoop), Data Lake, SQL Server Data Tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](http://aka.ms/ptvs) et [R Tools for Visual Studio (RTVS)](http://microsoft.github.io/RTVS-docs/) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Visual Studio Code](https://code.visualstudio.com/) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Atom](https://atom.io/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Juno (Julia IDE)](http://junolab.org/)| O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* Vim et Emacs | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* Git et GitBash | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* OpenJDK | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* .Net Framework | O | N |
| PowerBI Desktop | O | N |
| Kits de développement logiciel (SDK) pour accéder à Azure et à la suite de services Cortana Intelligence | O | O |
| **Outils de gestion et de déplacement de données** | | |
| &nbsp;&nbsp;&nbsp;&nbsp;* Explorateur de stockage Microsoft Azure | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Interface de ligne de commande Azure](https://docs.microsoft.com/cli/azure) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* Azure Powershell | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Pilote Blob FUSE](https://github.com/Azure/azure-storage-fuse) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Outil de migration de données DocDB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Passerelle de gestion des données Microsoft](https://msdn.microsoft.com/library/dn879362.aspx) : déplace les données entre OnPrem et le cloud | O | N |
| &nbsp;&nbsp;&nbsp;&nbsp;* Utilitaires en ligne de commande Unix/Linux | O | O |
| [Apache Drill](http://drill.apache.org) pour l’exploration des données | O | O |
| **Outils Machine Learning** |||
| &nbsp;&nbsp;&nbsp;&nbsp;* Intégration avec [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Xgboost](https://github.com/dmlc/xgboost) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Weka](http://www.cs.waikato.ac.nz/ml/weka/) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Rattle](https://togaware.com/rattle/) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [LightGBM](https://github.com/Microsoft/LightGBM) | N | O (Ubuntu uniquement) |
| &nbsp;&nbsp;&nbsp;&nbsp;* [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/), [Deep Water](https://www.h2o.ai/deep-water/) | N | O (Ubuntu uniquement) |
| **Outils d’apprentissage profond** <br>Tous les outils fonctionnent sur un GPU ou un processeur |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Microsoft Cognitive Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) (Windows 2016) | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow](https://www.tensorflow.org/) | O (Windows 2016) | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Horovod](https://github.com/uber/horovod) | N | O (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [MXNet](http://mxnet.io/) | O (Windows 2016) | O|
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Caffe et Caffe2](https://github.com/caffe2/caffe2) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Chainer](https://chainer.org/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Torch](http://torch.ch/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Theano](https://github.com/Theano/Theano) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [Keras](https://keras.io/)| N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [PyTorch](http://pytorch.org/)| N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;*   [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorFlow Serving](https://www.tensorflow.org/serving/) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [TensorRT](https://developer.nvidia.com/tensorrt) | N | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [CUDA, cuDNN, pilote NVIDIA](https://developer.nvidia.com/cuda-toolkit) | O | O |
| **Plateforme Big Data (Devtest uniquement)**|||
| &nbsp;&nbsp;&nbsp;&nbsp;* [Spark](http://spark.apache.org/) autonome localité | O | O |
| &nbsp;&nbsp;&nbsp;&nbsp;* [Hadoop](http://hadoop.apache.org/) local (HDFS, YARN) | N | O |

## <a name="get-started"></a>Prise en main

### <a name="windows-data-science-vm"></a>Machine virtuelle de science des données Windows
* Pour plus d’informations sur la création d’une DSVM Windows et son utilisation, consultez [Approvisionner la machine virtuelle de science des données Windows](provision-vm.md). Pour plus d’informations sur l’exécution de diverses tâches nécessaires à votre projet de science des données sur la DSVM Windows, consultez [Dix choses que vous pouvez effectuer sur la machine virtuelle de science des données](vm-do-ten-things.md).

### <a name="linux-data-science-vm"></a>Machine virtuelle de science des données Linux
* Pour plus d’informations sur la création d’une DSVM Ubuntu et son utilisation, consultez [Approvisionner la machine virtuelle de science des données pour Linux (Ubuntu)](dsvm-ubuntu-intro.md). Pour plus d’informations sur la création d’une DSVM CentOS et son utilisation, consultez [Approvisionner la machine virtuelle de science des données CentOS sur Azure](linux-dsvm-intro.md).
* Pour obtenir une procédure montrant comment effectuer plusieurs tâches courantes relatives à la science des données avec la machine virtuelle Linux (CentOS et Ubuntu), consultez [Science des données sur la machine virtuelle de science des données Linux](linux-dsvm-walkthrough.md).

