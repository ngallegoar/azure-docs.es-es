---
title: Recopilación de registros de recurso de un recurso de Azure y análisis con Azure Monitor
description: Tutorial para configurar los valores de diagnóstico para recopilar registros de recurso de un recurso de Azure en un área de trabajo de Log Analytics donde se puedan analizar con una consulta de registro.
ms.subservice: ''
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 12/15/2019
ms.openlocfilehash: d356042d65c419163de4951e64a635a22ea90e6d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78269187"
---
# <a name="tutorial-collect-and-analyze-resource-logs-from-an-azure-resource"></a>Tutorial: Recopilación y análisis de registros de recurso desde un recurso de Azure

Los registros de recurso proporcionan información minuciosa sobre los detalles de funcionamiento de un recurso de Azure y son útiles para supervisar su estado y disponibilidad. Los recursos de Azure generan registros de recurso automáticamente, pero debe configurar el lugar donde recopilarlos. Este tutorial le guiará por el proceso de creación de una configuración de diagnóstico para recopilar registros de recurso de un recurso en la suscripción de Azure y de análisis con una consulta de registro.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un área de trabajo de Log Analytics en Azure Monitor
> * Crear una configuración de diagnóstico para recopilar registros de recurso 
> * Crear una consulta de registro simple para analizar registros


## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesitará un recurso de Azure para supervisarlo. Puede usar cualquier recurso de la suscripción de Azure que admita la configuración de diagnóstico. Para determinar si un recurso es compatible con la configuración de diagnóstico, vaya a su menú en Azure Portal y busque la opción **Configuración de diagnóstico** en la sección **Supervisión** del menú.


## <a name="log-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).


## <a name="create-a-workspace"></a>Crear un área de trabajo
Un área de trabajo de Log Analytics en Azure Monitor recopila e indexa los datos de registro de una variedad de orígenes y permite el análisis avanzado con un lenguaje de consulta eficaz. El área de trabajo Log Analytics debe existir antes de la configuración de diagnóstico para enviar datos a ella. Puede usar un área de trabajo existente en la suscripción de Azure o crear una con el procedimiento siguiente. 

> [!NOTE]
> Aunque puede trabajar con datos en el área de trabajo de Log Analytics en el menú de **Azure Monitor**, las áreas de trabajo se crean y se administran en el menú **Áreas de trabajo de Log Analytics**.

1. En **Todos los servicios**, seleccione **Áreas de trabajo de Log Analytics**.
2. Haga clic en **Agregar** en la parte superior de la pantalla y proporcione los detalles siguientes para el área de trabajo:
   - **Área de trabajo de Log Analytics**: nombre para la nueva área de trabajo. Este nombre debe ser único globalmente en todas las suscripciones de Azure Monitor.
   - **Suscripción**: donde almacenar el área de trabajo. No es necesario que sea la misma suscripción que la del recurso bajo supervisión.
   - **Grupo de recursos**: seleccione uno existente o haga clic en **Crear** para crear un grupo de recursos nuevo. No es necesario que sea el mismo grupo de recursos que el del recurso bajo supervisión.
   - **Ubicación**: seleccione una región de Azure o cree una. No es necesario que sea la misma ubicación que la del recurso bajo supervisión.
   - **Plan de tarifa**: Seleccione *Pago por uso* como plan de tarifa. Puede cambiar el plan de tarifa más adelante. Haga clic en el vínculo **Precios de Log Analytics** para más información sobre los distintos planes de tarifa.

    ![Nueva área de trabajo](media/tutorial-resource-logs/new-workspace.png)

3. Haga clic en **Aceptar** para crear el área de trabajo.

## <a name="create-a-diagnostic-setting"></a>Creación de una configuración de diagnóstico
La [configuración de diagnóstico](../platform/diagnostic-settings.md) define dónde se deben enviar los registros de recurso y de un recurso determinado. Un solo valor de configuración de diagnóstico puede tener varios [destinos](../platform/diagnostic-settings.md#destinations), pero en este tutorial solo usaremos un área de trabajo de Log Analytics.

1. En la sección **Supervisión** del menú del recursos, seleccione **Configuración de diagnóstico**.
2. Debería recibir el mensaje "No se ha definido ninguna configuración de diagnóstico". Haga clic en **Agregar configuración de diagnóstico**.

    ![Configuración de diagnóstico](media/tutorial-resource-logs/diagnostic-settings.png)

3. Cada configuración de diagnóstico tiene tres partes básicas:
 
   - **Name**: no afecta en nada y debe ser simplemente descriptivo.
   - **Destinos**: Uno o más destinos para enviar los registros. Todos los servicios de Azure comparten el mismo conjunto de tres destinos posibles. Cada configuración de diagnóstico puede definir uno o más destinos, pero no más de uno de un tipo determinado. 
   - **Categorías**: categorías de registros que se van a enviar a cada uno de los destinos. Este conjunto de categorías variará para cada servicio de Azure.

4. Seleccione **Enviar a Log Analytics** y el área de trabajo que ha creado.
5. Seleccione las categorías que desea recopilar. Consulte la documentación de cada servicio para una definición de sus categorías disponibles.

    ![Configuración de diagnóstico](media/tutorial-resource-logs/diagnostic-setting.png)

6. Haga clic en **Guardar** para guardar la configuración de diagnóstico.

    
 
 ## <a name="use-a-log-query-to-retrieve-logs"></a>Uso de una consulta de registro para recuperar registros
Los datos se recuperan de un área de trabajo de Log Analytics mediante una consulta de registro escrita en el lenguaje de consulta de Kusto (KQL). La información y las soluciones de Azure Monitor proporcionarán consultas de registro para recuperar datos de un servicio determinado, pero puede trabajar directamente con las consultas de registro y sus resultados en Azure Portal con Log Analytics. 

1. En la sección **Supervisión** del menú de su recurso, seleccione **Registros**.
2. Log Analytics se abre con una ventana de consulta vacía con el ámbito establecido en el recurso. Las consultas incluirán solo los registros de ese recurso.

    > [!NOTE]
    > Si ha abierto registros desde el menú de Azure Monitor, el ámbito se establecerá como el área de trabajo de Log Analytics. En este caso, las consultas incluirán todos los registros del área de trabajo.
   
    ![Registros](media/tutorial-resource-logs/logs.png)

4. El servicio que se muestra en el ejemplo escribe los registros de recurso en la tabla **AzureDiagnostics**, pero otros servicios pueden escribir en tablas distintas. Consulte [Servicios, esquemas y categorías admitidos en los registros de recursos de Azure](../platform/diagnostic-logs-schema.md) para conocer las tablas que usan los distintos servicios de Azure.

    > [!NOTE]
    > Varios servicios escriben registros de recurso en la tabla AzureDiagnostics. Si inicia Log Analytics desde el menú de Azure Monitor, tendrá que agregar una instrucción `where` con la columna `ResourceProvider` para especificar el servicio concreto. Al iniciar Log Analytics desde el menú de un recurso, el ámbito se establece solo en los registros de este recurso, por lo que esta columna no es necesaria. Consulte la documentación del servicio para ver consultas de ejemplo.


5. Escriba una consulta y haga clic en **Ejecutar** para inspeccionar los resultados. 
6. Para un tutorial sobre cómo escribir consultas de registro, consulte [Introducción a las consultas de registro en Azure Monitor](../log-query/get-started-queries.md).

    ![Consulta de registro](media/tutorial-resource-logs/log-query-1.png)




## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a recopilar registros de recurso en un área de trabajo Log Analytics, complete un tutorial sobre cómo escribir consultas de registro para analizar estos datos.

> [!div class="nextstepaction"]
> [Introducción a las consultas de registro en Azure Monitor](../log-query/get-started-queries.md)
