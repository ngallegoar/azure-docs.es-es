---
title: Detección de fraudes en tiempo real con Azure Stream Analytics
description: Aprenda a crear una solución para la detección de fraudes en tiempo real con Stream Analytics. Use un centro de eventos para el procesamiento de eventos en tiempo real.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 03/24/2020
ms.custom: seodec18
ms.openlocfilehash: ba216e41672e1d19e552b3f82a2ea65da7d3a435
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96007093"
---
# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Introducción al uso de Azure Stream Analytics: Detección de fraudes en tiempo real

Este tutorial muestra de principio a fin cómo usar Azure Stream Analytics. Aprenderá a: 

* Incorporar eventos de streaming a una instancia de Azure Event Hubs. En este tutorial, usará una aplicación que simula un flujo de registros de metadatos de teléfono móvil.

* Escribir consultas de Stream Analytics de tipo SQL para transformar datos, agregar información o buscar patrones. Verá cómo usar una consulta para examinar el flujo entrante y buscar llamadas que puedan ser fraudulentas.

* Enviar los resultados a un receptor de salida (almacenamiento) que pueda analizar para obtener información adicional. En este caso, enviará los datos de llamadas sospechosas a Azure Blob Storage.

En este tutorial se usa el ejemplo de detección de fraudes en tiempo real basado en datos de llamadas telefónicas. La técnica que se muestra también es adecuada para la detección de otros tipos de fraudes, como el fraude con tarjetas de crédito o el robo de identidad. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Escenario: telecomunicaciones y detección de fraudes de SIM en tiempo real

Una empresa de telecomunicaciones tiene un gran volumen de datos en llamadas entrantes. La compañía quiere detectar llamadas fraudulentas en tiempo real para poder notificarlo a los clientes o cancelar el servicio para un número concreto. Un tipo de fraude de SIM implica varias llamadas desde la misma identidad aproximadamente a la misma hora, pero en distintas ubicaciones geográficas. Para detectar este tipo de fraude, la compañía ha de examinar los registros de llamadas entrantes y buscar patrones concretos (en este caso, llamadas realizadas aproximadamente a la misma hora en distintos países o regiones). Los registros de teléfono que entren en esta categoría se escriben en el almacenamiento para su análisis posterior.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial, simulará datos de llamadas telefónicas mediante una aplicación cliente que genera metadatos de llamada telefónica de muestra. Algunos de los registros que genera la aplicación parecen llamadas fraudulentas. 

Antes de empezar, asegúrese de que dispone de lo siguiente:

* Una cuenta de Azure.
* La aplicación que genera eventos de llamada, [TelcoGenerator.zip](https://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip), que puede descargarse desde el Centro de descarga de Microsoft. Descomprima este paquete un una carpeta en el equipo. Si quiere ver el código fuente y ejecutar la aplicación en un depurador, puede obtener el código fuente de la aplicación en [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Windows puede bloquear el archivo zip descargado. Si no puede descomprimirlo, haga clic con el botón derecho en el archivo y seleccione **Propiedades**. Si ve el mensaje "Este archivo proviene de otro equipo y podría bloquearse para ayudar a proteger este equipo", seleccione la opción **Desbloquear** y luego haga clic en **Aplicar**.

Si quiere examinar los resultados del trabajo de análisis de Stream Analytics, necesita además una herramienta para ver el contenido de un contenedor de Azure Blob Storage. Si utiliza Visual Studio, puede usar [Azure Tools para Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) o [Visual Studio Cloud Explorer](/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer). También puede instalar herramientas independientes, tales como [Explorador de Azure Storage](https://storageexplorer.com/) y [Cerulean](https://www.cerebrata.com/products/cerulean/features/azure-storage). 

## <a name="create-an-azure-event-hubs-to-ingest-events"></a>Creación de una instancia de Azure Event Hubs para la ingesta de eventos

Para analizar un flujo de datos, debe *introducirlo* en Azure. Una forma habitual de ingesta de datos consiste en usar [Azure Event Hubs](../event-hubs/event-hubs-about.md), que permite la ingesta de millones de eventos por segundo y el procesamiento y almacenamiento de la información de los eventos. En este tutorial, creará un centro de eventos e indicará a la aplicación que genera eventos de llamada que envíe los datos de las llamadas a dicho centro.

>[!NOTE]
>Para obtener una versión más detallada de este procedimiento, vea [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>Creación de un espacio de nombres y un centro de eventos
En este procedimiento, cree primero un espacio de nombres del centro de eventos y agregue luego un centro de eventos a dicho espacio de nombres. Los espacios de nombres del centro de eventos se usan para agrupar lógicamente instancias de bus de eventos relacionados. 

1. Inicie sesión en Azure Portal y haga clic en **Crear un recurso** en la parte superior izquierda de la pantalla.

2. Seleccione **Todos los servicios** en el menú de la izquierda y seleccione el **asterisco (`*`)** junto a **Event Hubs** en la categoría **Análisis**. Confirme que **Event Hubs** se agrega a **FAVORITOS** en el menú de navegación de la izquierda. 

   ![Búsqueda de instancias de Event Hubs](./media/stream-analytics-real-time-fraud-detection/select-event-hubs-menu.png)

3. Seleccione **Event Hubs** en **FAVORITOS** en el menú de navegación de la izquierda y seleccione **Agregar** en la barra de herramientas.

   ![Botón Agregar](./media/stream-analytics-real-time-fraud-detection/event-hubs-add-toolbar.png)

4. En el panel **Crear espacio de nombres**, especifique un nombre de espacio de nombres, como `<yourname>-eh-ns-demo`. Puede utilizar cualquier nombre para el espacio de nombres, pero debe ser válido para una dirección URL y ser único en Azure. 
    
5. Seleccione una suscripción y cree o elija un grupo de recursos. Luego haga clic en **Crear**.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png" alt="Create event hub namespace in Azure portal" width="300px"/>

6. Cuando el espacio de nombres termine de implementarse, busque el espacio de nombres del centro de eventos en la lista de recursos de Azure. 

7. Haga clic en el nuevo espacio de nombres y, en el panel del espacio de nombres, haga clic en **Event Hub**.

   ![Botón Agregar centro de eventos para crear un centro de eventos](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
8. Asigne al nuevo centro de eventos el nombre `asa-eh-frauddetection-demo`. Puede usar otro nombre. Si lo hace, tome nota de él, porque más adelante se necesita el nombre. No es preciso establecer otras opciones del centro de eventos ahora mismo.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png" alt="Name event hub in Azure portal" width="400px"/>
    
9. Haga clic en **Crear**.

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Concesión de acceso al centro de eventos y obtención de una cadena de conexión

Para que un proceso pueda enviar datos a un centro de eventos, este debe tener una directiva que permita el acceso adecuado. La directiva de acceso genera una cadena de conexión que incluye la información de autorización.

1. En el panel del espacio de nombres, haga clic en **Event Hubs** y luego en el nombre del nuevo centro de eventos.

2. En el panel de Event Hub, haga clic en **Directivas de acceso compartido** y luego en **+&nbsp;Agregar**.

    > [!NOTE]
    > Asegúrese de que trabaja en el centro de eventos, no en el espacio de nombres del centro de eventos.

3. Agregue una directiva denominada `asa-policy-manage-demo` y, en **Notificación**, seleccione **Administrar**.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png" alt="Create shared access policy for Stream Analytics" width="300px"/>
 
4. Haga clic en **Crear**.

5. Una vez implementada la directiva, haga clic en ella en la lista de directivas de acceso compartido.

6. Busque el cuadro **CADENA DE CONEXIÓN: CLAVE PRINCIPAL** y haga clic en el botón Copiar que se encuentra junto a la cadena de conexión. 

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png" alt="Stream Analytics shared access policy" width="300px"/>
 
7. Pegue la cadena de conexión en un editor de texto. Necesita esta cadena de conexión en la siguiente sección, después de hacerle pequeñas algunas modificaciones.

    La cadena de conexión tiene el siguiente aspecto:

    `Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=asa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=asa-eh-frauddetection-demo`

    Observe que la cadena de conexión contiene varios pares clave-valor, separados por punto y coma: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey` y `EntityPath`.  


## <a name="configure-and-start-the-event-generator-application"></a>Configuración e inicio de la aplicación del generador de eventos

Antes de iniciar la aplicación TelcoGenerator, es preciso configurarla para que envíe los registros de las llamadas al centro de eventos que ha creado.

### <a name="configure-the-telcogenerator-app"></a>Configuración de la aplicación TelcoGenerator

1. En el editor en el que copió la cadena de conexión, apunte el valor `EntityPath` y luego quite el par `EntityPath` (no olvide quitar el punto y coma que lo precede). 

2. En la carpeta donde descomprimió el archivo TelcoGenerator.zip, abra el archivo telcodatagen.exe.config en un editor. (Hay más de un archivo .config, así que asegúrese de abrir el correcto).

3. En el elemento `<appSettings>`:

   * Establezca el valor de la clave `EventHubName` en el nombre del centro de eventos (es decir, en el valor de la ruta de acceso de la entidad).
   * Establezca el valor de la clave `Microsoft.ServiceBus.ConnectionString` en la cadena de conexión. 

   La sección `<appSettings>` tendrá un aspecto similar al del ejemplo siguiente:

    ```xml
    <appSettings>
     <!-- Service Bus specific app setings for messaging connections -->
     <add key="EventHubName" value="asa-eh-ns-demo"/>
     <add key="Microsoft.ServiceBus.ConnectionString" value="Endpoint=sb://asa-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=asa-policy-manage-demo;SharedAccessKey=GEcnTKf2//1MRn6SN1A2u0O76MP9pj3v0Ccyf1su4Zo="/>
   </appSettings>
    ```
 
4. Guarde el archivo. 

### <a name="start-the-app"></a>Inicio de la aplicación

1. Abra una ventana de comandos y cambie a la carpeta en la que se ha descomprimido la aplicación TelcoGenerator.

2. Escriba el comando siguiente:

    ```console
    telcodatagen.exe 1000 0.2 2
    ```

   Los parámetros son los siguientes: 

   * Número de registros CDR por hora. 
   * Probabilidad de fraude de tarjeta SIM: frecuencia, como porcentaje de todas las llamadas, con que la aplicación debe simular una llamada fraudulenta. El valor 0,2 indica que aproximadamente el 20 % de los registros de llamada tendrán un aspecto fraudulento.
   * Duración en horas. Número de horas que debe ejecutarse la aplicación. También puede detener la aplicación en cualquier momento presionando Ctrl+C en la línea de comandos.

   Después de unos segundos, la aplicación comienza a mostrar los registros de llamada telefónica en la pantalla tal y como los envía al centro de eventos.

Estos son algunos de los principales campos que se van a usar en esta aplicación de detección de fraudes en tiempo real:

|**Registro**|**Definición**|
|----------|--------------|
|`CallrecTime`|Marca de tiempo para la hora de inicio de la llamada. |
|`SwitchNum`|Conmutador de teléfono que se usa para conectar la llamada. En este ejemplo, los conmutadores son cadenas que representan el país o la región de origen (Estados Unidos, China, Reino Unido, Alemania o Australia). |
|`CallingNum`|Número de teléfono del autor de la llamada. |
|`CallingIMSI`|Identidad del suscriptor móvil internacional (IMSI). Se trata del identificador único del autor de la llamada. |
|`CalledNum`|Número de teléfono del destinatario de la llamada. |
|`CalledIMSI`|Identidad del suscriptor móvil internacional (IMSI). Se trata del identificador único del destinatario de la llamada. |


## <a name="create-a-stream-analytics-job-to-manage-streaming-data"></a>Creación de un trabajo de Stream Analytics para administrar los datos de streaming

Ahora que tiene un flujo de eventos de llamada, puede configurar un trabajo de Stream Analytics. El trabajo leerá los datos del centro de eventos que configuró. 

### <a name="create-the-job"></a>Creación del trabajo 

1. En Azure Portal, haga clic en **Crear un recurso** > **Internet de las cosas** > **Trabajo de Stream Analytics**.

2. Asigne al trabajo el nombre `asa_frauddetection_job_demo` y especifique una suscripción, un grupo de recursos y una ubicación.

    Es conveniente colocar el trabajo y el centro de eventos en la misma región para obtener el mejor rendimiento y no pagar por transferir datos de una región a otra.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png" alt="Create Stream Analytics job in portal" width="300px"/>

3. Haga clic en **Crear**.

    El trabajo se crea y el portal muestra los detalles del trabajo. Pero todavía no se ejecuta nada, hay que configurar el trabajo para poder iniciarlo.

### <a name="configure-job-input"></a>Configuración de la entrada del trabajo

1. En el panel de información o en el panel **Todos los recursos**, busque el trabajo de Stream Analytics `asa_frauddetection_job_demo` y selecciónelo. 
2. En la sección **Información general** del panel del trabajo de Stream Analytics, haga clic en el cuadro **Input** (Entradas).

   ![Cuadro Entradas en Topología en el panel del trabajo de Stream Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Haga clic en **Add stream input** (Agregar entrada de flujo) y seleccione **Event Hub** (Centro de eventos). Luego, especifique la siguiente información en la página New input (Entrada nueva):

   |**Configuración**  |**Valor sugerido**  |**Descripción**  |
   |---------|---------|---------|
   |Alias de entrada  |  CallStream   |  Escriba un nombre para identificar la entrada del trabajo.   |
   |Suscripción   |  \<Your subscription\> |  Seleccione la suscripción de Azure que tenga el centro de eventos que ha creado.   |
   |Espacio de nombres del centro de eventos  |  asa-eh-ns-demo |  Escriba el nombre del espacio de nombres del centro de eventos.   |
   |Nombre del centro de eventos  | asa-eh-frauddetection-demo | Seleccione el nombre del centro de eventos.   |
   |Nombre de la directiva del centro de eventos  | asa-policy-manage-demo | Seleccione la directiva de acceso que ha creado.   |

    </br>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png" alt="Create Stream Analytics input in portal" width="300px"/>


4. Haga clic en **Crear**.

## <a name="create-queries-to-transform-real-time-data"></a>Creación de consultas para transformar datos en tiempo real

En este momento, tiene un trabajo de Stream Analytics configurado para leer un flujo de datos entrantes. El siguiente paso consiste en crear una consulta que analice los datos en tiempo real. Stream Analytics admite un modelo de consulta declarativo sencillo que describe las transformaciones para el procesamiento en tiempo real. Las consultas usan un lenguaje de tipo SQL que tiene algunas extensiones específicas para Stream Analytics. 

Una consulta sencilla bastaría para leer todos los datos entrantes. Pero, a menudo se crean consultas que buscan datos concretos o relaciones en los datos. En esta sección del tutorial, crea y prueba varias consultas para aprender algunas formas en las que puede transformar un flujo de entrada para su análisis. 

Las consultas que se creen aquí solo mostrarán los datos transformados en la pantalla. En una sección posterior, configurará un receptor de salida y una consulta que escriba los datos transformados en ese receptor.

Para más información sobre el lenguaje, consulte la [Referencia de lenguaje de consulta de Azure Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference).

### <a name="get-sample-data-for-testing-queries"></a>Obtención de datos de muestra para probar las consultas

La aplicación TelcoGenerator envía registros de llamada al centro de eventos y el trabajo de Stream Analytics se configura para leer los datos del centro de eventos. Puede unas una consulta para probar el trabajo y asegurarse de que lee correctamente. Para probar una consulta en la consola de Azure, necesita datos de muestra. En este tutorial, podrá extraer datos de muestra del flujo que entra en el centro de eventos.

1. Asegúrese de que la aplicación TelcoGenerator se ejecuta y genera registros de llamada.
2. En el portal, vuelva al panel del trabajo de Stream Analytics. (Si ha cerrado el panel, busque `asa_frauddetection_job_demo` en el panel **Todos los recursos**).
3. Haga clic en el cuadro **Consulta**. Azure muestra las entradas y salidas que están configuradas para el trabajo y le permite crear una consulta que permite transformar el flujo de entrada a medida que se envía a la salida.
4. En el panel **Consulta**, haga clic en los puntos que aparecen junto a la entrada `CallStream` y seleccione **Datos de ejemplo de la entrada**.

   ![Opciones de menú para usar datos de muestra en la entrada del trabajo de Stream Analytics, con la opción "Datos de ejemplo de la entrada" seleccionada](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)


5. Establezca **Minutos** en 3 y haga clic en **Aceptar**. 
    
   ![Opciones de muestreo del flujo de entrada con la opción de 3 minutos seleccionada](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure muestrea los datos del flujo de entrada correspondientes a tres minutos y le avisa cuando los datos de muestra están listos. (Tardará unos minutos). 

Los datos de muestra se almacenan temporalmente y están disponibles mientras tenga abierta la ventana de consulta. Si cierra la ventana de consulta, los datos de muestra se descartan y tendrá que crear un nuevo conjunto de datos de muestra. 

También puede obtener un archivo .json que contiene datos de ejemplo [en GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) y luego cargar ese archivo .json para usarlo como datos de muestra para la entrada de `CallStream`. 

### <a name="test-using-a-pass-through-query"></a>Prueba con una consulta de paso a través

Si quiere archivar todos los eventos, puede usar una consulta de paso a través para leer todos los campos de la carga útil del evento.

1. En la ventana Consulta, escriba la consulta siguiente:
        
   ```SQL
   SELECT 
       *
   FROM 
       CallStream
   ```

    >[!NOTE]
    >Al igual que con SQL, las palabras clave no distinguen entre mayúsculas y minúsculas y un espacio en blanco no es significativo.

    En esta consulta, `CallStream` es el alias que especificó al crear la entrada. Si ha usado otro alias, use ese nombre en su lugar.

2. Haga clic en **Probar**.

    El trabajo de Stream Analytics ejecuta la consulta en los datos de muestra y presenta la salida en la parte inferior de la ventana. El conjunto de resultados indica que tanto el centro de eventos como el trabajo de Stream Analytics están configurados correctamente. (Como ya se ha señalado, más adelante creará un receptor de salida en el que la consulta pueda escribir los datos).

   ![Salida del trabajo de Stream Analytics que muestra 73 registros generados](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    El número exacto de registros que aparecen variará en función de los registros que se hayan capturado durante el muestreo de tres minutos.
 
### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Reducción del número de campos mediante una proyección de columna

En muchos casos, el análisis no necesita todas las columnas del flujo de entrada. Puede usar una consulta que proyecte un conjunto de campos devueltos más pequeño que la consulta de paso a través.

1. Cambie la consulta en el editor de código a:

    ```SQL
    SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
    FROM 
        CallStream
    ```

2. Haga clic de nuevo en **Probar**. 

   ![Salida del trabajo de Stream Analytics correspondiente a la proyección que muestra 25 registros](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Recuento de llamadas entrantes por región: Ventana de saltos de tamaño constante con agregación

Suponga que quiere saber el número de llamadas entrantes por región. En los datos de streaming, si se quiere ejecutar funciones de agregado como el recuento, hay que segmentar el flujo en unidades temporales (ya que el flujo en sí es de hecho ilimitado). Para ello, use una [función de ventana](stream-analytics-window-functions.md) de Stream Analytics. Luego puede trabajar con los datos contenidos en esa ventana como unidad.

Para esta transformación, se busca una secuencia de ventanas temporales que no se superpongan; cada ventana tendrá un conjunto discreto de datos que se pueden agrupar y agregar. Este tipo de ventana se denomina *ventana de saltos de tamaño constante*. En la ventana de saltos de tamaño constante, puede ver el número de llamadas entrantes agrupadas por `SwitchNum`, que representa el país o la región en los que se realizó la llamada. 

1. Cambie la consulta en el editor de código a:

    ```SQL
    SELECT 
        System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
    FROM
        CallStream TIMESTAMP BY CallRecTime 
    GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum
    ```

    En esta consulta se usa la palabra clave `Timestamp By` en la cláusula `FROM` para especificar el campo de marca de tiempo del flujo de entrada que se usa para definir la ventana de saltos de tamaño constante. En este caso, la ventana divide los datos en segmentos por el campo `CallRecTime` de cada registro. Si no se ha especificado este campo, la operación de ventana usa la hora en la que cada evento llega al centro de eventos. Vea "Tiempo de llegada frente a tiempo de aplicación" en [Referencia de lenguaje de consulta de Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference). 

    La proyección incluye `System.Timestamp`, que devuelve una marca de tiempo para el final de cada ventana. 

    Para especificar que desea usar una ventana de saltos de tamaño constante, use la función [TUMBLINGWINDOW](/stream-analytics-query/tumbling-window-azure-stream-analytics) en la cláusula `GROUP BY`. En la función, especifique una unidad de tiempo (entre un microsegundo y un día) y un tamaño de ventana (el número de unidades). En este ejemplo, la ventana de saltos de tamaño constante se compone de intervalos de cinco segundos, por lo que se obtendrá un recuento por país o región de las llamadas correspondientes a cada cinco segundos.

2. Haga clic de nuevo en **Probar**. En los resultados, observe que las marcas de tiempo que aparecen en **WindowEnd** reflejan incrementos de cinco segundos.

   ![Salida del trabajo de Stream Analytics correspondiente a la agregación que muestra 13 registros](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Detección de fraudes de SIM mediante una autocombinación

Para este ejemplo, se considera uso fraudulento aquellas llamadas cuyo origen sea un solo usuario, pero que se realicen en ubicaciones diferentes en intervalos inferiores a cinco segundos entre una y otra. Por ejemplo, el mismo usuario no puede legítimamente realizar una llamada desde Estados Unidos y Australia al mismo tiempo. 

Para comprobar estos casos, puede usar una autocombinación de los datos de streaming que combine el flujo consigo mismo en función del valor `CallRecTime`. Luego puede buscar los registros de llamada en los que el valor `CallingIMSI` (el número donde se origina) sea el mismo, pero el valor `SwitchNum` (país o región de origen) no lo sea.

Cuando use una combinación con datos de streaming, la combinación debe especificar algunos límites en la diferencia de tiempo que puede haber entre filas coincidentes. (Como ya se ha señalado, los datos de streaming son de hecho ilimitados). Los límites de tiempo de la relación se especifican en la cláusula `ON` de la combinación, con la función `DATEDIFF`. En este caso, la combinación se basa en un intervalo de cinco segundos entre datos de llamada.

1. Cambie la consulta en el editor de código a: 

    ```SQL
    SELECT  System.Timestamp as Time, 
        CS1.CallingIMSI, 
        CS1.CallingNum as CallingNum1, 
        CS2.CallingNum as CallingNum2, 
        CS1.SwitchNum as Switch1, 
        CS2.SwitchNum as Switch2 
    FROM CallStream CS1 TIMESTAMP BY CallRecTime 
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
        ON CS1.CallingIMSI = CS2.CallingIMSI 
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
    WHERE CS1.SwitchNum != CS2.SwitchNum
    ```

    Esta consulta es similar a cualquier instrucción SQL JOIN, salvo por la función `DATEDIFF` en la combinación. Esta versión de `DATEDIFF` es específica de Stream Analytics y debe aparecer en la cláusula `ON...BETWEEN`. Los parámetros son una unidad de tiempo (segundos en este ejemplo) y los alias de los dos orígenes de la combinación. Esta función no es la misma que la función `DATEDIFF` de SQL estándar.

    La cláusula `WHERE` incluye la condición que marca la llamada fraudulenta: los conmutadores de origen no son iguales. 

2. Haga clic de nuevo en **Probar**. 

   ![Salida del trabajo de Stream Analytics correspondiente a la autocombinación que muestra seis registros generados](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Haga clic en **Save** (Guardar) para guardar la consulta de autocombinación como parte del trabajo de Stream Analytics. (No guarda los datos de muestra).

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png" alt="Save Stream Analytics query in portal" width="300px"/>

## <a name="create-an-output-sink-to-store-transformed-data"></a>Creación de un receptor de salida para almacenar los datos transformados

Se ha definido un flujo de eventos, una entrada de centro de eventos para la ingesta de eventos y una consulta para realizar una transformación en el flujo de datos. El último paso es definir un receptor de salida para el trabajo (es decir, un lugar en el que escribir el flujo transformado). 

Puede usar muchos recursos como receptores de salida: una base de datos de SQL Server, Table Storage, Data Lake Store, Power BI e incluso otro centro de eventos. En este tutorial, escribirá un flujo de datos en Azure Blob Storage, algo que es habitual para recopilar información de eventos para su posterior análisis, ya que acepta datos sin estructura alguna.

Si ya tiene una cuenta de Blob Storage, puede utilizarla. En este tutorial, aprenderá a crear una nueva cuenta de almacenamiento.

### <a name="create-an-azure-blob-storage-account"></a>Creación de una cuenta de Azure Blob Storage

1. En la esquina superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Almacenamiento** > **Cuenta de almacenamiento**. Rellene la página del trabajo de la cuenta de almacenamiento. En **Nombre**, escriba "asaehstorage", en **Ubicación**, escriba "Este de EE. UU." y en **Grupo de recursos**, escriba "asa-eh-ns-rg" (para mejorar el rendimiento, hospede la cuenta de almacenamiento en el mismo grupo de recursos que el trabajo de Stream Analytics). El resto de la configuración se puede dejar con sus valores predeterminados.  

   ![Creación de una cuenta de almacenamiento en Azure Portal](./media/stream-analytics-real-time-fraud-detection/stream-analytics-storage-account-create.png)

2. En Azure Portal, vuelva al panel del trabajo de Stream Analytics. (Si ha cerrado el panel, busque `asa_frauddetection_job_demo` en el panel **Todos los recursos**).

3. En la sección **Topología de trabajo**, haga clic en el cuadro **Salida**.

4. En el panel **Salidas**, haga clic en **Agregar** y, después, seleccione **Almacenamiento de blobs**. Luego, especifique la siguiente información en la página New output (Salida nueva):

   |**Configuración**  |**Valor sugerido**  |**Descripción**  |
   |---------|---------|---------|
   |Alias de salida  |  CallStream-FraudulentCalls   |  Escriba un nombre para identificar la salida del trabajo.   |
   |Suscripción   |  \<Your subscription\> |  Seleccione la suscripción de Azure que tiene la cuenta de almacenamiento que creó. La cuenta de almacenamiento puede estar en la misma suscripción, o en otra diferente. En este ejemplo se da por supuesto que ha creado la cuenta de almacenamiento en la misma suscripción. |
   |Cuenta de almacenamiento  |  asaehstorage |  Escriba el nombre de la cuenta de almacenamiento que ha creado. |
   |Contenedor  | asa-fraudulentcalls-demo | Elija Crear nuevo y escriba un nombre de contenedor. |

    <br/>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png" alt="Create blob output for Stream Analytics job" width="300px"/>
    
5. Haga clic en **Save**(Guardar). 


## <a name="start-the-streaming-analytics-job"></a>Inicio del trabajo de Stream Analytics

El trabajo ya está configurado. Ha especificado una entrada (el centro de eventos), una transformación (la consulta que busca llamadas fraudulentas) y una salida (Blob Storage). Ahora puede iniciar el trabajo. 

1. Asegúrese de que la aplicación TelcoGenerator se ejecuta.

2. En el panel del trabajo, haga clic en **Iniciar**. En el panel **Iniciar trabajo**, en Hora de inicio de salida del trabajo, seleccione **Ahora**. 

   ![Inicio del trabajo de Stream Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start.png)



## <a name="examine-the-transformed-data"></a>Examen de los datos transformados

Ya dispone de un trabajo de Stream Analytics completo. El trabajo examina un flujo de metadatos de llamada telefónica, busca llamadas telefónicas fraudulentas en tiempo real y escribe información sobre esas llamadas fraudulentas en Storage. 

Para finalizar este tutorial, es posible que quiera ver los datos que captura el trabajo de Stream Analytics. Los datos se escriben en Azure Blog Storage in fragmentos (archivos). Puede usar cualquier herramienta que lea Azure Blob Storage. Como ya se ha señalado en la sección Requisitos previos, puede usar extensiones de Azure en Visual Studio o utilizar una herramienta como [Explorador de Azure Storage](https://storageexplorer.com/) y [Cerulean](https://www.cerebrata.com/products/cerulean/features/azure-storage). 

Al examinar el contenido de un archivo en Blob Storage, se ve algo similar a esto:

   ![Azure Blob Storage con salida de Streaming Analytics](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Limpieza de recursos

Hay otros artículos que continúan con el escenario de detección de fraudes y usan los recursos que ha creado en este tutorial. Si desea continuar, vea las sugerencias que se ofrecen en **Pasos siguientes**.

Pero, si ha terminado y no necesita los recursos que ha creado, puede eliminarlos para no incurrir en cargos de Azure innecesarios. En ese caso, se recomienda hacer lo siguiente:

1. Detenga el trabajo de Stream Analytics. En la parte superior del panel **Trabajos**, haga clic en **Detener**.
2. Detenga la aplicación Telco Generator. En la ventana de comandos donde inició la aplicación, presione Ctrl+C.
3. Si ha creado una cuenta de Blob Storage exclusivamente para este tutorial, elimínela. 
4. Elimine el trabajo de Stream Analytics.
5. Elimine el centro de eventos.
6. Elimine el espacio de nombres del centro de eventos.

## <a name="get-support"></a>Obtención de soporte técnico

Para más ayuda, pruebe la [Página de preguntas y respuestas de Microsoft sobre Azure Stream Analytics](/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Pasos siguientes

Puede seguir este tutorial con el artículo siguiente:

* [Stream Analytics y Power BI: panel de análisis en tiempo real de flujo de datos](stream-analytics-power-bi-dashboard.md). En este artículo se muestra cómo enviar la salida de TelCo del trabajo de Stream Analytics a Power BI para su análisis y visualización en tiempo real.

Para más información sobre Stream Analytics en general, examine estos artículos:

* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Escalado de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Azure Stream Analytics](/rest/api/streamanalytics/)