---
title: SDK de Azure Event Grid
description: Se describen los SDK de Azure Event Grid. Estos SDK proporcionan administración, publicación y consumo.
ms.topic: reference
ms.date: 07/07/2019
ms.openlocfilehash: 5e93448ef703be88583cd59ae5eee9d1993c4054
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91322536"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>SDK de Event Grid para administración y publicación

Event Grid proporciona SDK que permiten administrar los recursos y exponer eventos mediante programación.

## <a name="management-sdks"></a>SDK de administración

Los SDK de administración permiten crear, actualizar y eliminar suscripciones y temas de Event Grid. En la actualidad, están disponibles los siguientes SDK:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>SDK de plano de datos

Los SDK de planos de datos permiten publicar eventos en temas, y se aseguran de autenticar, formar el evento y publicarlo de forma asincrónica en el punto de conexión especificado. También permiten consumir eventos de primera entidad. En la actualidad, están disponibles los siguientes SDK:

| Lenguaje de programación | SDK | 
| -------------------- | ---------- | ---------- | 
| .NET | SDK estable: [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)<p>SDK de versión preliminar: [Azure.Messaging.EventGrid](https://www.nuget.org/packages/Azure.Messaging.EventGrid/) |
| Java | SDK estable: [azure-eventgrid](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)<p>SDK de versión preliminar: [azure-messaging-eventgrid](https://search.maven.org/artifact/com.azure/azure-messaging-eventgrid/)</p> |  
| Python | [azure-eventgrid](https://pypi.org/project/azure-eventgrid/#history) (consulte las versiones preliminares y estables más recientes en la página **Historial de versiones**) |
| JavaScript | [@azure/eventgrid](https://www.npmjs.com/package/@azure/eventgrid/) (vaya a la pestaña **Versiones** para ver los paquetes de versión beta y estable más recientes). | 
| Go | [Azure SDK para Go](https://github.com/Azure/azure-sdk-for-go) | | 
| Ruby | [azure_event_grid](https://rubygems.org/gems/azure_event_grid) | | 


## <a name="next-steps"></a>Pasos siguientes

* Para obtener aplicaciones de ejemplo, consulte [Ejemplos de código de Event Grid](https://azure.microsoft.com/resources/samples/?sort=0&service=event-grid).
* Para ver una introducción a Event Grid, consulte el artículo acerca de [qué es Event Grid](overview.md).
* Para consultar los comandos de Event Grid en la CLI de Azure, vea [CLI de Azure](/cli/azure/eventgrid).
* Para consultar los comandos de Event Grid en PowerShell, vea [PowerShell](/powershell/module/az.eventgrid).
