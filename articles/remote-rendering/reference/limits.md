---
title: Limitaciones
description: Limitaciones de código para las características del SDK
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 33f5314c80dc33dbec50dc21a71f4cb507979e12
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427435"
---
# <a name="limitations"></a>Limitaciones

Algunas características tienen limitaciones de tamaño, recuento u otras.

## <a name="azure-frontend"></a>Azure Frontend

Las siguientes limitaciones se aplican a la API de front-end (C++ y C#):
* Número total de instancias de [AzureFrontend](/dotnet/api/microsoft.azure.remoterendering.azurefrontend) por proceso: 16.
* Número total de instancias de [AzureSession](/dotnet/api/microsoft.azure.remoterendering.azuresession) por [AzureFrontend](/dotnet/api/microsoft.azure.remoterendering.azurefrontend): 16.

## <a name="objects"></a>Objetos

* Número total de objetos permitidos de un único tipo ([Entity](../concepts/entities.md), [CutPlaneComponent](../overview/features/cut-planes.md), etc.): 16 777 215.
* Número total de planos de corte activo permitidos: 8.

## <a name="geometry"></a>Geometría

* **Animación:** las animaciones se limitan a animar transformaciones individuales de [objetos de juego](../concepts/entities.md). No se admiten las animaciones esqueléticas con animaciones de piel y vértices. Las pistas de animación del archivo de recursos de origen no se conservan. En su lugar, el código cliente debe controlar las animaciones de transformación de objetos.
* **Sombreadores personalizados:** no se admite la creación de sombreadores personalizados. Solo se pueden usar [materiales de color](../overview/features/color-materials.md) o [materiales PBR](../overview/features/pbr-materials.md) integrados.
* **Número máximo de materiales distintos** en un recurso: 65 535. Para más información sobre la reducción automática del número de materiales, consulte el capítulo sobre la [desduplicación de materiales](../how-tos/conversion/configure-model-conversion.md#material-de-duplication).
* **Dimensión máxima de una sola textura**: 16 384 x 16 384. El proceso de conversión reducirá el tamaño de las texturas de origen más grandes.

### <a name="overall-number-of-polygons"></a>Número total de polígonos

El número de polígonos permitidos para todos los modelos cargados depende del tamaño de la máquina virtual que se pasa a [la API REST de administración de sesión](../how-tos/session-rest-api.md#create-a-session):

| Tamaño del servidor | Número máximo de polígonos |
|:--------|:------------------|
|estándar| 20 millones |
|prémium| ilimitado |

Para más información sobre esta limitación, consulte el capítulo sobre el [tamaño del servidor](../reference/vm-sizes.md).

## <a name="platform-limitations"></a>Limitaciones de la plataforma

**Escritorio de Windows 10**

* Win32/x64 es la única plataforma Win32 compatible. Win32/x86 no es compatible.

**HoloLens 2**

* No se admite la característica [representación desde la cámara PV](/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in).