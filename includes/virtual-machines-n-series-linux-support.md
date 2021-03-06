---
title: Fichier Include
description: Fichier Include
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 05/29/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 1813367a2d143f75fb51a3160dd00219c709c57b
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935757"
---
## <a name="supported-distributions-and-drivers"></a>Distributions et pilotes pris en charge

### <a name="nvidia-cuda-drivers"></a>Pilotes CUDA NVIDIA

Les pilotes CUDA NVIDIA pour les machines virtuelles des séries NC, NCv2, NCv3 et ND (facultatif pour la série NV) sont pris en charge uniquement dans les distributions Linux répertoriées dans le tableau suivant. Les informations relatives aux pilotes CUDA sont à jour au moment de la publication. Pour les pilotes CUDA les plus récents, visitez le site web de [NVIDIA](https://developer.nvidia.com/cuda-zone). Assurez-vous que vous installez ou mettez à niveau en utilisant les derniers pilotes CUDA pour votre distribution. 

> [!TIP]
> Vous pouvez déployez une image de [machine virtuelle de science des données](../articles/machine-learning/data-science-virtual-machine/overview.md) Azure, comme alternative à une installation manuelle du pilote CUDA sur une machine virtuelle Linux. Les éditions des machines virtuelles Science des données pour Ubuntu 16.04 LTS ou CentOS 7.4 préinstallent les pilotes NVIDIA CUDA, la bibliothèque de réseau neuronal profond et d’autres outils.

| Distribution | Pilote |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/> HPC basé sur CentOS 7.3 ou 7.4, HPC basé sur CentOS 7.4 | NVIDIA CUDA 9.1, branche pilote R390 |

### <a name="nvidia-grid-drivers"></a>Pilotes NVIDIA GRID

Microsoft redistribue les programmes d’installation du pilote GRID NVIDIA pour les machines virtuelles de série NV utilisées comme stations de travail virtuelles ou pour les applications virtuelles. Installez uniquement ces pilotes GRID sur des machines virtuelles Azure NV, et uniquement sur les distributions répertoriées dans le tableau suivant. Ces pilotes incluent les licences des logiciels GRID Virtual GPU dans Azure.

| Distribution | Pilote |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/>Basé sur CentOS 7.3 ou 7.4 | NVIDIA GRID 6.2, branche pilote R390|



> [!WARNING] 
> L’installation de logiciels tiers sur des produits Red Hat peut affecter les conditions de prise en charge de Red Hat. Consultez l’[article de la Base de connaissances Red Hat](https://access.redhat.com/articles/1067).
>
