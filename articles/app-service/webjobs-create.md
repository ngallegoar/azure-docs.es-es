---
title: Ejecución de tareas en segundo plano con WebJobs
description: Aprenda a usar WebJobs para ejecutar tareas en segundo plano en Azure App Service. Elija entre diversos formatos de script y ejecútelos con expresiones CRON.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ms.reviewer: msangapu;suwatch;pbatum;naren.soni
ms.custom: seodec18
ms.openlocfilehash: 235d82e54c79350f110ab0cda4f4b672e396c61d
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91652013"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Ejecución de tareas en segundo plano con WebJobs en Azure App Service

En este artículo se muestra cómo implementar WebJobs con [Azure Portal](https://portal.azure.com) para cargar un ejecutable o un script. Para información sobre cómo desarrollar e implementar WebJobs mediante Visual Studio, consulte [Implementación de WebJobs con Visual Studio](webjobs-dotnet-deploy-vs.md).

## <a name="overview"></a>Información general
WebJobs es una característica de [Azure App Service](index.yml) que le permite ejecutar un programa o script en la misma instancia que una aplicación web, una aplicación de API o una aplicación móvil. No hay ningún coste adicional en el uso de trabajos web.

> [!IMPORTANT]
> WebJobs todavía no es compatible con App Service en Linux.

El SDK de Azure WebJobs se puede usar con WebJobs para simplificar muchas tareas de programación. Para obtener más información, consulte [¿Qué es el SDK de Webjobs?](https://github.com/Azure/azure-webjobs-sdk/wiki)

Azure Functions proporciona otra manera de ejecutar programas y scripts. Para una comparación entre WebJobs y Functions, consulte [Elección entre Flow, Logic Apps, Functions y WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="webjob-types"></a>Tipos de WebJob

En la tabla siguiente se describen las diferencias entre los WebJobs *continuo* y *desencadenado*.


|Continuo  |Desencadenado  |
|---------|---------|
| Se inicia inmediatamente cuando se crea el WebJob. Para evitar que el trabajo finalice, el programa o script habitualmente funciona dentro de un bucle sin fin. Si el trabajo finaliza, es posible reiniciarlo. | Se inicia solo cuando se desencadena manualmente o de acuerdo con una programación. |
| Se ejecuta en todas las instancias en que se ejecuta la aplicación web. También puede restringir el WebJob a una sola instancia. |Se ejecuta en una sola instancia que Azure selecciona para el equilibrio de carga.|
| Admite la depuración remota. | No admite la depuración remota.|

[!INCLUDE [webjobs-always-on-note](../../includes/webjobs-always-on-note.md)]

## <a name="supported-file-types-for-scripts-or-programs"></a><a name="acceptablefiles"></a>Tipos de archivo admitidos para scripts o programas

Se admiten los tipos de archivo siguientes:

* .cmd, .bat, .exe (con cmd de Windows)
* .ps1 (con PowerShell)
* .sh (con Bash)
* .php (con PHP)
* .py (con Python)
* .js (con Node.js)
* .jar (con Java)

## <a name="create-a-continuous-webjob"></a><a name="CreateContinuous"></a> Creación de un WebJob continuo

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. En [Azure Portal](https://portal.azure.com), vaya a la página **App Service** de la aplicación web, la aplicación de API o la aplicación móvil de App Service.

2. Seleccione **WebJobs**.

   ![Selección de WebJobs](./media/web-sites-create-web-jobs/select-webjobs.png)

2. En la página **WebJobs**, seleccione **Agregar**.

    ![Página WebJob](./media/web-sites-create-web-jobs/wjblade.png)

3. Use la configuración **Agregar WebJob** que se especifica en la tabla.

   ![Captura de pantalla que muestra la configuración de Agregar WebJob que necesita establecer.](./media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Configuración      | Valor de ejemplo   | Descripción  |
   | ------------ | ----------------- | ------------ |
   | **Nombre** | myContinuousWebJob | Nombre único dentro de una aplicación de App Service. Debe comenzar con una letra o un número y no puede contener caracteres especiales salvo "-" y "_". |
   | **Carga de archivos** | ConsoleApp.zip | Archivo *ZIP* que contiene el archivo de script o el archivo ejecutable así como cualquier archivo complementario necesario para ejecutar el programa o script. Los tipos de archivo de script o de archivo ejecutable compatibles aparecen en la sección [Tipos de archivo admitidos](#acceptablefiles). |
   | **Tipo** | Continuo | Los [tipos de WebJob](#webjob-types) aparecen descritos anteriormente en este artículo. |
   | **Escala** | Instancias múltiples | Solo disponible para WebJobs continuos. Determina si el programa o el script se ejecuta en todas las instancias o solo en una. La opción para que se ejecute en varias instancias no se aplica a los [planes de tarifa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Gratis o Compartido. | 

4. Haga clic en **OK**.

   El WebJob nuevo aparece en la página **WebJobs**.

   ![Lista de WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

2. Para detener o reiniciar un WebJob continuo, haga clic con el botón derecho en el WebJob en la lista y haga clic en **Detener** o **Iniciar**.

    ![Detención de un WebJob continuo](./media/web-sites-create-web-jobs/continuousstop.png)

## <a name="create-a-manually-triggered-webjob"></a><a name="CreateOnDemand"></a> Creación de un WebJob desencadenado de manera manual

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. En [Azure Portal](https://portal.azure.com), vaya a la página **App Service** de la aplicación web, la aplicación de API o la aplicación móvil de App Service.

2. Seleccione **WebJobs**.

   ![Selección de WebJobs](./media/web-sites-create-web-jobs/select-webjobs.png)

2. En la página **WebJobs**, seleccione **Agregar**.

    ![Página WebJob](./media/web-sites-create-web-jobs/wjblade.png)

3. Use la configuración **Agregar WebJob** que se especifica en la tabla.

   ![Captura de pantalla que muestra la configuración que debe establecerse para crear un WebJob desencadenado manualmente.](./media/web-sites-create-web-jobs/addwjtriggered.png)

   | Configuración      | Valor de ejemplo   | Descripción  |
   | ------------ | ----------------- | ------------ |
   | **Nombre** | myTriggeredWebJob | Nombre único dentro de una aplicación de App Service. Debe comenzar con una letra o un número y no puede contener caracteres especiales salvo "-" y "_".|
   | **Carga de archivos** | ConsoleApp.zip | Archivo *ZIP* que contiene el archivo de script o el archivo ejecutable así como cualquier archivo complementario necesario para ejecutar el programa o script. Los tipos de archivo de script o de archivo ejecutable compatibles aparecen en la sección [Tipos de archivo admitidos](#acceptablefiles). |
   | **Tipo** | Desencadenado | Los [tipos de WebJob](#webjob-types) aparecen descritos anteriormente en este artículo. |
   | **Desencadenadores** | Manual | |

4. Haga clic en **OK**.

   El WebJob nuevo aparece en la página **WebJobs**.

   ![Lista de WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

7. Para ejecutar el trabajo web, haga clic en su nombre en la lista y haga clic en **Ejecutar**.
   
    ![Ejecución de WebJobs](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="create-a-scheduled-webjob"></a><a name="CreateScheduledCRON"></a> Creación de un WebJob programado

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. En [Azure Portal](https://portal.azure.com), vaya a la página **App Service** de la aplicación web, la aplicación de API o la aplicación móvil de App Service.

2. Seleccione **WebJobs**.

   ![Selección de WebJobs](./media/web-sites-create-web-jobs/select-webjobs.png)

2. En la página **WebJobs**, seleccione **Agregar**.

   ![Página WebJob](./media/web-sites-create-web-jobs/wjblade.png)

3. Use la configuración **Agregar WebJob** que se especifica en la tabla.

   ![Página Agregar WebJob](./media/web-sites-create-web-jobs/addwjscheduled.png)

   | Configuración      | Valor de ejemplo   | Descripción  |
   | ------------ | ----------------- | ------------ |
   | **Nombre** | myScheduledWebJob | Nombre único dentro de una aplicación de App Service. Debe comenzar con una letra o un número y no puede contener caracteres especiales salvo "-" y "_". |
   | **Carga de archivos** | ConsoleApp.zip | Archivo *ZIP* que contiene el archivo de script o el archivo ejecutable así como cualquier archivo complementario necesario para ejecutar el programa o script. Los tipos de archivo de script o de archivo ejecutable compatibles aparecen en la sección [Tipos de archivo admitidos](#acceptablefiles). |
   | **Tipo** | Desencadenado | Los [tipos de WebJob](#webjob-types) aparecen descritos anteriormente en este artículo. |
   | **Desencadenadores** | Programado | Para que la programación funcione de manera confiable, habilite la característica AlwaysOn. AlwaysOn solo está disponible en los planes de tarifa Básico, Estándar y Premium.|
   | **Expresión CRON** | 0 0/20 * * * * | Las [expresiones CRON](#ncrontab-expressions) se describen en la sección siguiente. |

4. Haga clic en **OK**.

   El WebJob nuevo aparece en la página **WebJobs**.

   ![Lista de WebJobs](./media/web-sites-create-web-jobs/listallwebjobs.png)

## <a name="ncrontab-expressions"></a>Expresiones NCRONTAB

Puede especificar una [expresión NCRONTAB](../azure-functions/functions-bindings-timer.md#ncrontab-expressions) en el portal o incluir un archivo `settings.job` en la raíz del archivo *.zip* de WebJob, como en el ejemplo siguiente:

```json
{
    "schedule": "0 */15 * * * *"
}
```

Para más información, consulte [Programar un trabajo Web desencadenado](webjobs-dotnet-deploy-vs.md#scheduling-a-triggered-webjob).

[!INCLUDE [webjobs-cron-timezone-note](../../includes/webjobs-cron-timezone-note.md)]

## <a name="view-the-job-history"></a><a name="ViewJobHistory"></a> Visualización del historial de trabajos

1. Seleccione el WebJob cuyo historial desea ver y, luego, seleccione el botón **Registros**.
   
   ![Botón Registros](./media/web-sites-create-web-jobs/wjbladelogslink.png)

2. En la página **Detalles del WebJob**, seleccione una hora para ver los detalles de una ejecución.
   
   ![Detalles del WebJob](./media/web-sites-create-web-jobs/webjobdetails.png)

3. En la página **Detalles de ejecución del WebJob**, seleccione **Alternar salida** para ver el texto del contenido del registro.
   
    ![Detalles de ejecución del trabajo web](./media/web-sites-create-web-jobs/webjobrundetails.png)

   Para ver el texto de salida en otra ventana del explorador, seleccione **Descargar**. Para descargar el texto propiamente tal, haga clic con el botón derecho en **Descargar** y use las opciones del explorador para guardar el contenido del archivo.
   
5. Seleccione el vínculo de la ruta de navegación de los **WebJobs** que se encuentra en la parte superior de la página para ir a una lista de los WebJobs.

    ![Ruta de navegación de WebJob](./media/web-sites-create-web-jobs/breadcrumb.png)
   
    ![Lista de WebJobs el panel del historial](./media/web-sites-create-web-jobs/webjobslist.png)
   
## <a name="next-steps"></a><a name="NextSteps"></a> Pasos siguientes

El SDK de Azure WebJobs se puede usar con WebJobs para simplificar muchas tareas de programación. Para obtener más información, consulte [¿Qué es el SDK de Webjobs?](https://github.com/Azure/azure-webjobs-sdk/wiki)
