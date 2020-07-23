---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7700b1118c4f04643607f44a474338ffdd5c09e0
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86231022"
---
- No es compatible con discos Ultra.
- No se puede habilitar si Azure Disk Encryption (cifrado de máquina virtual de invitado con BitLocker/VM-Decrypt) está habilitado en las máquinas virtuales o en los conjuntos de escalado de máquinas virtuales.
- No se puede habilitar Azure Disk Encryption en discos que tienen habilitado el cifrado en el host.
- El cifrado se puede habilitar en el conjunto de escalado de máquinas virtuales existente. Sin embargo, solo se cifrarán automáticamente las nuevas máquinas virtuales creadas después de habilitar el cifrado.
- Las máquinas virtuales existentes se deben desasignar y reasignar para su cifrado.