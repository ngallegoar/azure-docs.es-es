---
title: Recopilación de registros de recursos de Azure en el área de trabajo de Log Analytics
description: Aprenda a transmitir registros de Azure Diagnostics a un área de trabajo de Log Analytics en Azure Monitor.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 36bd464624118b7671a3879bcc1d34114bba9ce3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226276"
---
# <a name="collect-azure-platform-logs-in-log-analytics-workspace-in-azure-monitor"></a>Recopilación de registros de plataforma de Azure en el área de trabajo de Log Analytics en Azure Monitor
Los [registros de plataforma](platform-logs-overview.md) en Azure, incluidos los registros de recurso y los registros de actividad de Azure, proporcionan información detallada de diagnóstico y auditoría para los recursos de Azure y la plataforma de Azure de la que dependen. En este artículo se describe la recopilación de registros de recurso en un área de trabajo de Log Analytics que permita el análisis con otros datos de supervisión recopilados en los registros de Azure Monitor mediante consultas de registro eficaces y también el aprovechamiento de otras características de Azure Monitor, como las alertas y las visualizaciones. 


## <a name="what-you-can-do-with-platform-logs-in-a-workspace"></a>Qué se puede hacer con los registros de plataforma en un área de trabajo
La recopilación de registros de plataforma en un área de trabajo de Log Analytics permite analizar los registros de todos los recursos de Azure de forma conjunta y aprovechar todas las características disponibles para los [registros de Azure Monitor](data-platform-logs.md), que incluyen lo siguiente:

* **Consultas de registro**: cree [consultas de registro](../log-query/log-query-overview.md) con un lenguaje de consulta eficaz para analizar y obtener información sobre los datos de diagnóstico rápidamente, y analizarlos junto con los datos recopilados de otros orígenes en Azure Monitor.
* **Alertas**: obtenga una notificación proactiva de los patrones y las condiciones críticas identificados en los registros de recurso mediante [alertas de registro en Azure Monitor](alerts-log.md).
* **Visualizaciones**: ancle los resultados de una consulta de registro a un panel de Azure o inclúyala en un libro como parte de un informe interactivo.

## <a name="prerequisites"></a>Prerrequisitos
Si no la tiene, [cree un área de trabajo](../learn/quick-create-workspace.md). El área de trabajo no tiene que estar en la misma suscripción que la del recurso que envía los registros, siempre que el usuario que realiza la configuración tenga el acceso RBAC adecuado a ambas suscripciones.

## <a name="create-a-diagnostic-setting"></a>Creación de una configuración de diagnóstico
Envíe los registros de plataforma a un área de trabajo de Log Analytics y otros destinos mediante la creación de configuración de diagnóstico para un recurso de Azure. Consulte [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](diagnostic-settings.md) para más información.


## <a name="activity-log-collection"></a>Recopilación de registros de actividades
Puede enviar el registro de actividad desde cualquier suscripción única hasta un máximo de cinco áreas de trabajo de Log Analytics. Los datos del registro de recursos recopilados en un área de trabajo de Log Analytics se almacenan en la tabla de **AzureActivity**. 

## <a name="resource-log-collection-mode"></a>Modo de recopilación de registros de recursos
Los datos de registros de recursos recopilados en un área de trabajo de Log Analytics se almacenan en tablas, tal como se describe en [Estructura de registros de Azure Monitor](../log-query/logs-structure.md). Las tablas que usan los registros de recurso dependen del tipo de colección del recurso:

- Azure Diagnostics: todos los datos se escriben en la tabla _AzureDiagnostics_.
- Específicos del recurso: los datos se escriben en una tabla individual para cada categoría del recurso.

### <a name="azure-diagnostics-mode"></a>Modo Azure Diagnostics 
En este modo se recopilan todos los datos de cualquier [configuración de diagnóstico](diagnostic-settings.md) en la tabla _AzureDiagnostics_. Este es el método heredado que usa hoy la mayoría de los servicios de Azure.

Dado que varios recursos envían datos a la misma tabla, el esquema de esta tabla es el superconjunto de los esquemas de todos los tipos de datos diferentes que se recopilan.

Considere el ejemplo siguiente, donde se recopila la configuración de diagnóstico en la misma área de trabajo para los siguientes tipos de datos:

- Registros de auditoría del servicio 1 (con un esquema que consta de las columnas A, B y C)  
- Registros de errores del servicio 1 (con un esquema que consta de las columnas D, E y F)  
- Registros de auditoría del servicio 2 (con un esquema que consta de las columnas G, H e I)  

La tabla AzureDiagnostics tendrá el siguiente aspecto:  

| ResourceProvider    | Category     | Un  | B  | C  | D  | E  | F  | G  | H  | I  |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Microsoft.Service1 | AuditLogs    | x1 | y1 | z1 |    |    |    |    |    |    |
| Microsoft.Service1 | ErrorLogs    |    |    |    | q1 | w1 | e1 |    |    |    |
| Microsoft.Service2 | AuditLogs    |    |    |    |    |    |    | j1 | k1 | l1 |
| Microsoft.Service1 | ErrorLogs    |    |    |    | q2 | w2 | e2 |    |    |    |
| Microsoft.Service2 | AuditLogs    |    |    |    |    |    |    | j3 | k3 | l3 |
| Microsoft.Service1 | AuditLogs    | x5 | y5 | z5 |    |    |    |    |    |    |
| … |

### <a name="resource-specific"></a>Específico del recurso
En este modo se crean tablas individuales en el área de trabajo seleccionada para cada categoría seleccionada en la configuración de diagnóstico. Se recomienda este método porque facilita el trabajo con los datos en las consultas de registro, proporciona mejor capacidad de detección de esquemas y su estructura, mejora el rendimiento en la latencia de ingesta y en los tiempos de consulta, y ofrece la capacidad de conceder derechos de RBAC en un tabla específica. Todos los servicios de Azure se migrarán finalmente al modo específico del recurso. 

En el ejemplo anterior, esto daría lugar a la creación de tres tablas:
 
- Tabla *Service1AuditLogs*:

    | Proveedor de recursos | Category | Un | B | C |
    | -- | -- | -- | -- | -- |
    | Service1 | AuditLogs | x1 | y1 | z1 |
    | Service1 | AuditLogs | x5 | y5 | z5 |
    | … |

- Tabla *Service1ErrorLogs*:  

    | Proveedor de recursos | Category | D | E | F |
    | -- | -- | -- | -- | -- | 
    | Service1 | ErrorLogs |  q1 | w1 | e1 |
    | Service1 | ErrorLogs |  q2 | w2 | e2 |
    | … |

- Tabla *Service2AuditLogs*:  

    | Proveedor de recursos | Category | G | H | I |
    | -- | -- | -- | -- | -- |
    | Service2 | AuditLogs | j1 | k1 | l1|
    | Service2 | AuditLogs | j3 | k3 | l3|
    | … |



### <a name="select-the-collection-mode"></a>Selección del modo de recopilación
La mayoría de los recursos de Azure escribirán los datos en el área de trabajo en modo **Azure Diagnostics** o **específico del recurso** sin que usted pueda elegir. Consulte la [documentación para cada servicio](diagnostic-logs-schema.md) para más información sobre el modo que usa. Todos los servicios de Azure usarán finalmente el modo específico del recurso. Como parte de esta transición, algunos recursos permitirán seleccionar un modo en la configuración de diagnóstico. Debe especificar el modo específico del recurso para las configuraciones de diagnóstico nuevas, ya que esto facilita la administración de los datos y puede ayudar a evitar migraciones complejas posteriores.
  
   ![Selector del modo de configuración de diagnósticos](media/resource-logs-collect-workspace/diagnostic-settings-mode-selector.png)




> [!NOTE]
> Actualmente solo se puede seleccionar el modo **Azure Diagnostics** o **específico del recurso** al realizar la configuración de diagnóstico en Azure Portal. Si realizar la configuración mediante la CLI, PowerShell o API REST, de forma predeterminada será **Azure Diagnostics**.

En el modo específico del recurso se puede modificar una configuración de diagnóstico existente. En este caso, los datos que ya se han recopilado permanecerán en la tabla _AzureDiagnostics_ hasta que se eliminen de acuerdo con la configuración de retención del área de trabajo. Los nuevos datos se recopilarán en la tabla dedicada. Use el operador [union](https://docs.microsoft.com/azure/kusto/query/unionoperator) para consultar datos en ambas tablas.

Puede continuar por el blog [Actualizaciones de Azure](https://azure.microsoft.com/updates/) para ver noticias sobre los servicios de Azure que admiten el modo específico del recurso.

### <a name="column-limit-in-azurediagnostics"></a>Límite de columnas en AzureDiagnostics
En los registros de Azure Monitor hay un límite de 500 propiedades por tabla. Una vez alcanzado este límite, cualquier fila que contenga datos con cualquier propiedad fuera de las primeras 500 se eliminará en el momento de la ingesta. Estos límites afectan particularmente a la tabla *AzureDiagnostics*, ya que incluye propiedades para todos los servicios de Azure que escriben en ella.

Si va a recopilar registros de recursos de varios servicios, _AzureDiagnostics_ puede superar este límite, y se omitirán datos. Hasta que todos los servicios de Azure admitan el modo específico del recurso, debe configurar los recursos para escribir en varias áreas de trabajo con el fin de reducir la posibilidad de alcanzar el límite de 500 columnas.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory, debido a un conjunto muy detallado de registros, es un servicio del cual se sabe que escribe un gran número de columnas y que puede provocar que _AzureDiagnostics_ supere el límite. Para cualquier configuración de diagnóstico configurada antes de que se habilitara el modo específico del recurso, se creará una nueva columna para cada parámetro de usuario con nombre único en cualquier actividad. Se crearán más columnas debido a la naturaleza detallada de las entradas y salidas de la actividad.
 
Debe migrar los registros para usar el modo específico del recurso lo antes posible. Si no puede hacerlo inmediatamente, una solución alternativa provisional es aislar los registros de Azure Data Factory en su propia área de trabajo para minimizar la posibilidad de que estos registros afecten a otros tipos de registro que se recopilan en las áreas de trabajo.


## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los registros de recurso](platform-logs-overview.md).
* [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](diagnostic-settings.md).
