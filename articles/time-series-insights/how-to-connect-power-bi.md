---
title: 'Conexión de un entorno a Power BI: Azure Time Series Insights | Microsoft Docs'
description: Aprenda a conectar Azure Time Series Insights a Power BI para compartir, representar y mostrar datos en toda una organización.
author: deepakpalled
ms.author: dpalled
manager: cshankar
services: time-series-insights
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 22053bdc3a9836b76aa92303234a095cac6448ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75863849"
---
# <a name="visualize-data-from-time-series-insights-in-power-bi"></a>Visualización de datos de Time Series Insights en Power BI

Azure Time Series Insights es una plataforma para almacenar, administrar, consultar y visualizar datos de series temporales en la nube. [Power BI](https://powerbi.microsoft.com) es una herramienta de análisis de negocio con funcionalidades de visualización enriquecidas que permiten compartir información y resultados en toda la organización. Ahora se pueden integrar ambos servicios para obtener lo mejor tanto de las funcionalidades de visualización inherentes de Time Series Insights como de Power BI.

Aprenderá a:

* Conectar Time Series Insights a Power BI mediante el conector de nube
* Crear objetos visuales con los datos en Power BI
* Publicar el informe en Power BI y compartirlo con el resto de la organización

Al final, aprenderá a visualizar datos de series temporales mediante Azure Time Series Insights y a mejorarlos con las potentes funcionalidades de visualización de datos y de fácil uso compartido de Power BI.

Asegúrese de que se registra para obtener una [suscripción gratuita a Azure](https://azure.microsoft.com/free/), en caso de que no tenga ninguna.

## <a name="prerequisites"></a>Prerequisites

* Descargar e instalar la versión más reciente de [Power BI Desktop](https://powerbi.microsoft.com/downloads/)
* Disposición o creación de una [instancia de Azure Time Series Insights (versión preliminar)](time-series-insights-update-how-to-manage.md)

> [!IMPORTANT]
> El conector de Power BI se admite actualmente en los entornos de *pago por uso* de la versión preliminar de Time Series Insights que se han configurado para el **almacenamiento intermedio**.

## <a name="connect-data-from-time-series-insights-to-power-bi"></a>Conexión de datos de Time Series Insights con Power BI

Para conectar un entorno de Time Series Insights a Power BI, siga estos pasos:

1. Abra Time Series Insights Explorer
1. Exporte los datos en forma de consulta o de datos sin procesar
1. Abra Power BI Desktop
1. Cargue desde una consulta personalizada

### <a name="export-data-into-power-bi-desktop"></a>Exporte los datos en Power BI Desktop

Primeros pasos:

1. Abra Time Series Insights Preview Explorer y ajuste los datos.
1. Una vez que haya creado una vista con la que esté conforme, vaya al menú desplegable **Más acciones** y seleccione **Conectar con Power BI**.

    [![Exportación desde el Explorador de versión preliminar de Time Series Insights](media/how-to-connect-power-bi/time-series-insights-export-option.png)](media/how-to-connect-power-bi/time-series-insights-export-option.png#lightbox)

1. Establezca los parámetros en esta pestaña:

   1. Especifique el periodo concreto que va a ver. Si no le gusta su vista existente, deje la opción **Periodo existente**.
   
   1. Elija entre **Agregado** y **Eventos sin formato**. 
   
       > [!NOTE]
       > Siempre puede agregar los datos posteriormente en Power BI, pero no puede realizar la reversión a datos sin formato después de la agregación. 
       
       > [!NOTE]
       > Hay un límite de recuento de eventos de 100 000 para los datos del nivel de datos sin formato.

       [![Conectar](media/how-to-connect-power-bi/connect-to-power-bi.png)](media/how-to-connect-power-bi/connect-to-power-bi.png#lightbox)

   1. Si no ha configurado su instancia de Time Series Insights para el **almacenamiento intermedio**, recibirá una advertencia.

       [![Advertencia de almacenamiento intermedio](media/how-to-connect-power-bi/connect-to-power-bi-warning.png)](media/how-to-connect-power-bi/connect-to-power-bi-warning.png#lightbox)

       > [!TIP]
       > Puede configurar la instancia existente para el **almacenamiento intermedio** en Azure Portal.

1. Seleccione **Copiar consulta en el Portapapeles**.
1. Ahora, inicie Power BI Desktop.
1. En Power BI Desktop, en la pestaña **Inicio**, seleccione **Obtener datos** en la esquina superior izquierda y luego **Más**.

    [![Menú desplegable de inicio](media/how-to-connect-power-bi/power-bi-home-drop-down.png)](media/how-to-connect-power-bi/power-bi-home-drop-down.png#lightbox)

1. Busque **Time Series Insights**, seleccione **Azure Time Series Insights (Beta)** y, después, **Conectar**.

    [![Conectar Power BI a Time Series Insights](media/how-to-connect-power-bi/connect-to-time-series-insights.png)](media/how-to-connect-power-bi/connect-to-time-series-insights.png#lightbox)

    También puede ir a la pestaña **Azure**, seleccionar **Azure Time Series Insights (Beta)** y, después, **Conectar**.
    
1. Se mostrará un cuadro de diálogo en el que se le pedirá permiso para conectarse a recursos de terceros. Seleccione **Continuar**.

    [![Elegir Creación de una consulta personalizada](media/how-to-connect-power-bi/confirm-the-connection.png)](media/how-to-connect-power-bi/confirm-the-connection.png#lightbox)

1. En el menú desplegable que hay en **Origen de datos**, elija **Create custom query** (Crear consulta personalizada). Pegue desde el portapapeles en el campo **Consulta personalizada (opcional)** siguiente y, después, presione **Aceptar**.

    [![Pasar la consulta personalizada y seleccionar Aceptar](media/how-to-connect-power-bi/custom-query-load.png)](media/how-to-connect-power-bi/custom-query-load.png#lightbox)  

1. Se cargará la tabla de datos. Pulse **Cargar** para realizar la carga en Power BI.

    [![Examinar los datos cargados en la tabla y seleccionar Cargar](media/how-to-connect-power-bi/review-the-loaded-data-table.png)](media/how-to-connect-power-bi/review-the-loaded-data-table.png#lightbox)  

Si ha completado estos pasos, vaya a la sección siguiente.

## <a name="create-a-report-with-visuals"></a>Crear un informe con objetos visuales

Ahora que ha importado los datos en Power BI, es el momento de crear un informe con objetos visuales.

1. En el lado izquierdo de la ventana, asegúrese de que ha seleccionado la vista **Informe**.

    [![Seleccionar la vista Informe](media/how-to-connect-power-bi/select-the-report-view.png)](media/how-to-connect-power-bi/select-the-report-view.png#lightbox)

1.  En la columna **Visualizaciones**, seleccione el objeto visual que prefiera. Por ejemplo, seleccione **Gráfico de líneas**. Se agregará un gráfico de líneas en blanco al lienzo.
 
1.  En la lista **Campos**, seleccione **Marca de tiempo** y arrástrelo hasta el campo **Eje** para mostrar los elementos en el eje X.

1.  En la lista **Campos**, seleccione **TimeSeriesId** y arrástrelo hasta el campo **Valores** para mostrar los elementos en el eje Y.

    [![Crear un gráfico de líneas](media/how-to-connect-power-bi/power-bi-line-chart.png)](media/how-to-connect-power-bi/power-bi-line-chart.png#lightbox)

1.  Para agregar otro gráfico al lienzo, seleccione cualquier lugar del lienzo fuera del gráfico de líneas y repita este proceso.

    [![Crear gráficos adicionales para compartir](media/how-to-connect-power-bi/power-bi-additional-charts.png)](media/how-to-connect-power-bi/power-bi-additional-charts.png#lightbox)

Una vez creado el informe, puede publicarlo en Reporting Services de Power BI.

## <a name="advanced-editing"></a>Edición avanzada

Si ya ha cargado un conjunto de datos en Power BI pero desea modificar la consulta (por ejemplo, los parámetros de fecha y hora o del identificador del entorno), puede hacerlo a través de la funcionalidad Editor avanzado de Power BI. Para más información, consulte la [documentación de Power BI](https://docs.microsoft.com/power-bi/desktop-query-overview).

A modo de información general:

1. En Power BI Desktop, seleccione **Editar consultas**.
1. Presione **Editor avanzado**.

    [![Editar consultas en el Editor avanzado](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png)](media/how-to-connect-power-bi/power-bi-advanced-query-editing.png#lightbox)

1. Modifique la carga de JSON según sea necesario.
1. Seleccione **Listo** y, después, **Cerrar y aplicar** en la **ventana del editor Power Query**.

Ahora, en la interfaz se reflejarán los cambios que haya aplicado.  

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de los [conceptos de Power BI Connector](https://docs.microsoft.com/power-bi/desktop-query-overview) para Azure Time Series Insights.

* Más información acerca de [Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-query-overview).

* Vea [Explorador de disponibilidad general de Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart) y [Explorador de la versión preliminar de Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart).
