---
title: archivo de inclusión
description: archivo de inclusión
services: data-factory
author: memildin
ms.service: data-factory
ms.topic: include
ms.date: 03/10/2020
ms.author: memildin
ms.custom: include file
ms.openlocfilehash: 55390a3eb2a074729b4a0868416a95e208325b76
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91401058"
---
```json
{
  "VendorName": "Microsoft",
  "AlertType": "SUSPECT_SVCHOST",
  "StartTimeUtc": "2016-12-20T13:38:00.000Z",
  "EndTimeUtc": "2019-12-20T13:40:01.733Z",
  "ProcessingEndTime": "2019-09-16T12:10:19.5673533Z",
  "TimeGenerated": "2016-12-20T13:38:03.000Z",
  "IsIncident": false,
  "Severity": "High",
  "Status": "New",
  "ProductName": "Azure Security Center",
  "SystemAlertId": "2342409243234234_F2BFED55-5997-4FEA-95BD-BB7C6DDCD061",
  "CompromisedEntity": "WebSrv1",
  "Intent": "Execution",
  "AlertDisplayName": "Suspicious process detected",
  "Description": "Suspicious process named 'SVCHOST.EXE' was running from path: %{Process Path}",
  "RemediationSteps": ["contact your security information team"],
  "ExtendedProperties": {
    "Process Path": "c:\\temp\\svchost.exe",
    "Account": "Contoso\\administrator",
    "PID": 944,
    "ActionTaken": "Detected"
  },
  "Entities": [],
  "ResourceIdentifiers": [
        {
            Type: "AzureResource",
            AzureResourceId: "/subscriptions/86057C9F-3CDD-484E-83B1-7BF1C17A9FF8/resourceGroups/backend-srv/providers/Microsoft.Compute/WebSrv1"
        },
        {
            Type: "LogAnalytics",
            WorkspaceId: "077BA6B7-8759-4F41-9F97-017EB7D3E0A8",
            WorkspaceSubscriptionId: "86057C9F-3CDD-484E-83B1-7BF1C17A9FF8",
            WorkspaceResourceGroup: "omsrg",
            AgentId: "5A651129-98E6-4E6C-B2CE-AB89BD815616",
        }
  ]
}
```

### <a name="the-data-model-of-the-schema"></a>El modelo de datos del esquema

|Campo|Tipo de datos|Descripción|
|----|----|----|
|**AlertDisplayName**|String|Nombre para mostrar de la alerta.|
|**AlertType**|String|Tipo de alerta. Las alertas del mismo tipo deben tener el mismo valor. Este campo es una cadena con clave que representa el tipo de alerta y no una instancia de alerta. Todas las instancias de alerta de la misma lógica o análisis de detección deben tener el mismo valor para el tipo de alerta.|
|**CompromisedEntity**|String|Nombre para mostrar del recurso más relacionado con esta alerta.|
|**Descripción**|String|Descripción de la alerta|
|**EndTimeUtc**|DateTime|Hora del último evento o actividad incluido en la alerta.  El campo debe ser una cadena que se ajuste al formato ISO8601, incluida la información de la zona horaria UTC.|
|**Entidades**|IEnumerable (IEntity)|Lista de entidades relacionadas con la alerta. Esta lista puede contener una combinación de entidades de diversos tipos. El tipo de entidades puede ser cualquiera de los tipos definidos en la sección Entidades. Las entidades que no están en la lista siguiente también se pueden enviar; no obstante, no se garantiza que se procesen (la alerta no superará la validación con nuevos tipos de entidades).|
|**ExtendedProperties**|Dictionary (String,String)|Los proveedores pueden (opcionalmente) incluir aquí campos personalizados.|
|**Intención**|Enum|Intención relacionada con la cadena de eliminación detrás de la alerta. Para obtener una lista de los valores admitidos, así como explicaciones sobre las intenciones de la cadena de eliminación admitidas de Azure Security Center, consulte [Intenciones](../articles/security-center/alerts-reference.md#intentions).<br/>Este campo puede tener varios valores (separados por coma).|
|**IsIncident**|Bool|Este campo determina si la alerta es un incidente (una agrupación compuesta de varias alertas) o una única alerta. El valor predeterminado para el campo es "false" (es decir, una alerta única).|
|**ProcessingEndTime**|DateTime|Hora en la que el usuario final pudo acceder a la alerta en el producto original que contiene la alerta.|
|**ProductName**|String|Nombre del producto que publicó esta alerta (Azure Security Center, ATP de Azure, ATP de Microsoft Defender, MCAS, etc.).|
|**RemediationSteps**|List<String>|Elementos de acción manual que se deben llevar a cabo para corregir la alerta.|
|**ResourceIdentifiers**|List (identificadores de recursos)|Los identificadores de recursos para esta alerta que se pueden usar para dirigir la alerta al grupo de exposición del producto adecuado (inquilino, área de trabajo, suscripción, etc.). Puede haber varios identificadores de tipo diferente por alerta.|
|**Gravedad**|Enum|La gravedad de la alerta según lo notifique el proveedor. Valores posibles: Informativo, Bajo, Medio y Alto.|
|**StartTimeUtc**|DateTime|Hora del primer evento o actividad incluido en la alerta. El campo debe ser una cadena que se ajuste al formato ISO8601, incluida la información de la zona horaria UTC.|
|**Estado**|Enum|Estado del ciclo de vida de la alerta.<br/>Los estados admitidos son: Nuevo, Resuelto, Descartado, Desconocido.<br/>A una alerta que especifica un valor distinto de las opciones admitidas se le asigna el estado "Desconocido".<br/>A una alerta que no especifica un valor se le asigna el estado "Nuevo".|
|**SystemAlertId**|String|Identificador de la alerta.|
|**TimeGenerated**|DateTime|Hora en la que el proveedor de la alerta generó la alerta. Si los proveedores de alertas internos no lo notifican, un producto puede optar por asignar la hora a la que se recibió para su procesamiento por parte del producto.  El campo debe ser una cadena que se ajuste al formato ISO8601, incluida la información de la zona horaria UTC.|
|**VendorName**|String|Nombre del proveedor que genera la alerta.|
|||
