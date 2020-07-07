---
title: Notas de la versión de Azure Data Box Gateway y Azure Data Box Edge 1906 | Microsoft Docs
description: Describe los problemas críticos abiertos y las soluciones para Azure Data Box Gateway y Azure Data Box Edge que ejecuta la versión 1906.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 09/18/2019
ms.author: alkohli
ms.openlocfilehash: 9f4633a3fcafcb2b3151270715036e7940eb1c91
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "82561847"
---
# <a name="azure-data-box-edge-and-azure-data-box-gateway-1906-release-notes"></a>Notas de la versión 1906 de Azure Data Box Edge y Azure Data Box Gateway

En las notas de la versión siguientes se identifican los problemas críticos pendientes y los problemas resueltos de la versión 1906 para Azure Data Box Edge y Azure Data Box Gateway. 

Las notas de la versión se actualizan continuamente y se van agregando a medida que se descubren problemas críticos que requieren una solución alternativa. Antes de implementar la instancia de Data Box Edge o Data Box Gateway, revise detenidamente la información que encontrará en las notas de la versión.

Esta versión se corresponde con las versiones de software siguientes:

- **Data Box Gateway 1906 (1.6.978.743)**
- **Data Box Edge 1906 (1.6.978.743)**

> [!NOTE]
> La actualización 1906 solo se puede aplicar a los dispositivos Data Box Edge en los que se ejecute la versión de disponibilidad general o la versión 1905 del software.

## <a name="whats-new"></a>Novedades

- **Corrección de errores en el flujo de trabajo de administración de claves de recuperación**: en la versión anterior, se produjo un error debido a que no se aplicó la clave de recuperación. Este error se ha corregido en esta versión. Se recomienda encarecidamente aplicar esta actualización, ya que la clave de recuperación le permite recuperar los datos en el dispositivo, en caso de que el dispositivo no arranque. Para obtener más información, consulte cómo [guardar la clave de recuperación al implementar Data Box Edge o Data Box Gateway](azure-stack-edge-deploy-connect-setup-activate.md#set-up-and-activate-the-physical-device).
- **Mejoras del registro de la matriz de puertas programables (FPGA)** : desde la versión 1905, se han realizado mejoras en el registro y las alertas relacionadas con FPGA. Sigue siendo una actualización necesaria para Data Box Edge si usa la característica de proceso perimetral con FPGA. Para más información, vea cómo [transformar datos con el proceso perimetral en Data Box Edge](azure-stack-edge-deploy-configure-compute-advanced.md).

## <a name="known-issues-in-ga-release"></a>Problemas conocidos en la versión de disponibilidad general

No se ha notificado ningún problema nuevo para esta versión. Todos los problemas notificados de la versión provienen de versiones anteriores. Para ver una lista de los problemas conocidos, vaya a [Problemas conocidos en la versión de disponibilidad general](data-box-gateway-release-notes.md#known-issues-in-ga-release).


## <a name="next-steps"></a>Pasos siguientes

- [Preparación para implementar Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
- [Preparación de la implementación de Azure Data Box Edge](azure-stack-edge-deploy-prep.md)
