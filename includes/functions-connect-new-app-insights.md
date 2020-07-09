---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/10/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 87af50c5b5e5b69fd175ac4a570c4b6f659b97e6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84731049"
---
Azure Functions simplifica el proceso de agregar la integración de Application Insights a una aplicación de función desde [Azure Portal].

1. En [Azure Portal][Azure Portal], busque y seleccione **aplicación de funciones** y, a continuación, elija la aplicación de funciones. 

1. Seleccione el banner **Application Insights no está configurado.** en la parte superior de la ventana. Si no ve el banner, es posible que la aplicación ya tenga Application Insights habilitado.

    :::image type="content" source="media/functions-connect-new-app-insights/enable-application-insights.png" alt-text="Habilitación de Application Insights desde el portal":::

1. Expanda **Cambie su recurso** y cree un recurso de Application Insights, para lo que deberá usar los valores que se especifican en la siguiente tabla.  

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nuevo nombre de recurso** | Nombre de aplicación único | Lo más fácil es usar el mismo nombre que usa para la aplicación de función, que debe ser único en su suscripción. | 
    | **Ubicación** | Oeste de Europa | Si es posible, use la misma [región](https://azure.microsoft.com/regions/) que la aplicación de función o una que esté cerca de esa región. |

    :::image type="content" source="media/functions-connect-new-app-insights/ai-general.png" alt-text="Creación de recursos en Application Insights":::

1. Seleccione **Aplicar**. 

   El recurso de Application Insights se crea en el mismo grupo de recursos y suscripción que su aplicación de función. Después de crear el recurso, cierre la ventana Application Insights.

1. En la aplicación de funciones, seleccione **Configuración** en **Configuración** y, después, seleccione **Configuración de la aplicación**. Si ve una configuración denominada `APPINSIGHTS_INSTRUMENTATIONKEY`, significa que la integración de Application Insights está habilitada para la aplicación de función que se ejecuta en Azure.

[Azure Portal]: https://portal.azure.com
