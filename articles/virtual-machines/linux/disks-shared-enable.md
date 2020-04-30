---
title: Habilitación de los discos compartidos para Azure Managed Disks
description: Configuración de un disco administrado de Azure con discos compartidos (versión preliminar) para poder compartirlo en varias VM
author: roygara
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0dc7cf6e3f72ff9fa7ef5d7cbd85c1fb93f76f9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146772"
---
# <a name="enable-shared-disk"></a>Habilitación del disco compartido

En este artículo se describe cómo habilitar la característica de discos compartidos (versión preliminar) para Azure Managed Disks. Los discos compartidos de Azure (versión preliminar) son una nueva característica de Azure Managed Disks que permite adjuntar un disco administrado en varias máquinas virtuales (VM) al mismo tiempo. Si adjunta un disco administrado en varias VM, podrá implementar nuevas aplicaciones en clúster o migrar las existentes a Azure. 

Si busca información conceptual sobre los discos administrados que tienen habilitados discos compartidos, consulte [Discos compartidos de Azure](disks-shared.md).
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]