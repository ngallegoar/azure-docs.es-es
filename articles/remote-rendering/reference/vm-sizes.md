---
title: Tamaños de VM
description: Se describen los distintos tamaños de máquina virtual que se pueden asignar.
author: florianborn71
ms.author: flborn
ms.date: 05/28/2020
ms.topic: reference
ms.openlocfilehash: aab914caa2647146639aa366f558c80bebcfde54
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84809942"
---
# <a name="vm-sizes"></a>Tamaños de VM

El servicio de representación puede operar en dos tipos de máquinas diferentes en Azure, denominadas `Standard` y `Premium`.

## <a name="polygon-limits"></a>Límites de polígonos

Hay una limitación estricta de **20 millones de polígonos** para el tamaño de VM `Standard`. Para el tamaño `Premium` no existe tal limitación.

Cuando el representador alcanza este límite en una VM estándar, cambia la representación a un fondo de tablero de ajedrez:

![Tablero de ajedrez](media/checkerboard.png)

## <a name="allocate-the-vm"></a>Asignación de la VM

Debe especificarse el tipo de VM deseado en el momento de la inicialización de la sesión de representación. No se puede cambiar en una sesión en ejecución. Los ejemplos de código siguientes muestran el lugar en el que debe especificarse el tamaño de la VM:

```cs
async void CreateRenderingSession(AzureFrontend frontend)
{
    RenderingSessionCreationParams sessionCreationParams = new RenderingSessionCreationParams();
    sessionCreationParams.Size = RenderingSessionVmSize.Standard; // or  RenderingSessionVmSize.Premium

    AzureSession session = await frontend.CreateNewRenderingSessionAsync(sessionCreationParams).AsTask();
}
```

```cpp
void CreateRenderingSession(ApiHandle<AzureFrontend> frontend)
{
    RenderingSessionCreationParams sessionCreationParams;
    sessionCreationParams.Size = RenderingSessionVmSize::Standard; // or  RenderingSessionVmSize::Premium

    if (auto createSessionAsync = frontend->CreateNewRenderingSessionAsync(sessionCreationParams))
    {
        // ...
    }
}
```

Para los [scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md), el tamaño de la VM debe especificarse dentro del archivo `arrconfig.json`:

```json
{
  "accountSettings": {
    ...
  },
  "renderingSessionSettings": {
    "vmSize": "<standard or premium>",
    ...
  },
```

### <a name="how-the-renderer-evaluates-the-number-of-polygons"></a>Procedimientos para que el representador evalúe el número de polígonos

El número de polígonos que se considera para la prueba de limitación es el número de polígonos que se pasa al representador. Normalmente, esta geometría es la suma de todos los modelos de los que se han creado instancias, aunque también hay excepciones. La siguiente geometría **no se incluye**:
* Instancias de modelos cargados que están totalmente fuera del tronco de la vista.
* Modelos o partes de modelos que se cambian a invisible mediante el [componente de invalidación del estado jerárquico](../overview/features/override-hierarchical-state.md).

En consecuencia, es posible escribir una aplicación que se destina al tamaño `standard` que carga varios modelos con un número de polígonos próximo al límite de cada modelo. Cuando la aplicación muestra únicamente un único modelo cada vez, el tablero de ajedrez no se desencadena.

### <a name="how-to-determine-the-number-of-polygons"></a>Procedimientos para determinar el número de polígonos

Hay dos maneras de determinar el número de polígonos de un modelo o una escena que contribuyen al límite del presupuesto de la VM de tamaño `standard`:
* En la conversión del modelo, recupere el [archivo JSON de salida de la conversión](../how-tos/conversion/get-information.md) y compruebe la entrada `numFaces` en la [sección *inputStatistics*](../how-tos/conversion/get-information.md#the-inputstatistics-section).
* Si la aplicación está tratando con contenido dinámico, el número de polígonos representados se puede consultar dinámicamente durante el tiempo de ejecución. Use una [consulta de evaluación de rendimiento](../overview/features/performance-queries.md#performance-assessment-queries) y busque el miembro `polygonsRendered` en la estructura `FrameStatistics`. El fondo de tablero de ajedrez siempre se atenúa con algún retraso para garantizar que se pueda realizar la acción del usuario después de esta consulta asincrónica. La acción del usuario puede consistir, por ejemplo, en ocultar o eliminar instancias de modelo.

## <a name="pricing"></a>Precios

Para obtener un desglose detallado de los precios de cada tipo de VM, consulte la página [Precios de Remote Rendering](https://azure.microsoft.com/pricing/details/remote-rendering).

## <a name="next-steps"></a>Pasos siguientes
* [Scripts de PowerShell de ejemplo](../samples/powershell-example-scripts.md)
* [Conversión de modelos](../how-tos/conversion/model-conversion.md)

