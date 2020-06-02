---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/09/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3289ba03d0f613d004bc8bff4dbcf2bd434f3da3
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83121589"
---
Functions simplifica el proceso de agregar la integración de Application Insights a una aplicación de función desde [Azure Portal].

1. En [Azure Portal][Azure Portal], busque y seleccione **aplicación de funciones** y, a continuación, elija la aplicación de funciones. 

1. Seleccione el banner **Application Insights no está configurado.** en la parte superior de la ventana. Si no ve el banner, significa que la aplicación ya tiene Application Insights habilitado.

    :::image type="content" source="media/functions-connect-new-app-insights/enable-application-insights.png" alt-text="Habilitación de Application Insights desde el portal":::

1. Cree un recurso de Application Insights mediante la configuración especificada en la tabla que hay debajo de la imagen.

   :::image type="content" source="media/functions-connect-new-app-insights/ai-general.png" alt-text="Creación de recursos en Application Insights":::

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre** | Nombre de aplicación único | Lo más fácil es usar el mismo nombre que usa para la aplicación de función, que debe ser único en su suscripción. | 
    | **Ubicación** | Oeste de Europa | Si es posible, use la misma [región](https://azure.microsoft.com/regions/) que la aplicación de función o una que esté cerca de esa región. |

1. Seleccione **Aplicar**. El recurso de Application Insights se crea en el mismo grupo de recursos y suscripción que su aplicación de función. Después de crear el recurso, cierre la ventana Application Insights.

1. De vuelta en la aplicación de funciones, seleccione **Configuración** > **Configuración** y, a continuación, seleccione **Configuración de la aplicación**. Si ve una configuración denominada `APPINSIGHTS_INSTRUMENTATIONKEY`, significa que la integración de Application Insights está habilitada para la aplicación de función que se ejecuta en Azure.

[Azure Portal]: https://portal.azure.com
