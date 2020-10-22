---
title: Creación de seguimientos del rendimiento en el cliente
description: Procedimientos recomendados para la generación de perfiles de rendimiento del cliente con WPF
author: florianborn71
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: 1d4ce68bdda5fbc3dfdb7396141289a58dab5bd1
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92204102"
---
# <a name="create-client-side-performance-traces"></a>Creación de seguimientos del rendimiento en el cliente

Hay muchas razones por las que el rendimiento de Azure Remote Rendering puede no ser tan bueno como se esperaba. Además del rendimiento de representación puro en el servidor en la nube, la calidad de la conexión de red tiene una influencia especialmente significativa en la experiencia. Para generar perfiles del rendimiento del servidor, consulte el capítulo [Consultas de rendimiento del lado servidor](../overview/features/performance-queries.md).

Este capítulo se centra en cómo identificar los posibles cuellos de botella del cliente mediante *:::no-loc text="performance traces":::* .

## <a name="getting-started"></a>Introducción

Si no está familiarizado con la funcionalidad :::no-loc text="performance tracing"::: de Windows, en esta sección se indicarán los términos y las aplicaciones fundamentales para comenzar.

### <a name="installation"></a>Instalación

Las aplicaciones que se usan para realizar el seguimiento con ARR son herramientas de uso general que se pueden utilizar para todo el desarrollo de Windows. Se proporcionan con el [Kit de herramientas de rendimiento de Windows](/windows-hardware/test/wpt/). Para obtenerlo, descargue [Windows Assessment and Deployment Kit](/windows-hardware/get-started/adk-install).

### <a name="terminology"></a>Terminología

Cuando busca información acerca de seguimientos de rendimiento, inevitablemente se encontrará con una serie de términos. Los más importantes son los siguientes:

* `ETW`
* `ETL`
* `WPR`
* `WPA`

**ETW** son las siglas en inglés de [**E**vent **T**racing para **W**indows](/windows/win32/etw/about-event-tracing) (o Seguimiento de eventos para Windows). Simplemente es el nombre de la eficaz herramienta de seguimiento del nivel de kernel que se integra en Windows. Se denomina de seguimiento de *eventos* porque las aplicaciones que admiten ETW emiten eventos para registrar las acciones que pueden ayudar a realizar un seguimiento de los problemas de rendimiento. De forma predeterminada, el sistema operativo ya emite eventos para elementos como los accesos al disco, los modificadores de tareas y otros similares. Las aplicaciones como ARR también emiten eventos personalizados, por ejemplo, los fotogramas que se han descartado, el retraso de la red, etc.

**ETL** son las siglas en inglés de **E**vent **T**race **L**ogging (o registro del seguimiento de eventos). Simplemente significa que se ha recopilado (registrado) un seguimiento y, por lo tanto, se suele usar como extensión de archivo para los archivos que almacenan los datos de seguimiento. Así, cuando realice un seguimiento, después, normalmente, tendrá un archivo \*.etl.

**WPR** son las siglas en inglés de [**W**indows **P**erformance **R**ecorder](/windows-hardware/test/wpt/windows-performance-recorder) (o Grabador de rendimiento de Windows) y es el nombre de la aplicación que inicia y detiene la grabación de los seguimientos de evento. WPR toma un archivo de perfil (\*.wprp) que configura los eventos exactos que se van a registrar. Este tipo de archivo `wprp` se proporciona con el SDK de ARR. Al realizar seguimientos en un equipo de escritorio, puede iniciar WPR directamente. En cambio, cuando realiza un seguimiento en HoloLens, normalmente se hace mediante la interfaz web.

**WPA** son las siglas de [**W**indows **P**erformance **A**nalyzer](/windows-hardware/test/wpt/windows-performance-analyzer) (o Analizador de rendimiento de Windows) y es el nombre de la aplicación GUI que se usa para abrir archivos \*.etl y examinar los datos a fin de identificar los problemas de rendimiento. WPA le permite ordenar los datos por varios criterios, mostrarlos de varias maneras, profundizar en los detalles y correlacionar la información.

Aunque los seguimientos de ETL se pueden crear en cualquier dispositivo Windows (equipo local, HoloLens, servidor en la nube, etc.), normalmente se guardan en el disco y se analizan con WPA en un equipo de escritorio. Los archivos ETL se pueden enviar a otros desarrolladores para que los examinen. Sin embargo, tenga en cuenta que la información importante, como las rutas de acceso de los archivos y las direcciones IP, se puede capturar en los seguimientos ETL. Puede usar ETW de dos maneras: para grabar o para analizar los seguimientos. La grabación de seguimientos es sencilla y requiere una configuración mínima. Por otro lado, el análisis de los seguimientos requiere un conocimiento aceptable tanto de la herramienta WPA como del problema que se esté investigando. El material general para el aprendizaje de WPA se proporcionará a continuación, así como directrices para interpretar los seguimientos específicos de ARR.

## <a name="recording-a-trace-on-a-local-pc"></a>Grabación de un seguimiento en un equipo local

Para identificar los problemas de rendimiento de ARR, es preferible realizar un seguimiento directamente en HoloLens, ya que es la única manera de obtener una instantánea de las características reales del rendimiento. Sin embargo, si desea realizar específicamente un seguimiento sin las restricciones de rendimiento de HoloLens o si solo desea obtener información sobre cómo usar WPA y no necesita un seguimiento realista, aquí se indica cómo hacerlo.

### <a name="wpr-configuration"></a>Configuración de WPR

1. Inicie [:::no-loc text="Windows Performance Recorder":::](/windows-hardware/test/wpt/windows-performance-recorder) desde el *menú Inicio*.
1. Expanda **Más opciones**.
1. Haga clic en **Agregar perfiles...** .
1. Seleccione el archivo *AzureRemoteRenderingNetworkProfiling.wprp*. Puede encontrar este archivo en el SDK de ARR en *Tools/ETLProfiles*.
   El perfil aparecerá ahora en WPR, bajo *Custom measurements* (Medidas personalizadas). Asegúrese de que es el único perfil habilitado.
1. Expanda *First level triage* (Evaluación de prioridades del primer nivel):
    * Si solo desea capturar un seguimiento rápido de los eventos de red de ARR, **deshabilite** esta opción.
    * Si tiene que correlacionar eventos de red de ARR con otras características del sistema, como el uso de la CPU o la memoria, **habilite** esta opción.
    * Si lo hace, lo más probable es que el seguimiento tenga un tamaño de varios gigabytes y que tarde mucho tiempo en guardarse y abrirse en WPA.

Después, la configuración de WPR debe ser similar a la siguiente:

![Configuración de WPR](./media/wpr.png)

### <a name="recording"></a>Grabación

Haga clic en **Iniciar** para iniciar la grabación de un seguimiento. Puede iniciar y detener la grabación en cualquier momento; no es necesario cerrar la aplicación antes. Como puede ver, no es necesario especificar la aplicación cuyo seguimiento se va a realizar, ya que ETW registrará siempre un seguimiento para todo el sistema. El archivo `wprp` especifica qué tipos de eventos se van a registrar.

Haga clic en **Guardar** para detener la grabación y especifique dónde desea almacenar el archivo ETL.

Ahora tiene un archivo ETL que puede abrir directamente en WPA o enviarlo a otra persona.

## <a name="recording-a-trace-on-a-hololens"></a>Grabación de un seguimiento en un HoloLens

Para grabar un seguimiento en un HoloLens, inicie el dispositivo y escriba su dirección IP en un explorador para abrir el *Portal de dispositivos*.

![Portal de dispositivos](./media/wpr-hl.png)

1. A la izquierda, vaya a *Performance > Performance Tracing* (Rendimiento > Seguimiento de rendimiento).
1. Seleccione **Custom profiles** (Perfiles personalizados).
1. Haga clic en **:::no-loc text="Browse...":::**
1. Seleccione el archivo *AzureRemoteRenderingNetworkProfiling.wprp*. Puede encontrar este archivo en el SDK de ARR en *Tools/ETLProfiles*.
1. Haga clic en **Start Trace** (iniciar seguimiento).
1. Ahora HoloLens está registrando un seguimiento. Asegúrese de desencadenar los problemas de rendimiento que desee investigar. Luego, haga clic en **Stop Trace** (Detener seguimiento).
1. El seguimiento se mostrará en la parte inferior de la página web. Haga clic en el icono de disco en el lado derecho para descargar el archivo ETL.

Ahora tiene un archivo ETL que puede abrir directamente en WPA o enviarlo a otra persona.

## <a name="analyzing-traces-with-wpa"></a>Análisis de seguimientos con WPA

### <a name="wpa-basics"></a>Aspectos básicos de WPA

El Analizador de rendimiento de Windows es la herramienta estándar para abrir los archivos ETL e inspeccionar los seguimientos. La explicación de cómo funciona WPA está fuera del ámbito de este artículo. Para empezar, consulte estos recursos:

* Vea los [vídeos introductorios](/windows-hardware/test/wpt/windows-performance-analyzer) para obtener una primera información general.
* El propio WPA tiene una pestaña *Getting Started* (Introducción), en la que se explican los pasos habituales. Vea los temas disponibles. Especialmente, en "View Data" (Ver datos), obtiene una introducción rápida sobre cómo crear grafos para datos específicos.
* Hay información excelente [en este sitio web](https://randomascii.wordpress.com/2015/09/24/etw-central/), sin embargo, no toda es pertinente para principiantes.

### <a name="graphing-data"></a>Inclusión de datos en grafos

Para empezar a trabajar con el seguimiento de ARR, es conveniente conocer los siguientes elementos.

![Grafo de rendimiento](./media/wpa-graph.png)

La imagen anterior muestra una tabla de datos de seguimiento y una representación gráfica de esos mismos datos.

En la tabla de la parte inferior, observe la barra amarilla (dorada) y la barra azul. Puede arrastrarlas y colocarlas en cualquier posición.

Todas las **columnas situadas a la izquierda de la barra amarilla** se interpretan como **claves**. Las claves se utilizan para estructurar el árbol en la ventana superior izquierda. Aquí tenemos dos columnas *clave*, "Provider name" (Nombre de proveedor) y "Task name" (Nombre de tarea). Por lo tanto, la estructura de árbol de la ventana superior izquierda tiene dos niveles de profundidad. Si se reordenan las columnas o se agregan o quitan columnas del área de claves, la estructura de la vista de árbol cambia.

**Las columnas situadas a la derecha de la barra azul** se usan para la **presentación del grafo** en la ventana superior derecha. La mayoría de las veces, solo se usa la primera columna, pero algunos modos de grafo requieren varias columnas de datos. Para que los grafos de líneas funcionen, debe establecerse el *modo de agregación* en esa columna. Use "Avg" o "Max". El modo de agregación se usa para determinar el valor del grafo en un píxel determinado, cuando un píxel cubre un intervalo con varios eventos. Para observarlo, establezca la agregación en "Sum" y, a continuación, acerque y aleje el grafo.

Las columnas del centro no tienen ningún significado especial.

![Vista de eventos](./media/wpa-event-view.png)

En el *editor de la vista de eventos genéricos* puede configurar todas las columnas que se van a mostrar, el modo de agregación, la ordenación y qué columnas se usan como claves o para grafos. En el ejemplo anterior, el **campo 2** está habilitado y los campos 3 a 6 están deshabilitados. El campo 2 suele ser el primero de *datos personalizados* de un evento ETW y, por tanto, para los eventos ARR "FrameStatistics", que representan algún valor de latencia de red. Habilite otras columnas "Campo" para ver más valores de este evento.

### <a name="presets"></a>Valores preestablecidos

Para analizar correctamente un seguimiento, tendrá que establecer su propio flujo de trabajo y la presentación de datos que prefiera. Sin embargo, para poder obtener una rápida información general de los eventos específicos de ARR, se incluyen archivos de perfil y valores preestablecidos de la Plataforma de protección de software de Windows en la carpeta *Tools/ETLProfiles*. Para cargar un perfil completo, seleccione *Perfiles > Aplicar...* en la barra de menús de WPA o abra el panel *My Presets* (Mis valores preestablecidos) (*Window > My Presets*) (Ventana > Mis valores preestablecidos) y seleccione *Importar*. En el primer caso, se establecerá una configuración de WPA completa como en la imagen siguiente. El último caso solo hará que estén disponibles los valores preestablecidos para las distintas configuraciones de vista y le permitirá abrir rápidamente una vista para ver una parte específica de los datos de eventos de ARR.

![Valores preestablecidos](./media/wpa-arr-trace.png)

La imagen anterior muestra vistas de varios eventos específicos de ARR más una vista del uso total de la CPU.

## <a name="next-steps"></a>Pasos siguientes

* [Consultas de rendimiento en el lado servidor](../overview/features/performance-queries.md)