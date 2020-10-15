---
title: Solución de problemas de reinicio inesperado de máquinas virtuales de Azure con discos duros virtuales conectados | Microsoft Docs
description: Solución de problemas de reinicio inesperado de máquinas virtuales.
keywords: conexión ssh rechazada, error de ssh azure ssh, error de conexión ssh
services: virtual-machines
author: genlin
manager: dcscontentpm
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 3a06db1afd130d936af868d0d20632c3ec4fbfd2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75358533"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Solución de problemas de reinicios inesperados de máquinas virtuales con discos duros virtuales asociados

Si una máquina virtual de Azure (VM) tiene una gran cantidad de discos duros virtuales de la misma cuenta de almacenamiento conectados, es posible que supere los objetivos de escalabilidad de una cuenta de almacenamiento individual, lo que provocaría un reinicio inesperado en la máquina virtual. Compruebe las métricas por minuto de la cuenta de almacenamiento (**TotalRequests**/**TotalIngress**/**TotalEgress**) para ver si hay picos que superen los objetivos de escalabilidad de una cuenta de almacenamiento. Consulte la sección [Las métricas muestran un aumento de PercentThrottlingError](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) para recibir asistencia a la hora de averiguar si se produjo alguna limitación en la cuenta de almacenamiento.

En general, cada operación individual de entrada o salida de un VHD desde una máquina virtual se traduce en operaciones **Get Page** o **Put Page** en el blob en páginas subyacente. Por lo tanto, puede usar el IOPS estimado para su entorno con el fin de ajustar la cantidad de VHD que puede tener en una sola cuenta de almacenamiento, en función del comportamiento específico de la aplicación. Microsoft recomienda tener 40 o menos discos en una cuenta de almacenamiento única. Para obtener más información sobre los objetivos de escalabilidad de cuentas de almacenamiento estándar, consulte [Objetivos de escalabilidad para cuentas de almacenamiento estándar](../../storage/common/scalability-targets-standard-account.md). Para obtener más información sobre los objetivos de escalabilidad de las cuentas de almacenamiento de blob en páginas prémium, consulte [Objetivos de escalabilidad de las cuentas de almacenamiento de blob en páginas prémium](../../storage/blobs/scalability-targets-premium-page-blobs.md).

Si supera los objetivos de escalabilidad para la cuenta de almacenamiento, coloque los VHD en varias cuentas de almacenamiento para reducir la actividad de cada cuenta individual.
