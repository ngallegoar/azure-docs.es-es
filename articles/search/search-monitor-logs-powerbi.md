---
title: Visualización de registros y métricas de Azure Cognitive Search con Power BI
description: Visualización de registros y métricas de Azure Cognitive Search con Power BI
manager: eladz
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 618033e24236d26601fafaf7b0678ee83ec973e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650178"
---
# <a name="visualize-azure-cognitive-search-logs-and-metrics-with-power-bi"></a>Visualización de registros y métricas de Azure Cognitive Search con Power BI
[Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-what-is-azure-search) permite almacenar registros de operaciones y métricas de servicio sobre su servicio de búsqueda en una cuenta de Azure Storage. En esta página se proporcionan instrucciones sobre cómo puede visualizar esa información a través de una aplicación de plantilla de Power BI. La aplicación proporciona información detallada sobre su servicio de búsqueda, incluida información acerca de las métricas de búsqueda, indexación, operaciones y servicio.

Puede buscar la aplicación de plantilla de Power BI **Azure Cognitive Search: Análisis de registros y métricas** en [Marketplace de aplicaciones de Power BI](https://appsource.microsoft.com/marketplace/apps).

## <a name="how-to-get-started-with-the-app"></a>Introducción a la aplicación
1. Activación del registro de diagnóstico para el servicio de búsqueda:
    1. Cree o identifique una [cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) existente donde pueda archivar los registros.
    1. Vaya al servicio Azure Cognitive Search en Azure Portal.
    1. En la sección Supervisión de la columna izquierda, seleccione **Configuración de diagnóstico**.

        ![](media/search-monitor-logs-powerbi/diagnostic-settings.png)

    1. Seleccione **+ Agregar configuración de diagnóstico**.
    1. Compruebe **Archivar en una cuenta de almacenamiento**, proporcione su información de la cuenta de almacenamiento y compruebe **OperationLogs** y **AllMetrics**.

        ![](media/search-monitor-logs-powerbi/add-diagnostic-setting.png)
    1. Seleccione **Guardar**.

1. Una vez habilitado el registro, use el servicio de búsqueda para empezar a generar registros y métricas. Transcurre hasta una hora antes de que los contenedores aparezcan en Blob Storage con estos registros. Verá un contenedor **insights-logs-operationlogs** para los registros de tráfico de búsqueda y un contenedor **insights-metrics-pt1m** para las métricas.

1. Busque la aplicación de Power BI de Azure Cognitive Search en [Marketplace de aplicaciones de Power BI](https://appsource.microsoft.com/marketplace/apps) e instálela en una nueva área de trabajo o un área de trabajo existente. La aplicación se llama **Azure Cognitive Search: Análisis de registros y métricas**.

1. Después de instalar la aplicación, seleccione la aplicación en su lista de aplicaciones de Power BI.

    ![](media/search-monitor-logs-powerbi/azure-search-app-tile.png)

1. Seleccione **Conectar** para conectar sus datos.

    ![](media/search-monitor-logs-powerbi/get-started-with-your-new-app.png)

1. Escriba el nombre de la cuenta de almacenamiento que contiene sus registros y métricas. De forma predeterminada, la aplicación revisará los últimos 10 días de datos, pero este valor puede cambiarse con el parámetro **Days**.

    ![](media/search-monitor-logs-powerbi/connect-to-storage-account.png)

1. Seleccione **Clave** como método de autenticación y proporcione su clave de cuenta de almacenamiento. Seleccione **Privado** como nivel de privacidad. Haga clic en Iniciar sesión para iniciar el proceso de carga.

    ![](media/search-monitor-logs-powerbi/connect-to-storage-account-step-two.png)

1. Espere a que se actualicen los datos. Esto puede tardar algún tiempo en función de la cantidad de datos. Puede ver si los datos siguen actualizándose en función del indicador siguiente.

    ![](media/search-monitor-logs-powerbi/workspace-view-refreshing.png)

1. Una vez que se haya completado la actualización de datos, seleccione **Informe de Azure Cognitive Search** para ver el informe.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-report.png)

1. Asegúrese de actualizar la página después de abrir el informe para que se abra con sus datos.

    ![](media/search-monitor-logs-powerbi/powerbi-search.png)

## <a name="how-to-change-the-app-parameters"></a>Cómo cambiar los parámetros de la aplicación
Si desea visualizar datos en otra cuenta de almacenamiento o cambiar el número de días de datos que se van a consultar, siga los pasos siguientes para cambiar los parámetros **Days** y **StorageAccount**.

1. Vaya a sus aplicaciones de Power BI, busque su aplicación de Azure Cognitive Search y seleccione el botón **Editar aplicación** para ver el área de trabajo.

    ![](media/search-monitor-logs-powerbi/azure-search-app-tile-edit.png)

1. Seleccione **Configuración** en las opciones de conjunto de datos.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-settings.png)

1. En la pestaña Conjuntos de datos, cambie los valores del parámetro y seleccione **Aplicar**. Si hay un problema con la conexión, actualice las credenciales de origen de datos en la misma página.

1. Vuelva al área de trabajo y seleccione **Actualizar ahora** en las opciones de conjunto de datos.

    ![](media/search-monitor-logs-powerbi/workspace-view-select-refresh-now.png)

1. Abra el informe para ver los datos actualizados. También es posible que tenga que actualizar el informe para ver los datos más recientes.

## <a name="troubleshooting"></a>Solución de problemas
Si ve que no puede ver sus datos, siga estos pasos para solucionar problemas:

1. Abra el informe y actualice la página para asegurarse de que está viendo los datos más recientes. Hay una opción en el informe para actualizar los datos. Selecciónela para obtener los datos más recientes.

1. Asegúrese de que el nombre de la cuenta de almacenamiento y la clave de acceso que proporcionó son correctos. El nombre de la cuenta de almacenamiento debe corresponder a la cuenta configurada con sus registros de servicios de búsqueda.

1. Confirme que su cuenta de almacenamiento contiene los contenedores **insights-logs-operationlogs** e **insights-metrics-pt1m** y que cada contenedor tiene datos. Los registros y las métricas estarán dentro de un par de niveles de carpetas.

1. Compruebe si sigue actualizándose el conjunto de datos. El indicador de estado de actualización se muestra en el paso 8 anterior. Si se sigue actualizando, espere a que se haya completado la actualización para abrir y actualizar el informe.

## <a name="next-steps"></a>Pasos siguientes
[Más información sobre Azure Cognitive Search](https://docs.microsoft.com/azure/search/)

[¿Qué es Power BI?](https://docs.microsoft.com/power-bi/fundamentals/power-bi-overview)

[Conceptos básicos para diseñadores en el servicio Power BI](https://docs.microsoft.com/power-bi/service-basic-concepts)