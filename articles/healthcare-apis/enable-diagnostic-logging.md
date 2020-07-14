---
title: Habilitación del registro de diagnóstico en Azure API for FHIR®
description: En este artículo se explica cómo habilitar el registro de diagnóstico en Azure API for FHIR®
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.reviewer: dseven
ms.author: cavoeg
author: CaitlinV39
ms.date: 11/01/2019
ms.openlocfilehash: 948ca03b5bf503c884df5df56c61951b381874a9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84870875"
---
# <a name="enable-diagnostic-logging-in-azure-api-for-fhir"></a>Habilitación del registro de diagnóstico en Azure API for FHIR®

En este artículo aprenderá a habilitar el registro de diagnóstico en Azure API for FHIR® y podrá revisar algunas consultas de ejemplo para estos registros. El acceso a los registros de diagnóstico es esencial para cualquier servicio del sector sanitario en el que el cumplimiento de los requisitos normativos (como HIPAA) es imprescindible. La característica de Azure API for FHIR® que habilita los registros de diagnóstico es [**Configuración de diagnóstico**](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) en Azure Portal. 

## <a name="enable-audit-logs"></a>Habilitación de los registros de auditoría
1. Para habilitar el registro de diagnóstico en Azure API for FHIR®, seleccione el servicio Azure API for FHIR® en Azure Portal. 
2. Vaya a **Configuración de diagnóstico** 
![Diagnostic settings](media/diagnostic-logging/diagnostic-settings-screen.png). 

3. Seleccione **+ Agregar configuración de diagnóstico**.

4. Escriba un nombre para la configuración.

5. Seleccione el método que desea usar para acceder a los registros de diagnóstico:

    1. **Archivar en una cuenta de almacenamiento** para auditoría o inspección manual. Es necesario que la cuenta de almacenamiento que desea usar ya esté creada.
    2. **Transmitir a un centro de eventos** para la ingesta en un servicio de terceros o una solución de análisis personalizado. Tendrá que crear un espacio de nombres de centro de eventos y una directiva de centro de eventos para poder configurar este paso.
    3. **Transmitir al área de trabajo de Log Analytics** en Azure Monitor. Tendrá que crear el área de trabajo de Log Analytics para poder seleccionar esta opción.

6. Seleccione **AuditLogs** y las métricas que desee capturar.

7. Haga clic en Guardar

> [!Note] 
> Los primeros registros pueden tardar hasta quince minutos en mostrarse en Log Analytics.  
 
Para más información sobre cómo trabajar con los registros de diagnóstico, consulte la [documentación del registro de recursos de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview).

## <a name="audit-log-details"></a>Detalles del registro de auditoría
En este momento, el servicio Azure API for FHIR® devuelve los campos siguientes en el registro de auditoría: 

|Nombre del campo  |Tipo  |Notas  |
|---------|---------|---------|
|CallerIdentity|Dinámica|Bolsa de propiedades genérica que contiene información de identidad
|CallerIdentityIssuer|String|Emisor 
|CallerIdentityObjectId|String|Object_Id 
|CallerIPAddress|String|Dirección IP del autor de la llamada 
|CorrelationId|String| Id. de correlación
|FhirResourceType|String|El tipo de recurso para el que se ejecutó la operación
|LogCategory|String|Categoría de registro (actualmente se devuelve "AuditLogs" LogCategory)
|Location|String|La ubicación del servidor que procesó la solicitud (por ejemplo, Centro-sur de EE. UU.)
|OperationDuration|Int|El tiempo que se tardó en completar esta solicitud en segundos
|OperationName|String| Describe el tipo de operación (por ejemplo, actualizar o tipo de búsqueda)
|RequestUri|String|URI de solicitud 
|ResultType|String|Los valores disponibles actualmente son **Started**, **Succeeded** o **Failed**
|StatusCode|Int|El código de estado HTTP. (por ejemplo, 200) 
|TimeGenerated|DateTime|Fecha y hora del evento|
|Propiedades|String| Describe las propiedades de fhirResourceType
|SourceSystem|String| Sistema de origen (siempre Azure en este caso)
|TenantId|String|Id. de inquilino
|Tipo|String|Tipo de registro (siempre MicrosoftHealthcareApisAuditLog en este caso)
|_ResourceId|String|Detalles sobre el recurso

## <a name="sample-queries"></a>Consultas de ejemplo

Estas son algunas consultas básicas de Application Insights que puede utilizar para explorar los datos de registro.

Ejecute esta consulta para ver los **cien registros más recientes**:

```Application Insights
MicrosoftHealthcareApisAuditLogs
| limit 100
```

Ejecute esta consulta para agrupar las operaciones por **tipo de recurso de FHIR**:

```Application Insights
MicrosoftHealthcareApisAuditLogs 
| summarize count() by FhirResourceType
```

Ejecute esta consulta para obtener todos los **resultados con errores**:

```Application Insights
MicrosoftHealthcareApisAuditLogs 
| where ResultType == "Failed" 
```

## <a name="conclusion"></a>Conclusión 
Tener acceso a los registros de diagnóstico es esencial para supervisar un servicio y proporcionar informes de cumplimiento. Azure API for FHIR® le permite realizar estas acciones a través de registros de diagnóstico. 
 
FHIR® es la marca registrada de HL7 y se usa con su permiso.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, aprendió a habilitar los registros de auditoría para Azure API for FHIR®. A continuación, obtenga información sobre otras valores adicionales que puede configurar en Azure API for FHIR.
 
>[!div class="nextstepaction"]
>[Configuración adicional](azure-api-for-fhir-additional-settings.md)