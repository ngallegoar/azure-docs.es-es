---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/26/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d06bda1826964b019edb156375885c7f389ca6ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186964"
---
Utilice el siguiente procedimiento para ver y actualizar la versión del runtime que utiliza una aplicación de función.

1. En [Azure Portal](https://portal.azure.com), vaya a la aplicación de función.

1. En **Características configuradas**, elija **Configuración de la aplicación de función**.

    ![Seleccionar Configuración de Function App](./media/functions-view-update-version-portal/add-update-app-setting.png)

1. En la pestaña **Configuración de Function App**, busque la **versión del runtime**. Observe la versión específica del runtime y la versión principal solicitada. En el ejemplo siguiente, la versión se establece en `~2`.

   ![Seleccionar Configuración de Function App](./media/functions-view-update-version-portal/function-app-view-version.png)

1. Para anclar la aplicación de función a la versión 1.x del entorno de ejecución, elija **~1** en **Versión en tiempo de ejecución**. Este modificador está deshabilitado cuando tiene funciones en la aplicación.

1. Cuando cambie la versión del runtime, vuelva a la pestaña **Información general** y elija **Reiniciar** para reiniciar la aplicación.  La aplicación de función se reinicia y se ejecuta en la versión 1.x del entorno de ejecución y se usan las plantillas de la versión 1.x al crear las funciones.