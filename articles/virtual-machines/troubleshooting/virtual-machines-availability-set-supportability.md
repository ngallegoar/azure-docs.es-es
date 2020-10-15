---
title: Compatibilidad a la hora de agregar máquinas virtuales de Azure a un conjunto de disponibilidad existente | Microsoft Docs
description: En este artículo se proporciona una matriz de compatibilidad sobre la serie de máquinas virtuales que se pueden combinar en el mismo conjunto de disponibilidad.
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: a9ca8f219bef787de04b51600209bfd3a24dd166
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77122664"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Compatibilidad a la hora de agregar máquinas virtuales de Azure a un conjunto de disponibilidad existente

En ocasiones, puede encontrar limitaciones al agregar nuevas máquinas virtuales (VM) a un conjunto de disponibilidad existente. El siguiente gráfico detalla las series de máquinas virtuales que puede mezclar en el mismo conjunto de disponibilidad.

Esta es la matriz de compatibilidad para mezclar distintos tipos de máquinas virtuales:

Serie y conjunto de disponibilidad|Segunda máquina virtual|Un|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|Primera máquina virtual|||||||
|Un||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|Av2||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|D||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|Dv2||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|
|Dv3||Aceptar|Aceptar|Aceptar|Aceptar|Aceptar|

Todas las otras series no pueden estar en el mismo conjunto de disponibilidad porque requieren un hardware específico.

El tamaño de una máquina virtual A8/A9 no se puede combinar debido al requisito sobre la red de backend de RDMA dedicada.
