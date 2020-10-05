---
title: Limitaciones
description: Limitaciones de código para las características del SDK
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: e6b12c2bac4a9732f868f6a6ac3491ef993f54c3
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90976560"
---
# <a name="limitations"></a>Limitaciones

Algunas características tienen limitaciones de tamaño, recuento u otras.

## <a name="azure-frontend"></a>Azure Frontend

* Número total de instancias de AzureFrontend por proceso: 16.
* Número total de instancias de AzureSession por AzureFrontend: 16.

## <a name="objects"></a>Objetos

* Número total de objetos permitidos de un único tipo (Entity, CutPlaneComponent, etc.): 16 777 215.
* Número total de planos de corte activo permitidos: 8.

## <a name="geometry"></a>Geometría

* Número total de materiales permitidos en un recurso: 65 535. Consulte [Desduplicación de materiales](../how-tos/conversion/configure-model-conversion.md#material-de-duplication) para obtener más información.
* Dimensión máxima de una sola textura: 16 384 x 16 384. El proceso de conversión reducirá la escala de las texturas de origen más grandes.

## <a name="overall-number-of-polygons"></a>Número total de polígonos

El número de polígonos permitidos para todos los modelos cargados depende del tamaño de la máquina virtual que se pasa a [la API REST de administración de sesión](../how-tos/session-rest-api.md#create-a-session):

| Tamaño del servidor | Número máximo de polígonos |
|:--------|:------------------|
|estándar| 20 millones |
|prémium| ilimitado |

Vea el capítulo [Tamaño del servidor](../reference/vm-sizes.md) para obtener información más detallada sobre esta limitación.

## <a name="platform-limitations"></a>Limitaciones de la plataforma

**Escritorio de Windows 10**

* Win32/x64 es la única plataforma Win32 compatible. Win32/x86 no es compatible.

**HoloLens 2**

* No se admite la característica [representación desde la cámara PV](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in).
