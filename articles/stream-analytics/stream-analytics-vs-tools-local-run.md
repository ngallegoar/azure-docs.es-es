---
title: Prueba de las consultas de Azure Stream Analytics localmente en Visual Studio
description: En este artículo se describe cómo probar las consultas localmente con las herramientas de Azure Stream Analytics para Visual Studio.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 34c8555356d5c0142d7b677c8119fe66806ab064
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76834916"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio"></a>Prueba de las consultas de Stream Analytics localmente con Visual Studio

Puede usar las herramientas de Azure Stream Analytics para Visual Studio para probar los trabajos de Stream Analytics localmente con datos de ejemplo o [datos activos](stream-analytics-live-data-local-testing.md). 

Use este [Inicio rápido](stream-analytics-quick-create-vs.md) para aprender a crear un trabajo de Stream Analytics mediante Visual Studio.

## <a name="test-your-query"></a>Prueba de la consulta

En el proyecto de Azure Stream Analytics, haga doble clic en **Script.asaql** para abrir el script en el editor. Puede compilar la consulta para ver si hay algún error de sintaxis. El editor de consultas admite IntelliSense, colores de sintaxis y marcador de errores.

![Editor de consultas](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="add-local-input"></a>Agregar entrada local

Para validar la consulta con datos estáticos locales, haga clic con el botón derecho en la entrada y seleccione **Add local input** (Agregar entrada local).
   
![Agregar entrada local](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-01.png)
   
En la ventana emergente, seleccione los datos de ejemplo de la ruta de acceso local y haga clic en **Guardar**.
   
![Agregar entrada local](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-02.png)
   
Un archivo denominado **local_EntryStream.json** se agrega automáticamente a la carpeta de entradas.
   
![Lista de archivos de la carpeta de entrada local](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-03.png)
   
En el editor de consultas, haga clic en **Ejecutar localmente**. O bien, puede presionar F5.
   
![Ejecución en modo local](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-01.png)
   
La salida se puede ver en un formato de tabla directamente desde Visual Studio.

![Salida en formato de tabla](./media/stream-analytics-vs-tools-local-run/stream-analytics-for-vs-local-result.png)

Puede encontrar la ruta de acceso de salida en la salida de la consola. Presione cualquier tecla para abrir la carpeta de resultados.
   
![Ejecución local](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-02.png)
   
Compruebe los resultados en la carpeta local.
   
![Resultado de la carpeta local](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-03.png)
   

### <a name="sample-input"></a>Entrada de ejemplo
También puede recopilar datos de entrada de ejemplo de orígenes de entrada a un archivo local. Haga clic con el botón derecho en el archivo de configuración de entrada y seleccione **Datos de ejemplo**. 

![Datos de ejemplo](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-01.png)

Solo se pueden muestrear los datos que se transmiten desde Event Hubs o IoT Hubs. No se admiten otros orígenes de entrada. En el cuadro de diálogo emergente, escriba la ruta de acceso local para guardar los datos de ejemplo y seleccione **Muestra**.

![Configuración de los datos de ejemplo](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-02.png)
 
Puede ver el progreso en la ventana **Salida**. 

![Salida de los datos de ejemplo](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-03.png)

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: creación de un trabajo de Stream Analytics con Visual Studio](stream-analytics-quick-create-vs.md)
* [Uso de Visual Studio para ver trabajos de Azure Stream Analytics](stream-analytics-vs-tools.md)
* [Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio (versión preliminar)](stream-analytics-live-data-local-testing.md)
* [Tutorial: Implementación de un trabajo de Azure Stream Analytics con CI/CD mediante Azure DevOps](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Integración y desarrollo continuos con las herramientas de Stream Analytics](stream-analytics-tools-for-visual-studio-cicd.md)
