---
title: 'Consulta de datos desde un entorno de versión preliminar mediante C#: Azure Time Series Insights | Microsoft Docs'
description: Aprenda a consultar datos desde un entorno de Azure Time Series Insights mediante una aplicación escrita en C#.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/14/2020
ms.custom: seodec18
ms.openlocfilehash: fbc2cbc29cb23a21e7d3713091fc22f01bb1b15a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81379828"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Consulta de datos desde el entorno de versión preliminar de Azure Time Series Insights mediante C#

En este ejemplo de C# se muestra cómo consultar datos desde las [API de acceso a datos en versión preliminar](https://docs.microsoft.com/rest/api/time-series-insights/preview) en los entornos de Azure Time Series Insights (versión preliminar).

> [!TIP]
> Consulte los ejemplos de código de C# de la versión preliminar en [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample).

## <a name="summary"></a>Resumen

En el código de ejemplo siguiente se muestran las siguientes características:

* Compatibilidad con la generación automática del SDK desde [Azure AutoRest](https://github.com/Azure/AutoRest).
* Cómo adquirir un token de acceso a través de Azure Active Directory con [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).
* Cómo pasar el token de acceso adquirido en el encabezado `Authorization` de las posteriores solicitudes de Data Access API. 
* El ejemplo proporciona una interfaz de consola que muestra cómo se realizan las solicitudes HTTP en:

    * [API de entornos (versión preliminar)](https://docs.microsoft.com/rest/api/time-series-insights/preview#preview-environments-apis)
        * [Get Environments Availability API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability) y [Get Event Schema API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema)
    * [API de consulta (versión preliminar)](https://docs.microsoft.com/rest/api/time-series-insights/preview#query-apis)
        * [Get Events API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents), [Get Series API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries) y [Get Aggregate Series API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
    * [API de modelo de serie temporal](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
        * [Get Hierarchies API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/get) y [Hierarchies Batch API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/executebatch)
        * [Get Types API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/get) y [Types Batch API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/executebatch)
        * [Get Instances API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/get) e [Instances Batch API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/executebatch)
* Funcionalidades avanzadas de [búsqueda](https://docs.microsoft.com/rest/api/time-series-insights/preview#search-features) y [TSX](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax).

## <a name="prerequisites-and-setup"></a>Requisitos previos y configuración

Realice los pasos siguientes antes de compilar y ejecutar el código de ejemplo:

1. [Aprovisione un entorno de Azure Time Series Insights (versión preliminar)](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment).
1. Configure el entorno de Azure Time Series Insights para Azure Active Directory, tal como se describe en [Autenticación y autorización](time-series-insights-authentication-and-authorization.md). 
1. Ejecute el archivo [GenerateCode.bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/GenerateCode.bat) como se especifica en el archivo [Readme.md](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/Readme.md) para generar las dependencias de cliente de Time Series Insights (versión preliminar).
1. Abra la solución `TSIPreviewDataPlaneclient.sln` y establezca `DataPlaneClientSampleApp` como proyecto predeterminado en Visual Studio.
1. Instale las dependencias del proyecto necesarias, para lo cual debe seguir los pasos que se describen [a continuación](#project-dependencies), y compile el ejemplo en un archivo `.exe` ejecutable.
1. Ejecute el archivo `.exe` haciendo doble clic en él.

## <a name="project-dependencies"></a>Dependencias del proyecto

Se recomienda usar la versión más reciente de Visual Studio:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/), versión 16.4.2+

El código de ejemplo tiene varias dependencias requeridas, que se pueden ver en el archivo [packages.config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config).

Para descargar los paquetes en Visual Studio 2019, seleccione la opción **Compilar** > **Compilar solución**. 

Como alternativa, agregue cada paquete con [NuGet 2.12+](https://www.nuget.org/). Por ejemplo:

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>Código de ejemplo de C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> * El código de ejemplo se puede ejecutar sin modificar los valores de entorno predeterminados.
> * El código de ejemplo se compilará en una aplicación de consola ejecutable de .NET.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la consulta, vea la [referencia sobre las API de consulta](https://docs.microsoft.com/rest/api/time-series-insights/preview-query).

- Lea cómo [conectar una aplicación de JavaScript mediante el SDK de cliente](https://github.com/microsoft/tsiclient) a Time Series Insights.
