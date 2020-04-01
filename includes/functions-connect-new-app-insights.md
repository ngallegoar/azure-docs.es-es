---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 266cd52117f36b282fdd4bc8615a15e451cc203f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132563"
---
Functions simplifica el proceso de agregar la integración de Application Insights a una aplicación de función desde [Azure Portal].

1. En el [portal][Azure portal], escriba `Function Apps` en la barra de búsqueda de la parte superior de la página, elija la aplicación de funciones y seleccione el banner **Application Insights no está configurado** en la parte superior de la ventana. Si no ve el banner, significa que la aplicación ya tiene Application Insights habilitado.

    ![Habilitación de Application Insights desde el portal](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Cree un recurso de Application Insights mediante la configuración especificada en la tabla que hay debajo de la imagen.

   ![Creación de recursos en Application Insights](media/functions-connect-new-app-insights/ai-general.png)

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre** | Nombre de aplicación único | Lo más fácil es usar el mismo nombre que usa para la aplicación de función, que debe ser único en su suscripción. | 
    | **Ubicación** | Oeste de Europa | Si es posible, use la misma [región](https://azure.microsoft.com/regions/) que la aplicación de función o una que esté cerca de esa región. |

1. Seleccione **Aceptar**. El recurso de Application Insights se crea en el mismo grupo de recursos y suscripción que su aplicación de función. Después de crear el recurso, cierre la ventana Application Insights.

1. De vuelta en la aplicación de función, seleccione **Configuración de la aplicación**y desplácese hacia abajo hasta **Configuración de la aplicación**. Si ve una configuración denominada `APPINSIGHTS_INSTRUMENTATIONKEY`, significa que la integración de Application Insights está habilitada para la aplicación de función que se ejecuta en Azure.

[Azure Portal]: https://portal.azure.com
