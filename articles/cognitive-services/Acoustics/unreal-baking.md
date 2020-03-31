---
title: Tutorial de simulación mediante "bake" de Project Acoustics con Unreal
titlesuffix: Azure Cognitive Services
description: En este documento se describe el proceso de enviar una simulación acústica mediante "bake" con la extensión del editor Unreal.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 7a868a5f9b06499e23710399733b0659d97f900d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68854900"
---
# <a name="project-acoustics-unreal-bake-tutorial"></a>Tutorial de simulación mediante "bake" de Project Acoustics con Unreal
En este documento se describe el proceso de enviar una simulación acústica mediante "bake" con la extensión del editor Unreal.

Los pasos necesarios para realizar la elaboración son cinco:

1. Crear o etiquetar la malla de navegación del jugador
2. Etiquetar la geometría de Acoustics
3. Asignar propiedades de materiales acústicos a la geometría
4. Vista previa de la ubicación de los sondeos
5. Elaboración

## <a name="open-the-project-acoustics-editor-mode"></a>Apertura del modo de editor de Project Acoustics

Importe el paquete del complemento de Project Acoustics al proyecto. Para obtener ayuda al respecto, consulte el tema [Integración con Unreal](unreal-integration.md). Cuando se integra el complemento, abra la interfaz de usuario de Acoustics; para ello, haga clic en el nuevo icono del modo Acoustics.

![Captura de pantalla de la opción de modo Acoustics del editor de Unreal](media/acoustics-mode.png)

## <a name="tag-actors-for-acoustics"></a>Etiquetado de actores para Acoustics

La pestaña de objetos es la primera pestaña que se muestra al abrir Acoustics Mode. Utilice esta pestaña para etiquetar actores en su nivel, lo que agrega las etiquetas **AcusticsGeometry** o **AcousticsNavigation** a los actores.

Seleccione uno o más objetos en World Outliner, o utilice la sección **Bulk Selection** (Selección masiva) para ayudar a seleccionar todos los objetos de una categoría específica. Una vez seleccionados los objetos, utilice la sección de **etiquetado** para aplicar la etiqueta deseada a los objetos seleccionados.

Si alguno no tiene las etiquetas **AcousticsGeometry** ni **AcousticsNavigation**, se omitirá en la simulación. Solo se admiten mallas estáticas, mallas de navegación y paisajes. El resto de elementos que se etiqueten se omitirán.

### <a name="for-reference-the-objects-tab-parts"></a>Como referencia: Partes de la pestaña Objects (Objetos)

![Captura de pantalla de la pestaña Objects (Objetos) de Acoustics en Unreal](media/unreal-objects-tab-details.png)

1. Los botones de selección de la pestaña [pestaña **Objects** (Objetos) seleccionada]. Use estos botones para desplazarse por los distintos pasos de la simulación acústica mediante "bake", de arriba a abajo.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. Selectores disponibles para los actores del nivel.
4. Al hacer clic en **Select** (Seleccionar), se seleccionarán todos los objetos del nivel que coincidan con al menos uno de los tipos de actores seleccionados.
5. Al hacer clic en **Deselect all** (Anular toda la selección), se borrará la selección actual. Esto equivale a presionar la tecla de escape.
6. Use estos botones de radio para elegir si desea aplicar la etiqueta de geometría o de navegación a los actores seleccionados.
7. Al hacer clic en **Tag** (Etiqueta), se agregará la etiqueta elegida a todos los actores seleccionados.
8. Al hacer clic en **Untag** (Quitar etiqueta), se quitará la etiqueta elegida de todos los actores seleccionados.
9. Al hacer clic en **Select Tagged** (Seleccionar etiquetados), se borrará la selección actual y se elegirán todos los actores con la etiqueta actualmente seleccionada.
10. Estas estadísticas muestran cuántos actores están etiquetados con cada tipo de etiqueta.

### <a name="tag-acoustics-occlusion-and-reflection-geometry"></a>Etiquetado de oclusión acústica y geometría de reflexión

Abra la pestaña Objects (Objetos) de la ventana de Acoustics. Marque los objetos como Acoustics Geometry (Geometría acústica) si deben bloquear, reflejar o absorber el sonido. La geometría acústica puede incluir elementos como suelo, paredes, techos, ventanas y cristales de ventana, alfombras y muebles grandes. Puede usar cualquier nivel de complejidad arbitrario para estos objetos. Dado que la escena se voxelizará antes de la simulación, las mallas muy detalladas, como árboles con muchas hojas pequeñas, no son más costosas para simular mediante "bake" que los objetos simplificados.

No incluya elementos que no afecten a la acústica, como las mallas de colisión invisibles.

La transformación de un objeto en el momento del cálculo del sondeo (a través de la pestaña Probes [Sondeo] siguiente) se corrige en los resultados de la simulación acústica mediante "bake". Si se mueve alguno de los objetos marcados en la escena se deberá rehacer el cálculo de sondeo y volver a elaborar la escena.

### <a name="create-or-tag-a-navigation-mesh"></a>Creación o etiquetado de una malla de navegación

Una malla de navegación se usa para colocar puntos de sondeo para la simulación. Puede usar el [volumen de límites de malla de navegación](https://api.unrealengine.com/INT/Engine/AI/BehaviorTrees/QuickStart/2/index.html) de Unreal o bien puede especificar su propia malla de navegación. Debe etiquetar al menos un objeto como **Acoustics Navigation** (Navegación de Acoustics). Si usa la malla de navegación de Unreal, asegúrese de que la ha compilado antes.

### <a name="acoustics-volumes"></a>Volúmenes de Acoustics ###

Con los **volúmenes de Acoustics** se puede realizar una posterior personalización avanzada en las zonas de navegación. Los **volúmenes de Acoustics** son actores que se pueden agregar a la escena y que permiten seleccionar las áreas que se van a incluir y omitir en la malla de navegación. El actor expone una propiedad que se puede conmutar entre "Include" (Incluir) y "Exclude" (Excluir). Los volúmenes de inclusión garantizan que solo se tienen en cuenta las zonas de la malla de navegación que están dentro de ellos, mientras que los volúmenes de exclusión marcan dichas zonas para que se omitan. Los volúmenes de exclusión se aplican siempre después de los volúmenes de inclusión. Asegúrese de etiquetar los **volúmenes de Acoustics** como **Acoustics Navigation** (Navegación de Acoustics) a través del proceso habitual en la pestaña Objects (Objectos). Estos actores ***no*** se etiquetan automáticamente.

![Captura de pantalla de las propiedades de volumen de Acoustics en Unreal](media/unreal-acoustics-volume-properties.png)

Los volúmenes de exclusión están diseñados principalmente para proporcionar un mayor control sobre dónde no colocar sondeos para reforzar el uso de recursos.

![Captura de pantalla de las volumen de exclusión de Acoustics en Unreal](media/unreal-acoustics-volume-exclude.png)

Los volúmenes de inclusión son útiles para crear secciones manuales de una escena, por ejemplo si se desea dividir la escena en varias zonas acústicas. Por ejemplo, si tiene una escena de gran tamaño, de muchos kilómetros cuadrados, y tiene dos zonas de interés en las que desea realizar una simulación acústica mediante "bake". Puede dibujar dos grandes volúmenes "Include" en la escena y generar archivos ACE para ambos a la vez. Luego, en el juego, puede usar los volúmenes del desencadenador combinados con llamadas del plano técnico para cargar el archivo ACE adecuado cuando el reproductor se acerca a cada icono.

Los **volúmenes de Acoustics** solo restringen la navegación, ***no*** la geometría. Todos los sondeos que se encuentren dentro de un **volumen de Acoustics** de inclusión extraerán toda la geometría necesaria fuera del volumen al realizar simulaciones de onda. Por consiguiente, no debería haber discontinuidades en la oclusión ni otras acústicas debidas a que el reproductor pase de una sección a otra.

## <a name="select-acoustic-materials"></a>Selección de materiales acústicos

Una vez etiquetados los objetos, haga clic en el botón **Materials** (Materiales) para ir a la pestaña Materials (Materiales). Esta pestaña se utilizará para especificar las propiedades del material para cada material del nivel. Antes de que cualquier actor se etiqueta, estará en blanco.

Los materiales acústicos controlan la cantidad de energía sonora que se refleja desde cada superficie. El material acústico predeterminado tiene una absorción similar a la del hormigón. Project Acoustics sugiere materiales en base al nombre de material de la escena.

El tiempo de reverberación de un material determinado en una habitación está inversamente relacionado con su coeficiente de absorción y la mayoría de los materiales tiene valores de absorción en un rango que va desde 0,01 a 0,20. Los materiales con coeficientes de absorción que quedan sobre este rango son muy absorbentes. Por ejemplo, si en una sala se produce demasiada reverberación, cambie el material acústico de las paredes, suelo o techo por otro con mayor absorción. La asignación de material acústico se aplica a todos los objetos actores que usan ese material de la escena.

![Gráfico que muestra una correlación negativa de tiempo de reverberación con el coeficiente de absorción](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Como referencia: Partes de la pestaña Materials (Materiales)

![Captura de pantalla de la pestaña Objects (Objetos) de Acoustics en Unreal](media/unreal-materials-tab-details.png)

1. El botón de la pestaña **Materials** (Materiales), se usa para abrir esta página.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. La lista de los materiales utilizados en el nivel, tomada de los actores etiquetados como **AcousticsGeometry**. Al hacer clic en un material, se seleccionarán todos los objetos de la escena que usan ese material.
4. Se muestra el material acústico al que se ha asignado el material de la escena. Haga clic en un menú desplegable para volver a asignar un material de la escena a un material acústico diferente.
5. Muestra el coeficiente de absorción acústica del material seleccionado en la columna anterior. Un valor de cero significa totalmente reflectante (ninguna absorción), mientras que un valor de 1 significa totalmente absorbente (sin reflexión). Al cambiar este valor, se actualizará el material de Acoustics (paso 4) a **Custom** (Personalizado).

Si realiza cambios en los materiales de la escena, deberá cambiar de pestaña en el complemento de Project Acoustics para verlos reflejados en la pestaña **Materials** (Materiales).

## <a name="calculate-and-review-listener-probe-locations"></a>Calculo y revisión de las ubicaciones de sondeo del oyente

Después de asignar los materiales, cambie a la pestaña **Probes** (Sondeos).

### <a name="for-reference-parts-of-the-probes-tab"></a>Como referencia: Partes de la pestaña Probes (Sondeos)

![Captura de pantalla de la pestaña Objects (Objetos) de Acoustics en Unreal](media/unreal-probes-tab-details.png)

1. El botón de la pestaña **Probes** (Sondeos), se usa para abrir esta página.
2. Una breve descripción de lo que debe hacer al usar esta página.
3. Use esto para elegir una resolución de simulación gruesa o fina. Gruesa es más rápido, pero tiene algunos inconvenientes. Para más información, consulte [Resolución de simulación mediante "bake"](bake-resolution.md).
4. Elija la ubicación donde se deben colocar los archivos de datos acústicos con este campo. Haga clic en el botón "..." para usar un selector de carpetas. Para más información acerca de los archivos de datos, consulte más abajo la sección de [Archivos de datos](#Data-Files).
5. El nombre de los archivos de datos para esta escena llevará el prefijo proporcionado aquí. El valor predeterminado es "[NombreNivel]_AcousticsData".
6. Haga clic en el botón **Calculate** (Calcular) para voxelizar la escena y calcular las ubicaciones de puntos de sondeo. Esto se realiza localmente en el equipo y debe hacerse antes de una elaboración. Cuando se hayan calculado los sondeos, se deshabilitarán los controles anteriores y este botón cambiará a **Clear** (Borrar). Haga clic en el botón **Clear** (Borrar) para borrar los cálculos y habilitar los controles de modo que pueda realizar otro cálculo con otros valores.

Los sondeos deben colocarse mediante el proceso automatizado que se proporciona en la pestaña **Probes** (Sondeos).


### <a name="what-the-calculate-button-calculates"></a>Lo que calcula el botón "Calculate" (Calcular)

El botón **Calculate** (Calcular) toma todos los datos que proporcionados hasta el momento (geometría, navegación, materiales y configuración gruesa o fina) y realiza varios pasos:

1. Toma la geometría de las mallas de la escena y calcula un volumen de vóxel. El volumen de vóxel es un volumen de 3 dimensiones que rodea toda la escena y se compone de pequeños "vóxeles" cúbicos. El tamaño de los vóxeles viene determinado por la frecuencia de la simulación, que se establece mediante la configuración **Simulation Resolution** (Resolución de configuración). Se marca cada vóxel como "cielo abierto" o que contiene geometría de la escena. Si un vóxel contiene geometría, se etiqueta con el coeficiente de absorción del material asignado a esa geometría.
2. A continuación, usa los datos de navegación para calcular las ubicaciones acústicamente interesantes donde es posible que el jugador vaya. Intenta encontrar un conjunto relativamente pequeño de estas ubicaciones que incluya áreas más pequeñas, como entradas y vestíbulos y luego salas y espacios abiertos. Para las escenas pequeñas suele ser de menos de 100 ubicaciones, mientras que las escenas de gran tamaño pueden tener hasta mil.
3. Para cada una de las ubicaciones de escucha final que calcula, determina varios parámetros, como cuán "abierto" es el espacio, el tamaño de la sala en que se encuentra, etcétera.
4. Los resultados de estos cálculos se almacenan en archivos en la ubicación que especifique (consulte [Archivos de datos](#Data-Files) más abajo).

Según el tamaño de la escena y la velocidad del equipo, estos cálculos pueden tardar varios minutos.

Una vez completados estos cálculos, puede obtener una vista previa de los datos de vóxel y las ubicaciones de puntos de sondeo para garantizar que la elaboración ofrecerá buenos resultados. Normalmente, casos como una malla de navegación incorrecta o que falte o sobre una geometría se verán rápidamente en la vista previa, por lo que podrá corregirlos.


## <a name="debug-display"></a>Visualización de depuración

Después de completar el cálculo del sondeo, aparecerá un nuevo actor en World Outliner llamado **AcousticsDebugRenderer**. Si se seleccionan las casillas **Render Probes** (Representar sondeos) y **Render Voxels** (Representar vóxeles), se habilitará la visualización de depuración dentro de la ventanilla del editor.

![Captura de pantalla que muestra el actor Representador de depuración de Acoustics en el Editor de Unreal](media/acoustics-debug-renderer.png)

Si no se ve ningún vóxel o sondeo superpuestos al nivel, hay que asegurarse de que la representación en tiempo real esté habilitada en la ventanilla.

![Captura de pantalla de la opción de representación en tiempo real en Unreal](media/unreal-real-time-rendering.png)

### <a name="voxels"></a>Vóxeles

Los vóxeles se muestran en la ventana de la escena como cubos verdes alrededor de la geometría participante. No se muestran los vóxeles que contienen solo aire. Hay un gran recuadro verde alrededor de toda la escena que indica el volumen de vóxel completo que se usará en la simulación.
Muévase por la escena y compruebe que la geometría de oclusión acústica tiene vóxeles. Además, compruebe que no se han voxelizado objetos no acústicos, como mallas de colisión. La cámara de la escena debe estar dentro de aproximadamente 5 metros del objeto para que se muestren los vóxeles.

Si se comparan los vóxeles creados con resolución gruesa con los creados con resolución fina, verá que los vóxeles gruesos son el doble de grandes.

![Captura de pantalla de vista previa de vóxeles de Acoustics en el editor de Unreal](media/unreal-voxel-preview.png)

### <a name="probe-points"></a>Puntos de sondeo

Los puntos de sondeo son sinónimos de ubicaciones posibles del jugador (escucha). Al simular mediante "bake", la simulación calcula la acústica mediante la conexión de todas las posibles ubicaciones de origen a cada punto del sondeo. En el entorno de ejecución, la ubicación del oyente se interpola entre los puntos de sondeo cercanos.

Es importante comprobar que existen puntos de sondeo en cualquier lugar del escenario en el que se espera que el jugador se desplace. Los puntos de sondeo los coloca en la malla de navegación el motor de Project Acoustics y no se pueden mover ni modificar, así que asegúrese de que la malla de navegación cubra todas las posibles ubicaciones de los jugadores inspeccionando los puntos de sondeo.

![Captura de pantalla de la versión preliminar de los sondeos de Acoustics en Unreal](media/unreal-probes-preview.png)

Para más información acerca de la diferencia entre baja y alta resolución, consulte [Resolución de simulación mediante "bake"](bake-resolution.md).

## <a name="bake-your-level-using-azure-batch"></a>Simular mediante "bake" el nivel con Azure Batch

Se puede simular mediante "bake" la escena con un clúster de proceso en la nube usando el servicio Azure Batch. El complemento de Unreal para Project Acoustics se conecta directamente a Azure Batch para crear instancias, administrar y eliminar un clúster de Azure Batch para cada simulación mediante "bake". En la pestaña Bake (Simular mediante "bake"), escriba sus credenciales de Azure, seleccione un tamaño y tipo de máquina del clúster y haga clic en Bake (Simular mediante "bake").

### <a name="for-reference-parts-of-the-bake-tab"></a>Como referencia: Partes de la pestaña Bake (Elaborar)

![Captura de pantalla de la pestaña Bake (Elaborar) de Acoustics en Unreal](media/unreal-bake-tab-details.png)

1. El botón de la pestaña Bake (Elaborar) se usa para abrir esta página.
2. Una breve descripción de qué hacer en esta página.
3. Campos para especificar las credenciales de Azure una vez creada la cuenta de Azure. Para más información, vea [Create an Azure Batch Account](create-azure-account.md) (Crear una cuenta de Azure Batch).
4. Abra Azure Portal para administrar las suscripciones, supervisar el uso, ver información de facturación, etc. 
5. Tipo de nodo de proceso de Azure Batch que se usará para el cálculo. El tipo de nodo debe ser compatible con la ubicación del centro de datos de Azure. Si no está seguro, deje **Standard_F8s_v2**.
6. Número de nodos que se usarán para este cálculo. El número que especifique aquí afectará al tiempo en que se completará la elaboración y está limitado por la asignación de núcleos de Azure Batch. La asignación predeterminada solo permite dos nodos de 8 núcleos o un nodo de 16 núcleos, pero se puede expandir. Para obtener más información sobre las restricciones de la asignación de núcleos, consulte [Create an Azure Batch Account](create-azure-account.md) (Crear una cuenta de Azure Batch).
7. Active esta casilla para configurar el grupo de proceso para que use [nodos de prioridad baja](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Los nodos de proceso de prioridad baja tienen un costo mucho menor, pero no siempre están disponibles o se pueden reemplazar en cualquier momento.
8. La cantidad de tiempo transcurrido que se espera que tarde el trabajo en ejecutarse en la nube. Esto no incluye el tiempo de inicio del nodo. Una vez que el trabajo comienza a ejecutarse, es el tiempo que tardará en obtener los resultados. Tenga en cuenta que esto es solo una estimación.
9. La cantidad total de tiempo de proceso necesario para ejecutar las simulaciones. Se trata de la cantidad total de tiempo de proceso del nodo que se usará en Azure. Consulte [Estimación de costos de la elaboración](#Estimating-bake-cost) más abajo para obtener más información sobre el uso de este valor.
10. Haga clic en el botón Bake (Elaborar) para enviar la elaboración a la nube. Mientras se ejecuta un trabajo, se muestra **Cancel Job** (Cancelar trabajo) en su lugar. Si hay algún error en esta pestaña, o si el flujo de trabajo en la pestaña **Probes** (Sondeos) no se ha completado, este botón se deshabilitará.
11. El recuento del sondeo de la escena tal como se ha calculado en la pestaña **Probes** (Sondeos). El número de sondeos determina el número de simulaciones que se deben ejecutar en la nube. No se pueden especificar más nodos que sondeos.
12. Este mensaje indica el estado actual del trabajo o, si hay algún error en esta pestaña, cuáles son esos errores.

Siempre puede obtener la información completa sobre los trabajos activos, los grupos de proceso y el almacenamiento en [Azure Portal](https://portal.azure.com).

Mientras un trabajo está en ejecución, el botón **Bake** (Elaborar) cambia a **Cancel Job** (Cancelar trabajo). Use este botón para cancelar el trabajo en curso. La cancelación de un trabajo no se puede deshacer, no estará disponible ningún resultado y se le cobrará por el tiempo de proceso de Azure usado antes de la cancelación.

Una vez iniciada una simulación acústica mediante "bake", puede cerrar Unreal. Según el proyecto, el tipo de nodo y el número de nodos, una elaboración en la nube puede tardar varias horas. Se actualizará el estado del trabajo de elaboración al recargar el proyecto y abrir la ventana Acoustics (Acústica). Si se ha completado el trabajo, se descargará el archivo de salida.

Las credenciales de Azure se almacenan de forma segura en el equipo local y están asociadas al proyecto Unreal. Se usan únicamente para establecer una conexión segura con Azure.

### <a name="estimating-azure-bake-cost"></a><a name="Estimating-bake-cost"></a> Estimación del costo de elaboración de Azure

Para estimar lo que costará una elaboración determinada, tome el valor que se muestra para **Estimated Compute Cost** (Costo de proceso estimado), que es una duración, y multiplíquelo por el costo por hora en su moneda local del **VM Node Type** (Tipo de nodo VM) que ha seleccionado. El resultado no incluirá el tiempo de nodo necesario para que los nodos entren en ejecución. Por ejemplo, si selecciona **Standard_F8s_v2** para el tipo de nodo, que tiene un costo de 0,40 USD/hora y el costo estimado de proceso es de 3 horas y 57 minutos, el costo estimado para la ejecución del trabajo será 0,40 USD * ~ 4 horas = ~ 1,60 USD. El costo real probablemente será un poco mayor debido al tiempo adicional para iniciar los nodos. Puede encontrar el costo de nodo por hora en la página [Azure Batch Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (Precios de Azure Batch) (seleccione "Proceso optimizado" o "Proceso de alto rendimiento" para la categoría).

### <a name="reviewing-the-bake-results"></a>Revisión de los resultados de la elaboración

Una vez finalizada la elaboración, compruebe que los vóxeles y los puntos de sondeo estén en las ubicaciones esperadas, ejecutando el complemento de tiempo de ejecución.

## <a name="data-files"></a><a name="Data-Files"></a>Archivos de datos

Este complemento crea cuatro archivos de datos en varios puntos. Solo se necesita uno de ellos en tiempo de ejecución y se coloca en la carpeta Content/Acoustics del proyecto, que se agrega automáticamente a la ruta de empaquetado del proyecto. Los otros tres están dentro de la carpeta Acoustics Data y no están empaquetados.

* **[Project]/Config/ProjectAcoustics.cfg**: Este archivo almacena los datos que se especifiquen en los campos de la interfaz de usuario del modo Acoustics. No se puede cambiar la ubicación y el nombre de este archivo. Otros valores almacenados en este archivo afectan a la elaboración, pero son para usuarios avanzados y no deben cambiarse.
* **[Proyecto]/Content/Acoustics/[LevelName]\_AcousticsData.ace**: este archivo es lo que se crea durante la simulación mediante "bake" y contiene los datos de búsqueda que usa el runtime para representar la acústica de la escena. La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos). Si quiere cambiar el nombre de este archivo una vez creado, elimine UAsset del proyecto de Unreal, cambie el nombre del archivo fuera de Unreal en el Explorador de archivos y, luego, vuelva a importar este archivo a Unreal para generar un UAsset nuevo. Cambiar el nombre del UAsset por sí solo no funcionará.
* **[Proyecto]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData.vox**: Este archivo almacena la geometría de acústica convertida en vóxeles y las propiedades del material. Se calcula con el botón **Calculate** (Calcular) de la pestaña **Probes** (Sondeos). La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).
* **[Proyecto]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData\_config.xml**: este archivo almacena los parámetros calculados mediante el botón **Calculate** (Calcular) de la pestaña **Probes** (Sondeos). La ubicación y el nombre de este archivo se pueden cambiar utilizando los campos de la pestaña **Probes** (Sondeos).

Tenga cuidado de no eliminar el archivo *.ace descargado desde Azure. Este archivo no es recuperable, excepto si se vuelve a elaborar la escena.

## <a name="next-steps"></a>Pasos siguientes
* Explore los [controles de diseño para Unreal](unreal-workflow.md).
* Explore los [conceptos de diseño de Project Acoustics](design-process.md).

