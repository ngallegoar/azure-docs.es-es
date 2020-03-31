---
title: Tutorial de simulación mediante "bake" de Project Acoustics con Unity
titlesuffix: Azure Cognitive Services
description: Este tutorial describe la simulación acústica mediante "bake" de Project Acoustics con Unity.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 310decf8053ea16ba46250ba3aabe81c9c254e5e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72243123"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Tutorial de simulación mediante "bake" de Project Acoustics con Unity
Este tutorial describe la simulación acústica mediante "bake" con Project Acoustics en Unity.

Requisitos de software:
* [Unity 2018.2+](https://unity3d.com) para Windows o macOS
* [Complemento de Project Acoustics integrado en el proyecto de Unity](unity-integration.md) o [contenido de ejemplo de Project Acoustics con Unity](unity-quickstart.md)
* *Opcional:* una [cuenta de Azure Batch](create-azure-account.md) para acelerar la simulación mediante "bake" utilizando la informática en la nube

## <a name="open-the-project-acoustics-bake-window"></a>Apertura de la ventana de simulación mediante "bake" de Project Acoustics
En Unity, seleccione **Acoustics** (Acústica) en el menú **Window** (Ventana).

![Editor de Unity con la opción del menú Window (Ventana) Acoustics (Acústica) resaltada](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Creación de una malla de navegación
Project Acoustics utiliza una malla de navegación para colocar puntos de sondeo del oyente para la simulación. Puede usar el [flujo de trabajo de la malla de navegación](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html) de Unity. O bien, puede usar un paquete de modelado 3D diferente para diseñar su propia malla.

## <a name="mark-acoustic-scene-objects"></a>Marcado de objetos de la escena acústica
Project Acoustics se basa en dos tipos de objetos de escena para la simulación:
- Los objetos que reflejan y bloquean el sonido en la simulación
- La malla de navegación del jugador que restringe los puntos de sondeo del oyente en la simulación

Ambos tipos de objetos se marcan mediante la pestaña **Objects** (Objetos).

Dado que marcar objetos simplemente agrega los componentes *AcousticsGeometry* o *AcousticsNavigation* al objeto, también puede usar el [flujo de trabajo estándar del componente Unity](https://docs.unity3d.com/Manual/UsingComponents.html) para marca o desmarca los objetos. Solo puede marcar representadores de malla y terrenos. Se omitirán todos los demás tipos de objeto. Las casillas marcan o desmarcan todos los objetos afectados.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Marcado de oclusión acústica y geometría de reflexión
Abra la pestaña **Objects** (Objetos) de la ventana **Acoustics** (Acústica). Marque los objetos como *Acoustics Geometry* (Geometría acústica) si deben bloquear, reflejar o absorber el sonido. La geometría acústica puede incluir elementos como suelo, paredes, techos, ventanas y cristales de ventana, alfombras y muebles grandes. Puede usar cualquier nivel de complejidad arbitrario para estos objetos. Dado que la escena se voxelizará antes de la simulación, las mallas muy detalladas, como por ejemplo los árboles con muchas hojas, no son más costosas para simular mediante "bake" que los objetos simplificados.

No incluya elementos que no afecten a la acústica, como las mallas de colisión invisibles.

La transformación de un objeto durante el cálculo del sondeo (mediante la pestaña **Probes** [Sondeos]) se fija en los resultados de la simulación acústica mediante "bake". Si más adelante se mueve alguno de los objetos marcados en la escena, se deberán rehacer el cálculo de sondeo y la simulación mediante "bake".

### <a name="mark-the-navigation-mesh"></a>Marca de una malla de navegación
El sistema acústico tomará las mallas de navegación que se crearon mediante el flujo de trabajo de Unity. Para usar sus propias mallas, márquelas en la pestaña **Objects** (Objetos).

### <a name="for-reference-the-objects-tab-parts"></a>Como referencia: Partes de la pestaña Objects (Objetos)
Las partes de la pestaña (que aparecen en la imagen tras las descripciones) son las siguientes:

1. Los botones de selección de la pestaña (con la pestaña **Objects** [Objetos] seleccionada). Use estos botones para desplazarse por los distintos pasos de la simulación acústica mediante "bake", de izquierda a derecha.
1. Una breve descripción de lo que se puede hacer con esta pestaña.
1. Filtros disponibles para la ventana de la jerarquía. Use estas opciones para filtrar la ventana de la jerarquía con los objetos del tipo especificado, de modo que pueda marcarlos fácilmente. Si aún no ha marcado nada para la acústica, la selección de las dos últimas opciones no mostrará nada. Sin embargo, estas opciones le ayudarán a localizar objetos después de que se hayan marcado.
1. Si no hay ningún objeto seleccionado, en esta sección se muestra el estado de todos los objetos de la escena.
    * Total: el número total de objetos activos no ocultos.
    * Ignored (Omitido): el número de objetos que no son representadores de malla ni terrenos.
    * Mesh (Malla): el número de objetos de representador de malla.
    * Terrain (Terreno): el número de objetos de terreno.
    * Geometry (Geometría): el número de objetos de malla o terreno marcados como **Acoustics Geometry** (Geometría acústica).
    * Navigation (Navegación): el número de objetos de malla o terreno marcados como **Acoustics Navigation** (Navegación de Acoustics). En este número no se incluye NavMesh de Unity.
1. El número total de objetos que se pueden marcar de la escena; solo son representadores de malla y terrenos. Utilice las casillas para marcar (agregar el componente adecuado) los objetos como geometría o navegación para la acústica.
1. Cuando no hay nada seleccionado, esta nota le recuerda que seleccione objetos para marcarlos, si es necesario. También puede seleccionar una o ambas casillas para marcar todos los objetos de la escena.
1. Cuando se han seleccionado objetos, en esta sección se muestra únicamente el estado de los objetos seleccionados.
1. El número total de objetos seleccionados que se pueden marcar. Al activar o desactivar las casillas, se marcan o desmarcan solo los objetos seleccionados.

Si no hay nada seleccionado en la escena, la pestaña **Objects** (Objetos) será similar a la siguiente imagen.

![La pestaña de objetos acústicos sin nada seleccionado](media/objects-tab-no-selection-detail.png)

Si ha seleccionado algún elemento en la escena o en la ventana de jerarquía, la pestaña será similar a la siguiente imagen.

![La pestaña de objetos acústicos con selecciones](media/objects-tab-selection-detail.png)

Si hay algunos objetos marcados y otros no, la casilla correspondiente mostrará un valor "mixto", como en la siguiente imagen.

![La pestaña de objetos acústicos con una selección mixta de iconos resaltados](media/mixed-object-selection-detail.png)

Seleccione la casilla para marcar todos los elementos. Anule la selección para desmarcar todos los objetos.

Los objetos se pueden marcar para la geometría y la navegación.

## <a name="select-acoustic-materials"></a>Selección de materiales acústicos
Después de marcar los objetos, seleccione el botón **Materials** (Materiales). A continuación, asigne materiales acústicos. El sistema de materiales de Project Acoustics está vinculado al sistema de materiales visuales de Unity. Para que dos objetos tengan materiales acústicos independientes, deben tener materiales visuales independientes.

La selección del material acústico determina la cantidad de energía sonora que se refleja desde cada superficie. El material acústico predeterminado tiene una absorción similar a la del acero. Project Acoustics sugiere materiales en base al nombre de material visual. Puede asignar el material acústico **Custom** (Personalizado) a un material para activar un control deslizante de coeficiente de absorción ajustable.

El tiempo de reverberación de un material determinado en una habitación está inversamente relacionado con su coeficiente de absorción. La mayoría de los materiales tienen valores de absorción en un rango que va desde 0,01 a 0,20. Los materiales que tienen coeficientes de absorción que quedan fuera de este rango son muy absorbentes.

![Un gráfico muestra la correlación negativa entre el tiempo de reverberación y el coeficiente de absorción.](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Como referencia: Partes de la pestaña Materials (Materiales)
![La pestaña de materiales acústicos en Unity](media/materials-tab-detail.png)
1. El botón **Materials** (Materiales) muestra esta pestaña.
2. Una breve descripción de lo que se puede hacer con esta pestaña.
3. Cuando se activa esta casilla, solo se muestran los materiales utilizados por los objetos marcados como **Acoustics Geometry** (Geometría acústica). En caso contrario, se muestran todos los materiales usados en la escena.
4. Use estas opciones para cambiar el orden de las opciones en un menú de la columna **Acoustics** (Acústica) (número 6). Ordene los materiales acústicos por los valores de **Name** (Nombre) o **Absorptivity** (Absorción), de menor a mayor.
5. Una lista ordenada alfabéticamente de los materiales que se usan en la escena. Si se selecciona la casilla **Show Marked Only** (Mostrar solo los marcados) (número 3), solo se muestran los materiales usados por los objetos marcados como **Acoustics Geometry** (Geometría acústica). Seleccione aquí un material para seleccionar todos los objetos de la escena que lo usan.
6. El material acústico al que se ha asignado el material de la escena. Seleccione cualquier elemento para cambiar el material acústico asignado a ese material de la escena. Para cambiar el criterio de ordenación de estos menús, use las opciones de **Sort Acoustics By** (Ordenar acústica por) (número 4).
7. El coeficiente de absorción acústica del material seleccionado en la columna de la izquierda (número 6). Un valor de 0 significa totalmente reflectante (ninguna absorción), mientras que un valor de 1 significa totalmente absorbente (sin reflexión). No se puede cambiar el coeficiente de absorción, a menos que el material seleccionado sea **Custom** (Personalizado).
8. En los materiales marcados como **Custom** (Personalizado), se activa el control deslizante. Para asignar un coeficiente de absorción, puede desplazar el control deslizante o escribir un valor.

## <a name="calculate-and-review-listener-probe-locations"></a>Calculo y revisión de las ubicaciones de sondeo del oyente
Después de asignar los materiales, cambie a la pestaña **Probes** (Sondeos). Seleccione **Calculate** (Calcular) para colocar los puntos de sondeo del oyente para la simulación.

### <a name="what-the-calculate-button-does"></a>Acciones del botón "Calculate" (Calcular)
El botón **Calculate** (Calcular) usa la geometría acústica de la escena seleccionada para preparar la simulación de esa escena:

- Toma la geometría de las mallas de la escena y calcula un *volumen de vóxel*. El volumen de vóxel es un volumen de toda la escena que se compone de pequeños "vóxeles" cúbicos. El tamaño de los vóxeles viene determinado por la frecuencia de la simulación, que se controla mediante el valor **Simulation Resolution** (Resolución de simulación). Cada vóxel se marca como "a cielo abierto" o que contiene geometría de la escena. Si un vóxel contiene geometría, se etiqueta con el coeficiente de absorción del material asignado a esa geometría.
- Usa las mallas de navegación para colocar puntos de sondeo del oyente. El algoritmo equilibra los aspectos enfrentados de cobertura espacial, tiempo y tamaño de archivo de la simulación. Su objetivo es garantizar que los pasillos estrechos y los espacios pequeños siempre tienen alguna cobertura. El recuento típico de puntos de sondeo va desde 100 para escenas pequeñas a varios miles para escenas grandes.

Según el tamaño de la escena y la velocidad del equipo, estos cálculos pueden tardar varios minutos.

### <a name="review-voxel-and-probe-placement"></a>Revisión de la ubicación de vóxeles y puntos de sondeo
Obtenga una vista previa de los datos de vóxel y de la ubicación de los puntos de sondeo para asegurarse de que está listo para realizar la simulación mediante "bake" de la escena. Una malla de navegación incompleta o una geometría acústica de más o de menos suelen verse fácilmente en la vista previa. Habilite o deshabilite la colocación de los vóxeles y los sondeos mediante el menú **Gizmos**.

![El menú Gizmos de Unity](media/gizmos-menu.png)

Los vóxeles que contienen geometría acústica se muestran como cubos verdes. Explore la escena y compruebe que todo lo que debe ser geometría tiene vóxeles. La cámara de la escena debe estar dentro de aproximadamente 5 metros del objeto para que se muestren los vóxeles.

Si compara los vóxeles creados con resolución gruesa con los creados con resolución fina, verá que los vóxeles gruesos son el doble de grandes.

![La vista previa de vóxeles gruesos en el editor de Unity](media/voxel-cubes-preview.png)

Los resultados de la simulación se interpolan entre ubicaciones de puntos de sondeo del oyente en tiempo de ejecución. Compruebe que haya puntos de sondeo cerca de cualquier lugar al que el jugador pueda ir dentro de la escena.

![La vista previa de sondeos en el editor de Unity](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Tenga cuidado con los cambios de nombre de las escenas
El nombre de la escena se usa para conectar la escena a los archivos que almacenan la colocación de los puntos de sondeo y la voxelización. Si cambia el nombre de la escena una vez calculados los puntos de sondeo, se pierden los datos de asignación y colocación del material y se debe volver a realizar la ejecución.

### <a name="for-reference-parts-of-the-probes-tab"></a>Como referencia: Partes de la pestaña Probes (Sondeos)
![Pestaña de sondeos acústicos de Unity](media/probes-tab-detail.png)

1. El botón **Probes** (Sondeos) muestra esta pestaña.
2. Una breve descripción de lo que se puede hacer en esta pestaña.
3. Use estas opciones para establecer la resolución de simulación gruesa o fina. La resolución gruesa es más rápida, pero tiene algunos inconvenientes. Para obtener detalles, consulte [Resolución de la simulación mediante "bake"](bake-resolution.md).
4. Especifica la ubicación de los archivos de datos acústicos. Seleccione el botón " **...** " para acceder a un selector de carpetas. La ubicación predeterminada es *Assets/AcousticsData*. También se crea una subcarpeta *Editor* en esta ubicación. Para más información, consulte [Archivos de datos agregados por el proceso de simulación mediante "bake"](#Data-Files), más adelante en este artículo.
5. El prefijo que se especifica aquí se usa para asignar nombre a los archivos de datos de esta escena. El valor predeterminado es "Acoustics_ *[nombre de la escena]* ".
6. Una vez calculados los sondeos, se deshabilitan los controles que acabamos de describir. Seleccione el botón **Clear** (Borrar) para borrar los cálculos y habilitar los controles; de este modo puede realizar otro cálculo con nuevos valores.
7. Seleccione **Calculate** (Calcular) para voxelizar la escena y calcular las ubicaciones de los puntos de sondeo. El cálculo se realiza localmente en la máquina. Debe efectuarse antes de llevar a cabo la simulación mediante "bake".

En esta versión de Project Acoustics, los sondeos no se pueden colocar manualmente. Use el proceso automatizado de la pestaña **Probes** (Sondeos).

Para más información acerca de las diferencias entre las resoluciones gruesa y fina, consulte [Resolución de la simulación mediante "bake"](bake-resolution.md).

## <a name="bake-your-scene-by-using-azure-batch"></a>Simulación mediante "bake" de la escena con Azure Batch
El servicio Azure Batch permite simular mediante "bake" la escena en un clúster de proceso en la nube. El complemento de Project Acoustics para Unity se conecta directamente a Azure Batch para crear instancias, administrar y anular un clúster de Azure Batch para cada simulación mediante "bake". En la pestaña **Bake** (Simular mediante "bake"), escriba sus credenciales de Azure, seleccione un tamaño y tipo de máquina del clúster y, después, seleccione **Bake** (Elaborar).

### <a name="for-reference-parts-of-the-bake-tab"></a>Como referencia: Partes de la pestaña Bake
![La pestaña de simulación acústica mediante "bake" en Unity](media/bake-tab-details.png)

1. El botón **Bake** (Simular mediante "bake") muestra esta pestaña.
2. Una breve descripción de lo que se puede hacer en esta página.
3. Escriba sus credenciales de Azure en estos campos, una vez creada la cuenta de Azure. Para más información, consulte [Creación de una cuenta de Azure Batch](create-azure-account.md).
4. El campo de etiqueta de imagen de Docker para el conjunto de herramientas acústicas.
5. Abre Azure Portal para administrar las suscripciones, supervisar el uso y ver información de facturación.
6. Especifica el tipo de nodo de proceso de Azure Batch que se usará para el cálculo. El tipo de nodo debe ser compatible con la ubicación del centro de datos de Azure. Si no está seguro, déjelo como **Standard_F8s_v2**.
7. El número de nodos que se usarán para el cálculo. Este número afecta al tiempo de la simulación mediante "bake". Está limitado por la asignación de núcleos de Azure Batch. La asignación predeterminada solo permite dos nodos de 8 núcleos o un nodo de 16 núcleos, pero se puede expandir. Para más información sobre las restricciones de la asignación de núcleos, consulte [Creación de una cuenta de Azure Batch](create-azure-account.md).
8. Seleccione esta casilla para establecer que el grupo de proceso use [nodos de prioridad baja](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Los nodos de proceso de prioridad baja tienen un costo mucho menor. Sin embargo, es posible que no siempre estén disponibles o que se reemplacen en cualquier momento.
9. El recuento del sondeo de la escena tal como se ha calculado en la pestaña **Probes** (Sondeos). El número de sondeos determina el número de simulaciones que se deben ejecutar en la nube. No se pueden especificar más nodos que sondeos.
10. Una estimación del tiempo que el trabajo tardará en ejecutarse en la nube, sin contar el tiempo de inicio del nodo. Después de que el trabajo empiece a ejecutarse, este campo muestra una estimación de cuánto tiempo pasará hasta que se obtengan los resultados.
11. La cantidad total de tiempo de proceso necesario para ejecutar las simulaciones. Este valor es la cantidad total de tiempo de proceso del nodo que se usará en Azure. Para más información, consulte [Estimación del costo de simulación mediante "bake" de Azure](#Estimating-bake-cost), más adelante en este artículo.
12. Este mensaje indica dónde se guardarán los resultados de la simulación mediante "bake" cuando finalice el trabajo.
13. *(Solo uso avanzado)* : este botón obliga a Unity a olvidarse de una simulación mediante "bake" enviada. Por ejemplo, si descargó los resultados con otra máquina, seleccione el botón **Clear State** (Borrar estado) para olvidar ese trabajo. Cuando esté listo el archivo de resultados, *no* se descargará. *Esto no es lo mismo que cancelar el trabajo. Si el trabajo se está ejecutando, seguirá haciéndolo en la nube*.
14. Seleccione este botón para enviar la simulación mediante "bake" a la nube. Mientras se ejecuta un trabajo, este botón se convierte en **Cancel job** (Cancelar trabajo).
15. Seleccione este botón para preparar el procesamiento de la [simulación acústica en el equipo](#Local-bake).
16. En esta área se muestra el estado de la elaboración. Cuando se completa la simulación mediante "bake", muestra "Downloaded" (Descargado).

Siempre puede obtener la información completa sobre los trabajos activos, los grupos de proceso y el almacenamiento en [Azure Portal](https://portal.azure.com).

Mientras un trabajo está en ejecución, la etiqueta del botón **Bake** (Elaborar) cambia a **Cancel Job** (Cancelar trabajo). Use este botón para cancelar el trabajo en curso. Se le pedirá confirmación. No se puede deshacer la cancelación de un trabajo. Al cancelar, no habrá resultados disponibles, pero se le seguirá cobrando por el tiempo de proceso de Azure usado.

Una vez iniciada una simulación mediante "bake", puede cerrar Unity. Según el proyecto, el tipo de nodo y el número de nodos, una elaboración en la nube puede tardar varias horas. Se actualizará el estado del trabajo de elaboración al recargar el proyecto y abrir la ventana Acoustics (Acústica). Si se ha completado el trabajo, se descargará el archivo de salida.

Por motivos de seguridad, las credenciales de Azure se almacenan en la máquina local y están asociadas al editor de Unity. Se usan únicamente para establecer una conexión segura con Azure.

## <a name="find-the-status-of-a-running-job-on-the-azure-portal"></a>Búsqueda del estado de un trabajo en ejecución en Azure Portal

1. Busque el identificador del trabajo de simulación mediante "bake" en la pestaña **Bake** (Simulación mediante "bake").

    ![El identificador de trabajo de simulación mediante "bake" de Unity resaltado en la pestaña Bake (Simulación mediante "bake")](media/unity-job-id.png)  

2. Abra [Azure Portal](https://portal.azure.com), vaya a la cuenta de Batch usada para el trabajo de simulación mediante "bake" y seleccione **Trabajos**.

    ![Vínculo Trabajos en Azure Portal](media/azure-batch-jobs.png)  

3. Busque el identificador del trabajo en la lista de trabajos.

   ![El estado del trabajo de simulación mediante "bake" resaltado en Azure Portal](media/azure-bake-job-status.png)  

4. Seleccione el identificador del trabajo para ver el estado de las tareas relacionadas y el estado general del trabajo.

   ![El estado de la tarea de simulación mediante "bake"](media/azure-batch-task-state.png)  


### <a name="estimate-azure-bake-cost"></a><a name="Estimating-bake-cost"></a> Estimación del costo de simulación mediante "bake" de Azure

Para estimar el costo de la simulación mediante "bake", comience con el valor **Estimated Compute Cost** (Costo de proceso estimado), que indica una duración. Multiplique ese valor por el costo por hora en la moneda local para el valor de **VM Node Type** (Tipo de nodo VM) que seleccione. Tenga en cuenta que el resultado no incluirá el tiempo de nodo necesario para que los nodos entren en funcionamiento.

Por ejemplo, suponga que selecciona **Standard_F8s_v2** como tipo de nodo, que tiene un costo de 0,40 USD/h. Si el valor de **Estimated Compute Cost** (Costo de proceso estimado) es de 3 horas y 57 minutos, el costo estimado de ejecución del trabajo será de 0,40 USD * ~ 4 horas = ~ 1,60 USD. El costo real probablemente será un poco mayor, debido al tiempo adicional necesario para iniciar los nodos.

Busque el costo por hora de los nodos en [Precios de Azure Batch](https://azure.microsoft.com/pricing/details/virtual-machines/linux) (seleccione los valores **Proceso optimizado** o **Proceso de alto rendimiento** en Categoría).

## <a name="bake-your-scene-on-your-pc"></a><a name="Local-bake"></a>Simulación mediante "bake" de una escena en el PC
Puede simular mediante "bake" una escena en su propio PC. Este método puede ser útil para experimentar con la acústica de escenas pequeñas antes de crear una cuenta de Azure Batch. Tenga en cuenta, sin embargo, que la simulación acústica local puede tardar mucho tiempo, según el tamaño de la escena.

### <a name="minimum-hardware-requirements"></a>Requisitos mínimos de hardware
* Procesador x86-64 con al menos 8 núcleos y 32 GB de RAM
* [Hyper-V habilitado](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) para ejecutar Docker

Por ejemplo, en las pruebas realizadas en una máquina de 8 núcleos con Intel Xeon E5-1660 a 3 GHz y con 32 GB de RAM:
* Una escena pequeña con 100 sondeos tardó unas 2 horas para una simulación mediante "bake" gruesa y unas 32 horas para una de resolución fina.
* Una escena de tamaño mediano con 1000 sondeos tardó unas 20 horas para una simulación mediante "bake" gruesa y 21 días para una de resolución fina.

### <a name="set-up-docker"></a>Configuración de Docker
Instale y configure Docker en el equipo en el que se va a procesar la simulación:
1. Instale [Docker Desktop](https://www.docker.com/products/docker-desktop).
2. Abra la configuración de Docker, vaya a **Advanced** (Avanzado) y configure los recursos para al menos 8 GB de RAM. Cuantas más CPU pueda asignar a Docker, más rápidamente se completará la simulación mediante "bake".  
![Configuración de Docker de ejemplo](media/docker-settings.png)
1. Vaya a **Shared Drives** (Unidades compartidas) y active el uso compartido de la unidad que se usa para el procesamiento.  
![Opciones de unidad compartida de Docker](media/docker-shared-drives.png)

### <a name="run-the-local-bake"></a>Ejecución de la simulación mediante "bake" local
1. Seleccione el botón **Prepare Local Bake** (Preparar simulación mediante "bake" local) en la pestaña **Bake** (Simulación mediante "bake"). A continuación, seleccione una ubicación de carpeta en la que guardar los archivos de entrada y los scripts de ejecución. Después, puede ejecutar la simulación mediante "bake" en cualquier máquina, siempre que cumpla los requisitos mínimos de hardware e instale Docker mediante la copia de la carpeta en esta máquina.
2. Para iniciar la simulación, ejecute el script *runlocalbake.bat* en Windows o bien el script *runlocalbake.sh* en MacOS. Este script captura la imagen de Docker de Project Acoustics con el conjunto de herramientas necesario para el procesamiento de la simulación e inicia la simulación.
3. Cuando haya finalizado la simulación, copie el archivo *.ace* resultante al proyecto de Unity. Para asegurarse de que Unity lo reconoce como un archivo binario, anexe ".bytes" a la extensión de archivo (por ejemplo, "Scene1.ace.bytes"). Los registros detallados de la simulación se almacenan en *AcousticsLog.txt*. Si tiene algún problema, inspeccione este archivo como ayuda para diagnosticarlo.

## <a name="data-files-added-by-the-bake-process"></a><a name="Data-Files"></a> Archivos de datos agregados por el proceso de simulación mediante "bake"

Durante el proceso de simulación mediante "bake" se crean los cuatro archivos de datos siguientes. Uno contiene los resultados de la simulación y se incluye con el título. Los demás almacenan datos relacionados con el editor de Unity.

Resultado de la simulación:
* *Assets/AcousticsData/Acoustics\_[nombreEscena].ace.bytes*: este archivo es la tabla de búsqueda del entorno de ejecución. Contiene los resultados de la simulación y los elementos acústicos voxelizados de la escena. Puede cambiar el nombre y la ubicación de este archivo en la pestaña **Probes** (Sondeos).

   *Tenga cuidado de no eliminar el archivo de resultados de la simulación. Este archivo no es recuperable, excepto si se vuelve a simular la escena mediante "bake".*

Archivos de datos del editor:
* *Assets/Editor/[nombreEscena]\_AcousticsParameters.asset*: este archivo almacena los datos que se especifican en los campos de la interfaz de usuario de Acoustics. No puede cambiar el nombre ni la ubicación de este archivo.
* *Assets/AcousticsData/Editor/Acoustics_[nombreEscena].vox*: este archivo almacena la geometría acústica voxelizada y las propiedades de material que se calculan al seleccionar el botón **Calculate** (Calcular) de la pestaña **Probes** (Sondeos). Puede cambiar el nombre y la ubicación de este archivo en la pestaña **Probes** (Sondeos).
* *Assets/AcousticsData/Editor/Acoustics\_[nombreEscena]\_config.xml*: este archivo almacena los parámetros de simulación que se calculan cuando se selecciona **Calculate** (Calcular). Puede cambiar el nombre y la ubicación de este archivo en la pestaña **Probes** (Sondeos).

## <a name="set-up-the-acoustics-lookup-table"></a>Configuración de la tabla de búsqueda acústica
Arrastre y coloque el objeto prefabricado **Project Acoustics** desde el panel del proyecto en la escena:

![Objeto prefabricado de acústica en Unity](media/acoustics-prefab.png)

Seleccione el objeto de juego **ProjectAcoustics** y vaya al panel del inspector. Especifique la ubicación del resultado de la simulación mediante "bake" (el archivo .ace, en *Assets/AcousticsData*): Arrástrelo al script del administrador de acústica o seleccione el botón de círculo situado junto al cuadro de texto.

![El objeto prefabricado del administrador de acústica en Unity](media/acoustics-manager.png)

## <a name="next-steps"></a>Pasos siguientes
* Explore los [controles de diseño de Unity](unity-workflow.md).
* Explore los [conceptos de diseño de Project Acoustics](design-process.md).
