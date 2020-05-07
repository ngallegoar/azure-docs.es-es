---
title: Visualización de eventos del registro de actividad de Azure en Azure Monitor
description: Vea el registro de actividad de Azure en Azure Monitor y recupérelo con PowerShell, la CLI y la API REST.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 4ea29888d4dcf589e3e5d4dfe594f5f4bff2287e
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82559977"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Visualización y recuperación de eventos del registro de actividad de Azure

El [registro de actividad de Azure](platform-logs-overview.md) es un registro que proporciona información de los eventos de nivel de suscripción que se han producido en Azure. En este artículo se proporcionan detalles sobre los diferentes métodos para ver y recuperar eventos del registro de actividad.

## <a name="azure-portal"></a>Azure Portal
Vea el registro de actividad de todos los recursos del menú **Supervisión** de Azure Portal. Vea el registro de actividad de un recurso determinado en la opción **Registro de actividad** del menú de ese recurso.

![Visualización del registro de actividad](./media/activity-logs-overview/view-activity-log.png)

Puede filtrar los eventos del registro de actividad por los campos siguientes:

* **Intervalo de tiempo**: la hora de inicio y finalización para los eventos.
* **Categoría**: la categoría de eventos tal y como se describe en [Categorías del registro de actividad](activity-log-view.md#categories-in-the-activity-log).
* **Suscripción**: uno o más nombres de suscripción de Azure.
* **Grupo de recursos**: uno o más grupos de recursos dentro de las suscripciones seleccionadas.
* **Recurso (nombre)** : el nombre de un recurso específico.
* **Tipo de recurso**: el tipo de recurso, por ejemplo, _Microsoft.Compute/virtualmachines_.
* **Nombre de la operación**: el nombre de una operación de Azure Resource Manager, por ejemplo, _Microsoft.SQL/servers/Write_.
* **Gravedad**: Nivel de gravedad del evento. Los valores disponibles son _Informativo_, _Advertencia_, _Error_, _Crítico_.
* **Evento iniciado por**: el usuario que realizó la operación.
* **Abrir búsqueda**: se trata de un cuadro de búsqueda de texto abierto que busca esa cadena en todos los campos de todos los eventos.

## <a name="categories-in-the-activity-log"></a>Categorías del registro de actividad
Cada evento del registro de actividad tiene una categoría determinada que se describe en la tabla siguiente. Para obtener todos los detalles sobre los esquemas de estas categorías, consulte [Esquema de eventos del registro de actividad de Azure](activity-log-schema.md). 

| Category | Descripción |
|:---|:---|
| Administrativo | Contiene el registro de todas las operaciones de creación, actualización, eliminación y acción realizadas mediante Resource Manager. Algunos ejemplos de eventos administrativos incluyen la _creación de una máquina virtual_ y la _eliminación de un grupo de seguridad de red_.<br><br>Cada acción realizada por un usuario o aplicación mediante Resource Manager se modela como una operación en un tipo de recurso determinado. Si el tipo de operación es _Write_, _Delete_ o _Action_, los registros de inicio y corrección o error de esa operación se registran en la categoría Administrativo. Los eventos de la categoría Administrativo también incluyen los cambios realizados en el control de acceso basado en rol de una suscripción. |
| Service Health | Contiene el registro de los incidentes de estado del servicio que se han producido en Azure. Ejemplo de un evento de Service Health: _SQL Azure está experimentando un tiempo de inactividad en la región Este de EE. UU._ . <br><br>Los eventos de Service Health pueden encuadrarse dentro de seis variedades: _Acción requerida_, _Recuperación asistida_, _Incidente_, _Mantenimiento_, _Información_ o _Seguridad_. Estos eventos solo se crean si tiene un recurso en la suscripción que se puede ver afectado por el evento.
| Estado de los recursos | Contiene el registro de los eventos de estado de los recursos que se han producido en los recursos de Azure. Ejemplo de un evento de Resource Health: _Cambio del estado de mantenimiento de una máquina virtual a No disponible_.<br><br>Los eventos de Resource Health pueden representar uno de los cuatro estados de mantenimiento siguientes: _Disponible_, _No disponible_, _Degradado_ y _Desconocido_. Además, los eventos de Resource Health se pueden clasificar como _iniciados por la plataforma_ o _por el usuario_. |
| Alerta | Contiene el registro de activaciones de alertas de Azure. Ejemplo de un evento de alerta: _% de CPU en myVM ha estado por encima de 80 durante los últimos 5 minutos_.|
| Escalado automático | Contiene el registro de los eventos relacionados con el funcionamiento del motor de escalado automático en función de cualquier configuración de escalado automático que haya definido en la suscripción. Ejemplo de un evento de escalado automático: _Error durante la acción de escalado vertical_. |
| Recomendación | Contiene eventos de recomendación de Azure Advisor. |
| Seguridad | Contiene el registro de todas las alertas generadas por Azure Security Center. Ejemplo de un evento de seguridad: _Se ha ejecutado un archivo de extensión doble_. |
| Directiva | Contiene registros de todas las operaciones de acción de efecto realizadas por Azure Policy. Ejemplos de eventos de directiva: _Auditar_ y _Denegar_. Cada acción llevada a cabo por Azure Policy se modela como una operación en un recurso. |

## <a name="view-change-history"></a>Visualización del historial de cambios

La revisión del registro de actividad puede ayudarle a ver qué cambios ocurrieron durante ese evento. Puede consultar esta información con el **historial de cambios**. Seleccione un evento del registro de actividad del que desee obtener información más detallada. Seleccione la pestaña **Historial de cambios (versión preliminar)** para ver los cambios asociados a ese evento.

![Lista del historial de cambios para un evento](media/activity-logs-overview/change-history-event.png)

Si hay cambios asociados con el evento, verá una lista de cambios que puede seleccionar. Se abrirá la página **Historial de cambios (versión preliminar)** . En esta página verá los cambios realizados en el recurso. Como puede ver en el siguiente ejemplo, podemos ver no solo que la máquina virtual cambió de tamaño, sino también cuál era el tamaño de la máquina virtual antes de ese cambio y a qué cambió.

![Página de historial de cambios que muestra las diferencias](media/activity-logs-overview/change-history-event-details.png)

Para obtener más información sobre el historial de cambios, vea [Obtención de los cambios del recurso](../../governance/resource-graph/how-to/get-resource-changes.md).






## <a name="powershell"></a>PowerShell
Use el cmdlet [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) para recuperar el registro de actividad de PowerShell. A continuación se muestran algunos ejemplos comunes.

> [!NOTE]
> `Get-AzLog` solo proporciona 15 días de historial. Use el parámetro **-MaxRecord** para consultar los últimos N eventos más allá de 15 días. Para acceder a eventos de hace más de 15 días, use el SDK o la API REST. Si no incluye **StartTime**, el valor predeterminado será **EndTime** menos una hora. Si no incluye **EndTime**, el valor predeterminado será la hora actual. Todas las horas se muestran en UTC.


Obtención de entradas de registro creadas después de una determinada fecha y hora:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Obtención de entradas de registro entre un intervalo de fecha y hora:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obtención de entradas de registro de un grupo de recursos específico:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Obtención de entradas de registro de un proveedor de recursos específico entre un intervalo de fecha y hora:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obtención de entradas de registro con un autor de llamada concreto:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Obtención de los últimos 1000 eventos:

```powershell
Get-AzLog -MaxRecord 1000
```


## <a name="cli"></a>CLI
Use [az monitor activity-log](cli-samples.md#view-activity-log-for-a-subscription) para recuperar el registro de actividad de la CLI. A continuación se muestran algunos ejemplos comunes.


Vea todas las opciones disponibles.

```azurecli
az monitor activity-log list -h
```

Obtención de entradas de registro de un grupo de recursos específico:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Obtención de entradas de registro con un autor de llamada concreto:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Obtención de registros por autor de llamada en un tipo de recurso, dentro de un intervalo de fechas:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>API DE REST
Use la [API REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/) para recuperar el registro de actividad de un cliente de REST. A continuación se muestran algunos ejemplos comunes.

Obtención de registros de actividad con filtro:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Obtención de registros de actividad con filtro y selección:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obtención de registros de actividad con selección:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obtención de registros de actividad sin filtro ni selección:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="next-steps"></a>Pasos siguientes

* [Introducción a los registros de plataforma Azure](platform-logs-overview.md)
* [Creación de la configuración de diagnóstico para enviar registros de actividad a otros destinos](diagnostic-settings.md)
