---
title: Tutorial de diseño de Project Acoustics en Unreal
titlesuffix: Azure Cognitive Services
description: En este tutorial se describe el flujo de trabajo de diseño para Project Acoustics en Unreal y Wwise.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 817a11171c5b4b4ef205e5fbb04f9b6d6d85b248
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68854249"
---
# <a name="project-acoustics-unrealwwise-design-tutorial"></a>Tutorial de diseño de Project Acoustics en Unreal y Wwise
En este tutorial se describe la configuración de diseño y el flujo de trabajo de Project Acoustics en Unreal y Wwise.

Requisitos previos de software:
* Un proyecto de Unreal con los complementos Wwise y Unreal para Project Acoustics

Para obtener un proyecto de Unreal con Project Acoustics, puede hacer lo siguiente:
* Seguir las instrucciones de [integración de Project Acoustics con Unreal](unreal-integration.md) para agregar Project Acoustics al proyecto de Unreal
* O bien, usar el [proyecto de ejemplo de Project Acoustics](unreal-quickstart.md).

## <a name="setup-project-wide-wwise-properties"></a>Configuración de las propiedades de Wwise en el proyecto
Wwise presenta curvas globales de obstrucción y oclusión que afectan a cómo el complemento de Project Acoustics transmite el DSP de audio de Wwise.

### <a name="design-wwise-occlusion-curves"></a>Diseño de las curvas de oclusión de Wwise
Cuando Project Acoustics está activo, responde a las curvas de volumen de oclusión, filtro de paso bajo (LPF) y filtro de paso alto (HPF) establecidas en Wwise. Se recomienda establecer el tipo de curva de volumen en lineal con un valor de -100 dB para un valor de oclusión de 100.

Con esta configuración, si la simulación de Project Acoustics calcula una oclusión de-18 db, se incluirá en la siguiente curva en X = 18 y el valor de Y correspondiente será la atenuación aplicada. Para realizar la mitad de la oclusión, establezca el punto de conexión en -50 db en lugar de -100 dB, o en -200 dB para exagerar la oclusión. Puede adaptar y ajustar la curva que funcione mejor en su juego.
 
![Captura de pantalla del editor de curva de oclusión de Wwise](media/wwise-occlusion-curve.png)

### <a name="disable-wwise-obstruction-curves"></a>Deshabilitación de las curvas de obstrucción de Wwise
Las curvas de obstrucción de Wwise afectan al nivel de sonido sin efectos en aislamiento, pero Project Acoustics usa controles de diseño y simulación para aplicar las proporciones de efectos/sin efectos. Se recomienda deshabilitar la curva de volumen de obstrucción. Para diseñar el sonido con efectos, use el control de ajuste de sonido con efectos descrito más adelante.
 
Si va a usar curvas de obstrucción LPF/HPF para otros fines, asegúrese de que las haya establecido en Y= 0 cuando X=0 (es decir, no hay LPF o HPF cuando no hay obstrucción).

![Captura de pantalla del editor de curva de obstrucción de Wwise](media/wwise-obstruction-curve.png)

### <a name="design-project-acoustics-mixer-parameters"></a>Diseño de los parámetros del mezclador de Project Acoustics
Puede controlar las propiedades globales de reverberación en la pestaña del complemento mezclador del bus de Project Acoustics. Haga doble clic en "Project Acoustics Mixer (Custom)" (Mezclador de Project Acoustics [personalizado]) para abrir el panel de configuración del complemento mezclador.

También puede ver que el complemento mezclador tiene una opción de espacialización. Si prefiere usar la espacialización integrada de Project Acoustics, active la casilla "Perform Spatialization" (Realizar espacialización) y elija HRTF o panorámica. Asegúrese de deshabilitar los buses auxiliares de sonido sin efectos que haya configurado, de lo contrario escuchará la vía directa dos veces. Use las opciones "Wetness Adjust" (Ajuste de sonido con efectos) y "Reverb Time Scale Factor" (Factor de escala de tiempo de reverberación) para ejercer un control global sobre la mezcla de reverberación. Tenga en cuenta que debe reiniciar Unreal y luego volver a generar bancos de sonido antes de pulsar el botón de reproducción para seleccionar los cambios de configuración del complemento mezclador, como la casilla "Perform Spatialization" (Realizar espacialización).

![Captura de pantalla de las opciones del complemento mezclador Wwise de Project Acoustics](media/mixer-plugin-global-settings.png)

## <a name="set-project-acoustics-design-controls-in-the-wwise-actor-mixer-hierarchy"></a>Establecimiento de los controles de diseño de Project Acoustics en la jerarquía actor-mezclador de Wwise
Para controlar los parámetros de un actor-mezclador, haga doble clic en el actor-mezclador y, luego, haga clic en su pestaña del complemento mezclador. Aquí podrá cambiar los parámetros de forma independiente para cada sonido. Estos valores se combinan con los establecidos en Unreal (que se describen a continuación). Por ejemplo, si el complemento Unreal de Project Acoustics establece el ajuste en exteriores de un objeto en 0,5 y Wwise lo establece en -0,25, el ajuste en exteriores resultante aplicado a ese sonido será de 0,25.

![Captura de pantalla de la configuración por mezclador de sonido en la jerarquía actor-mezclador de Wwise](media/per-sound-mixer-settings.png)

### <a name="ensure-the-aux-bus-has-dry-send-and-output-bus-has-wet-send"></a>Asegúrese de que el bus auxiliar tenga envío sin efectos y que el bus de salida tenga envío con efectos.
Recuerde que la configuración del actor-mezclador requerido intercambia la ruta habitual de sonido sin efectos y con efectos en Wwise. Se produce la señal de reverberación en el bus de salida del actor-mezclador (establecido en el bus de Project Acoustics) y la señal sin efectos a lo largo del bus auxiliar definido por el usuario. Esta ruta es necesaria debido a las características de la API del complemento mezclador de Wwise que usa el complemento Wwise para Project Acoustics.

![Captura de pantalla del editor de Wwise que muestra las directrices de diseño de Project Acoustics](media/voice-design-guidelines.png)
 
### <a name="set-up-distance-attenuation-curves"></a>Configuración de las curvas de atenuación de distancia
Asegúrese de que las curvas de atenuación empleadas por los actores-mezcladores con Project Acoustics tengan el envío auxiliar definido por el usuario establecido en "volumen del bus de salida". Wwise realiza esta acción de forma predeterminada con las curvas de atenuación recién creadas. Si va a migrar un proyecto existente, compruebe la configuración de las curvas.

De forma predeterminada, la simulación de Project Acoustics tiene un radio de 45 metros en torno a la ubicación del jugador. Por lo general, se recomienda configurar la curva de atenuación en -200 dB en torno a esa distancia. Esta distancia no es una restricción severa. En el caso de algunos sonidos, como el de las armas, puede que prefiera un radio mayor. En tales casos, la salvedad es que solo participará la geometría situada a 45 m de la ubicación del jugador. Si el jugador está en una sala y hay un segundo origen fuera y 100 m de distancia, se ocluirá correctamente. Si la fuente está en una sala y el jugador está fuera y hay 100 m de distancia, no se ocluirá correctamente.

![Captura de pantalla de curvas de atenuación de Wwise](media/atten-curve.png)

### <a name="post-mixer-equalization"></a>Ecualización posterior al mezclador ###
 Otra cosa que puede hacer es agregar un ecualizador posterior al mezclador. Puede tratar el bus de Project Acoustics como un bus de reverberación típico (en el modo predeterminado de reverberación) y colocarle un filtro que haga la ecualización. Puede ver un ejemplo de esto en Proyecto de ejemplo de Project Acoustics en Wwise.

![Captura de pantalla del ecualizador posterior al mezclador de Wwise](media/wwise-post-mixer-eq.png)

Por ejemplo, un filtro paso alto puede ayudar a controlar los graves de grabaciones de campo cercano que producen una reverberación muy saturado y poco real. También se puede lograr más control posterior a la simulación mediante "bake" mediante el ajuste de la ecualización a través de RTPCs, lo que permite modificar el color de reverberación durante el juego.

## <a name="set-up-scene-wide-project-acoustics-properties"></a>Configuración de las propiedades de Project Acoustics en una escena

El actor del espacio acústico expone muchos controles que modifican el comportamiento del sistema y son útiles para la depuración.

![Captura de pantalla de los controles del espacio acústico de Unreal](media/acoustics-space-controls.png)

* **Acoustics Data** (Datos acústicos): a este campo se le debe asignar un recurso acústico con simulación acústica mediante "bake" del directorio Content/Acoustics. El complemento de Project Acoustics agregará automáticamente el directorio Content/Acoustics al paquete de directorios del proyecto.
* **Tile size** (Tamaño de icono): las extensiones de la región que rodea al oyente que quiere que los datos acústicos se carguen en la memoria RAM. Mientras se carguen los sondeos del oyente inmediatamente en torno al jugador, los resultados son los mismos que cargar los datos acústicos de todos los sondeos. Los mosaicos más grandes usan más memoria RAM, pero reducen la E/S de disco.
* **Auto Stream** (Secuencia automática): cuando se habilita, se cargan automáticamente nuevos mosaicos a medida que el oyente llega al borde de la región cargada. Cuando se deshabilita, deberá cargar nuevos mosaicos manualmente mediante código o planos técnicos.
* **Cache Scale** (Escala de caché): controla el tamaño de la caché usada en las consultas acústicas. Una caché más pequeña usa menos RAM, pero puede aumentar el uso de CPU con cada consulta.
* **Acoustics Enabled** (Acústica habilitada): un control de depuración que permite la alternancia rápida entre A y B de la simulación acústica. Este control se omite en las configuraciones de envío. El control es útil para descubrir si un determinado error de audio se origina en los cálculos acústicos o se debe a cualquier otro problema del proyecto de Wwise.
* **Update Distances** (Actualizar distancias): use esta opción si quiere utilizar la información acústica previa a la simulación acústica mediante "bake" con las consultas de distancia. Estas consultas son parecidas a las emisiones de rayos, pero se han calculado previamente por lo que consumen mucha menos CPU. Un ejemplo de uso es para las reflexiones discretas de la superficie más cercana al oyente. Para sacar el máximo partido de esto, deberá usar código o planos técnicos para las distancias de consulta.
* **Draw Stats** (Extraer estadísticas): aunque `stat Acoustics` de la EU puede proporcionarle información de la CPU, esta visualización de estado mostrará el archivo ACE cargado actualmente, el uso de RAM y otra información de estado en la esquina superior izquierda de la pantalla.
* **Draw Voxels** (Extraer vóxels): los vóxels próximos al oyente se superponen para mostrar la cuadrícula de vóxels usada durante la interpolación del tiempo de ejecución. Si un emisor está dentro de un vóxel de tiempo de ejecución, se producirá un error en las consultas acústicas.
* **Draw Probes** (Extraer sondeos): se muestran todos los sondeos de esta escena. Tendrán distintos colores según su estado de carga.
* **Draw Distances** (Extraer distancias): si está habilitada la actualización de distancias, se mostrará un cuadro en la superficie más cercana al oyente en direcciones cuantificadas en torno a este.

## <a name="actor-specific-acoustics-design-controls"></a>Controles de diseño acústico específicos del actor
Estos controles de diseño están destinados a un componente de audio de Unreal.

![Captura de pantalla de los controles de componentes de audio de Unreal](media/audio-component-controls.png)

* **Occlusion Multiplier** (Multiplicador de oclusión): controla el efecto de oclusión. Los valores < 1 amplifican la oclusión. Los valores < 1 la reducen.
* **Wetness Adjustment** (Ajuste de sonido con efectos): dB de reverberación adicional.
* **Decay Time Multiplier** (Multiplicador del tiempo de decadencia): controla la RT60 de manera multiplicativa, según la salida de la simulación acústica.
* **Outdoorness Adjustment** (Ajuste en exteriores): controla cómo es la reverberación en exteriores. Los valores más cercanos a 0 son más de interiores y más cercanos a 1 son más de exteriores. Este ajuste es aditivo, por lo que si se establece en -1 se aplicará en interiores; si se establece en +1 se aplicará en exteriores.
* **Transmission Db** (Db de transmisión): represente un sonido a través de la pared adicional con esta sonoridad combinada con atenuación de distancia basada en el campo visual.
* **Wet Ratio Distance Warp** (Distorsión de distancia de proporción de sonido con efectos): ajusta las características de reverberación en la fuente como si estuviera más cerca o más lejos, sin que afecte a la vía directa.
* **Reproducir en el inicio:** alterne para especificar si el sonido debe reproducirse automáticamente en el inicio de la escena. De forma predeterminada está habilitado.
* **Show Acoustic Parameters** (Mostrar parámetros acústicos): se muestra información de depuración directamente sobre el componente del juego. (solo para las configuraciones de no envío)

## <a name="blueprint-functionality"></a>Funcionalidad de plano técnico
Se puede acceder al actor del espacio acústico mediante plano técnico, que proporciona funcionalidad como carga de un mapa o modificación de la configuración mediante scripts de nivel. Aquí se proporcionan dos ejemplos.

### <a name="add-finer-grained-control-over-streaming-load"></a>Agregar un control preciso sobre el streaming de la carga
Para administrar el streaming de datos acústicos por su cuenta en lugar de realizar el streaming automáticamente según la posición del jugador, puede usar la función de plano técnico Force Load Tile (Forzar mosaico de carga):

![Captura de pantalla de las opciones de streaming de plano técnico de Unreal](media/blueprint-streaming.png)

* **Target** (Destino): el actor de AcousticsSpace.
* **Center Position** (Posición central): el centro de la región que necesita los datos cargados.
* **Unload Probes Outside Tile** (Descargar sondeos fuera de mosaico): si se activa, todos los sondeos que no están en la nueva región se descargarán desde la memoria RAM. Si no se activa, la nueva región se carga en memoria mientras se deja que los sondeos existentes se carguen también en memoria
* **Block on Completion** (Bloquear al finalizar): convierte la carga del mosaico en una operación sincrónica.

Antes de llamar a la función para forzar el icono de carga, se debe establecer el tamaño del icono. Por ejemplo, podría hacer algo parecido para cargar un archivo ACE, establecer su tamaño de icono y transmitirlo en una región:

![Captura de pantalla de las opciones de streaming de configuración de Unreal](media/streaming-setup.png)

La función del proyecto de carga de datos acústicos utilizado en este ejemplo tiene los siguientes parámetros:

* **Target** (Destino): El actor AcousticsSpace.
* **Nueva simulación acústica mediante "bake":** El recurso de datos acústicos que se va a cargar. Si se deja este valor en blanco o se establece en un valor null, se descargará la simulación acústica mediante "bake" actual sin tener que cargar una nueva.

### <a name="optionally-query-for-surface-proximity"></a>Consultar opcionalmente la proximidad de superficie
Si quiere ver cómo de cerca están las superficies en una dirección determinada en torno al oyente, puede usar la función de distancia de consulta. Esta función puede ser útil para controlar reflexiones retardadas direccionales o para otra lógica de juego basada en la proximidad de superficies. La consulta es menos costosa que una emisión de rayos porque los resultados se extraen de la tabla de búsqueda acústica.

![Captura de pantalla de ejemplo de consulta de distancia de plano técnico](media/distance-query.png)

* **Target** (Destino): el actor de AcousticsSpace.
* **Look Direction** (Dirección de aspecto): la dirección en la que se realiza la consulta, centrada en el oyente.
* **Distance** (Distancia): si la consulta se realiza correctamente, la distancia a la superficie más cercana.
* **Return Value** (Valor devuelto): valor booleano: true si la consulta se realizó correctamente; en caso contrario, false.

## <a name="next-steps"></a>Pasos siguientes
* Explore los conceptos que subyacen al [proceso de diseño](design-process.md).
* [Cree una cuenta de Azure](create-azure-account.md) para realizar una simulación acústica mediante "bake" de su propia escena.


