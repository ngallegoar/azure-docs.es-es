---
title: Configuración de diagnósticos
titleSuffix: Azure Digital Twins
description: Consulte cómo habilitar el registro con la configuración de diagnóstico.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: d47bb7cc868c5733c6e36290f097fec783764cd3
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "88003587"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Solución de problemas de Azure Digital Twins: Registro de diagnóstico

Azure Digital Twins recopila [métricas](troubleshoot-metrics.md) para la instancia del servicio que proporcionan información sobre el estado de los recursos. Puede usar estas métricas para evaluar el estado general del servicio Azure Digital Twins y los recursos conectados a él. Estas estadísticas orientadas al usuario ayudan a ver lo que está ocurriendo con su instancia de Azure Digital Twins y a realizar el análisis de la causa raíz de los problemas sin necesidad de ponerse en contacto con el soporte técnico de Azure.

En este artículo se muestra cómo activar el **registro de diagnóstico** para los datos de métricas de la instancia de Azure Digital Twins. Estos registros se pueden usar como ayuda para solucionar problemas del servicio y configurar opciones de diagnóstico para enviar métricas de Azure Digital Twins a diferentes destinos. Puede encontrar más información sobre esta configuración en [*Creación de una configuración de diagnóstico para enviar los registros y las métricas de la plataforma a diferentes destinos*](../azure-monitor/platform/diagnostic-settings.md).

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Activación de la configuración de diagnóstico con Azure Portal

Aquí se describe cómo habilitar la configuración de diagnóstico de la instancia de Azure Digital Twins:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la instancia de Azure Digital Twins. Escriba el nombre en la barra de búsqueda para encontrarla. 

2. En el menú, seleccione **Configuración de diagnóstico** y **Agregar configuración de diagnóstico**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Captura de pantalla que muestra la página de configuración de diagnóstico y el botón para agregarla":::

3. En la página siguiente, rellene los valores a continuación:
     * **Diagnostic setting name** (Nombre de la configuración de diagnóstico): Asigne un nombre a la configuración de diagnóstico.
     * **Category details** (Detalles de categoría): Elija qué operaciones desea supervisar y marque las casillas para habilitar el diagnóstico para ellas. Las operaciones sobre las que puede informar la configuración de diagnóstico son:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - QueryOperation
        - AllMetrics
        
        Para más información sobre estas opciones, consulte la sección [*Detalles de categoría*](#category-details) a continuación.
     * **Detalles de destino**: Elija dónde desea enviar los registros. También puede seleccionar cualquier combinación de las tres opciones:
        - Enviar a Log Analytics
        - Archivar en una cuenta de almacenamiento
        - Transmitir a un centro de eventos

        Si es necesario para el destino seleccionado, se le pedirá que rellene detalles adicionales.  
    
4. Guarde la configuración nueva. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Captura de pantalla que muestra la página de configuración de diagnóstico y el botón para agregarla":::

La nueva configuración surte efecto en unos 10 minutos. Después, los registros aparecen en el destino configurado en la página **Configuración de diagnóstico** de la instancia. 

## <a name="category-details"></a>Detalles de categoría

Aquí encontrará más información sobre las categorías de registro que se pueden seleccionar en **Category details** (Detalles de categoría) al configurar los valores de diagnóstico.

| Categoría de registro | Descripción |
| --- | --- |
| ADTModelsOperation | Registra todas las llamadas API relativas a modelos. |
| ADTQueryOperation | Registra todas las llamadas API relativas a consultas. |
| ADTEventRoutesOperation | Registra todas las llamadas API relativas a rutas de eventos, así como la salida de eventos de Azure Digital Twins a un servicio de punto de conexión como Event Grid, Event Hubs y Service Bus. |
| ADTDigitalTwinsOperation | Registra todas las llamadas API relativas a Azure Digital Twins. |

Cada categoría de registro consta de operaciones de escritura, lectura, eliminación y acción.  Estas se asignan a las llamadas API REST de la siguiente manera:

| Tipo de evento | Operaciones de API REST |
| --- | --- |
| Escritura | PUT y PATCH |
| Lectura | GET |
| Eliminar | Delete |
| Acción | POST |

Esta es una lista completa de las operaciones y las [llamadas API REST de Azure Digital Twins](https://docs.microsoft.com/rest/api/azure-digitaltwins/) correspondientes que se registran en cada categoría. 

>[!NOTE]
> Cada categoría de registro contiene varias operaciones y llamadas API REST. En la tabla siguiente, cada categoría de registro se asigna a todas las operaciones y llamadas API REST que hay debajo hasta que se muestra la siguiente categoría de registro. 

| Categoría de registro | Operación | Llamadas API REST y otros eventos |
| --- | --- | --- |
| ADTModelsOperation | Microsoft.DigitalTwins/models/write | Digital Twin Models Update API |
|  | Microsoft.DigitalTwins/models/read | Digital Twin Models Get By Id and List API |
|  | Microsoft.DigitalTwins/models/delete | Digital Twin Models Delete API |
|  | Microsoft.DigitalTwins/models/action | Digital Twin Models Add API |
| ADTQueryOperation | Microsoft.DigitalTwins/query/action | Query Twins API |
| ADTEventRoutesOperation | Microsoft.DigitalTwins/eventroutes/write | Event Routes Add API |
|  | Microsoft.DigitalTwins/eventroutes/read | Event Routes Get By Id and List API |
|  | Microsoft.DigitalTwins/eventroutes/delete | Event Routes Delete API |
|  | Microsoft.DigitalTwins/eventroutes/action | Salida de un evento a un servicio de punto final (no una llamada API) |
| ADTDigitalTwinsOperation | Microsoft.DigitalTwins/digitaltwins/write | Agregar, agregar relaciones, actualizar, actualizar componente de Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/read | Obtener por identificador, obtener componente, obtener relación por identificador, enumerar relaciones entrantes, enumerar relaciones de Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/delete | Eliminar, eliminar relación de Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/action | Enviar telemetría de componentes, enviar telemetría de Digital Twins |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la configuración de diagnóstico, consulte el artículo [*Recopilación y consumo de los datos de registro provenientes de los recursos de Azure*](../azure-monitor/platform/platform-logs-overview.md).
* Para información sobre las métricas de Azure Digital Twins, consulte el artículo [*Solución de problemas: visualización de las métricas con Azure Monitor*](troubleshoot-metrics.md).
* Para ver cómo habilitar las alertas de las métricas, consulte [*Solución de problemas: Configuración de alertas*](troubleshoot-alerts.md).
