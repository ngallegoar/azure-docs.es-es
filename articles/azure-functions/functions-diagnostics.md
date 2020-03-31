---
title: Información general sobre diagnósticos de Azure Functions
description: Obtenga información sobre cómo solucionar incidencias con la aplicación de funciones con los diagnósticos de Azure Functions.
author: yunjchoi
ms.topic: article
ms.date: 11/01/2019
ms.author: yunjchoi
ms.custom: na
ms.openlocfilehash: 41acc5703f6b6e5cdeedf2afd0dc61b23bc579c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834049"
---
# <a name="azure-functions-diagnostics-overview"></a>Información general sobre diagnósticos de Azure Functions

Al ejecutar una aplicación de función, quiere estar preparado para las incidencias que puedan surgir, desde errores 4xx hasta errores de desencadenador. Los diagnósticos de Azure Functions son una experiencia inteligente e interactiva que le ayuda a solucionar los problemas de la aplicación de funciones sin ninguna configuración o costo adicional. Cuando surgen incidencias con la aplicación de funciones, los diagnósticos de Azure Functions le indicarán lo que pasa y le guiarán a la información correcta de forma que pueda solucionar la incidencia de la manera más fácil y rápida. En este artículo se muestran los conceptos básicos sobre cómo usar los diagnósticos de Azure Functions para diagnosticar y resolver incidencias comunes de la aplicación de funciones.

## <a name="start-azure-functions-diagnostics"></a>Inicio de diagnósticos de Azure Functions

Para acceder a los diagnósticos de Azure Functions:

1. Vaya a la aplicación de funciones en [Azure Portal](https://portal.azure.com).
2. Seleccione la pestaña **Características de la plataforma**.
3. Seleccione **Diagnosticar y resolver problemas** en **Administración de recursos**, para abrir los diagnósticos de Azure Functions.
4. Elija una categoría que describa mejor la incidencia de la aplicación de funciones mediante las palabras clave en el icono de Página principal. También puede escribir una palabra clave que describa mejor la incidencia en la barra de búsqueda. Por ejemplo, podría escribir `execution` para ver una lista de los informes de diagnóstico relacionados con la ejecución de la aplicación de funciones y abrirlos directamente desde la página principal.

![Página principal](./media/functions-diagnostics/homepage.png)

## <a name="use-the-interactive-interface"></a>Uso de la interfaz interactiva

Una vez que seleccione una categoría de la página principal que mejor se corresponda con el problema de la aplicación de funciones, la interfaz interactiva de diagnósticos de Azure Functions, Genie, le guiará a través del diagnóstico y la resolución del problema de la aplicación. Puede usar los accesos directos de iconos que proporciona Genie para ver el informe de diagnóstico completo de la categoría del problema que le interesa. Los accesos directos de iconos proporcionan una manera directa de acceder a las métricas de diagnóstico.

![Genie](./media/functions-diagnostics/genie.png)

Después de seleccionar un icono, podrá ver una lista de temas relacionados con la incidencia descrita en el icono. En estos temas se proporcionan fragmentos de información importante del informe completo. Puede seleccionar cualquiera de estos temas para profundizar en las incidencias. Además, puede seleccionar **Ver el informe completo** para explorar todos los temas en una sola página.

![Vista previa del informe de diagnóstico](./media/functions-diagnostics/preview-of-diagnostic-report.png)

## <a name="view-a-diagnostic-report"></a>Visualización de un informe de diagnóstico

Después de elegir un tema, podrá ver un informe de diagnóstico específico de la aplicación de funciones. Los informes de diagnóstico usan iconos de estado para indicar si hay alguna incidencia específica con la aplicación. Se puede ver una descripción detallada de la incidencia, las acciones recomendadas, las métricas relacionadas y documentos útiles. Los informes de diagnóstico personalizados se generan a partir de una serie de comprobaciones que se ejecutan en la aplicación de funciones. Los informes de diagnóstico pueden ser una herramienta útil para identificar problemas en la aplicación de funciones y guiarle hacia la resolución de la incidencia.

## <a name="find-the-problem-code"></a>Búsqueda del código del problema

En el caso de las funciones basadas en scripts, se puede usar **Function Execution and Errors** (Ejecuciones y errores de funciones) de **Function App Down or Reporting Errors** (Aplicación de funciones fuera de servicio o notificación de errores) para restringir la línea de código que provoca las excepciones o los errores. Esta característica puede ser una herramienta útil para llegar a la causa principal y corregir las incidencias de una línea de código específica. Esta opción no está disponible para las funciones de C# y de Java compiladas previamente.

![Informe de diagnóstico sobre errores de ejecución de función](./media/functions-diagnostics/diagnostic-report-on-function-execution-errors.png)

![Excepción de función](./media/functions-diagnostics/function-exception.png)

## <a name="next-steps"></a>Pasos siguientes

Puede formular preguntas o enviar comentarios sobre los diagnósticos de Azure Functions en [UserVoice](https://feedback.azure.com/forums/355860-azure-functions). Incluya `[Diag]` en el título de los comentarios.

> [!div class="nextstepaction"]
> [Supervisión de la aplicación de funciones](functions-monitoring.md)
