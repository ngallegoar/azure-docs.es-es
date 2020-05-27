---
title: Limitaciones
description: Limitaciones de código para las características del SDK
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 6a1a51ee09422607ae1392704add4d49d3367d57
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83759054"
---
# <a name="limitations"></a>Limitaciones

Algunas características tienen limitaciones de tamaño, recuento u otras.

## <a name="azure-frontend"></a>Azure Frontend

* Número total de instancias de AzureFrontend por proceso: 16.
* Número total de instancias de AzureSession por AzureFrontend: 16.

## <a name="objects"></a>Objetos

* Número total de objetos permitidos de un único tipo (Entity, CutPlaneComponent, etc.): 16 777 215.
* Número total de planos de corte activo permitidos: 8.

## <a name="materials"></a>Materiales

* Número total de materiales permitidos en un recurso: 65 535.

## <a name="overall-number-of-polygons"></a>Número total de polígonos

El número de polígonos permitidos para todos los modelos cargados depende del tamaño de la máquina virtual que se pasa a [la API REST de administración de sesión](../how-tos/session-rest-api.md#create-a-session):

| Tamaño de VM | Número máximo de polígonos |
|:--------|:------------------|
|estándar| 20 millones |
|prémium| ilimitado |


## <a name="platform-limitations"></a>Limitaciones de la plataforma

**Escritorio de Windows 10**

* UWP/x86 es la única plataforma UWP compatible. UWP/x64 no es compatible.

**HoloLens 2**

* No se admite la característica [representación desde la cámara PV](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in).
