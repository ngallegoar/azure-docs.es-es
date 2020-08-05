---
title: Configuración de diagnósticos
titleSuffix: Azure Digital Twins
description: Consulte cómo habilitar el registro con la configuración de diagnóstico.
author: baanders
ms.author: baanders
ms.date: 7/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 60ee2102a2b725c68e29afec9a44de03e52e9467
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87379603"
---
# <a name="enable-logging-with-diagnostics-settings"></a>Habilitación del registro con la configuración de diagnóstico

Puede optar por enviar los datos de métricas a [Log Analytics](../azure-monitor/log-query/get-started-portal.md), un punto de conexión de [Event Hubs](../event-hubs/event-hubs-about.md) o [Azure Storage](../storage/blobs/storage-blobs-overview.md), mediante la habilitación del registro con la configuración de diagnóstico para la instancia.

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Activación de la configuración de diagnóstico con Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la instancia de Azure Digital Twins. Escriba el nombre en la barra de búsqueda para encontrarla. 

2. En el menú, seleccione **Configuración de diagnóstico** y **Agregar configuración de diagnóstico**.

    :::image type="content" source="media/how-to-view-metrics/diagnostic-settings.png" alt-text="Captura de pantalla que muestra la página de configuración de diagnóstico y el botón para agregarla":::

3. En la página siguiente, rellene los valores a continuación:
 * **Diagnostic setting name** (Nombre de la configuración de diagnóstico): Asigne un nombre a la configuración de diagnóstico.
 * **Category details** (Detalles de categoría): Elija qué operaciones desea supervisar y marque las casillas para habilitar el diagnóstico para ellas. Las operaciones sobre las que puede informar la configuración de diagnóstico son:
    - DigitalTwinsOperation
    - EventRoutesOperation
    - ModelsOperation
    - QueryOperation
    - AllMetrics
 * **Detalles de destino**: Elija dónde desea enviar los registros. También puede seleccionar cualquier combinación de las tres opciones:
    - Enviar a Log Analytics
    - Archivar en una cuenta de almacenamiento
    - Transmitir a un centro de eventos

    Si es necesario para el destino seleccionado, se le pedirá que rellene detalles adicionales.  
    
4. Guarde la configuración nueva. 

    :::image type="content" source="media/how-to-set-up-diagnostics/diagnostic-settings-details.png" alt-text="Captura de pantalla que muestra la página de configuración de diagnóstico y el botón para agregarla":::

La nueva configuración surte efecto en unos 10 minutos. Después, los registros aparecen en el destino configurado en la página **Configuración de diagnóstico** de la instancia. 

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la configuración de diagnóstico, consulte el artículo de [*Recopilación y consumo de los datos de registro provenientes de los recursos de Azure*](../azure-monitor/platform/platform-logs-overview.md).
* Para información sobre las métricas de Azure Digital Twins, consulte el artículo de [*Procedimiento de Visualización de métricas con Azure Monitor*](how-to-view-metrics.md)