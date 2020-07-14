---
title: Configuración de materiales de representación con representación física en 3DSMax
description: En este artículo se explica cómo configurar materiales con representación física en 3DSMax y exportarlos en formato FBX.
author: muxanickms
ms.author: misams
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: df4be8963c93199f9fad23ab3f709f691e1da768
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85857617"
---
# <a name="tutorial-set-up-physically-based-rendering-materials-in-3d-studio-max"></a>Tutorial: Configuración de materiales de representación con representación física en 3D Studio Max

## <a name="overview"></a>Información general
En este tutorial, aprenderá a:

>[!div class="checklist"]
>
> * Asignar materiales con iluminación avanzada a los objetos de la escena.
> * Administrar la creación de instancias de objetos y materiales.
> * Exportar una escena a formato FBX y seleccionar las opciones importantes.

La creación de [materiales de representación con representación física (PBR)](../../overview/features/pbr-materials.md) en 3D Studio Max (3DSMax) es una tarea relativamente sencilla. Es similar de muchas maneras a la configuración de PBR en otras aplicaciones de creación de contenido como Maya. Este tutorial es una guía para la configuración básica del sombreador de PBR y la exportación a FBX de proyectos de Azure Remote Rendering.

La escena de ejemplo de este tutorial contiene varios objetos de cuadro poligonal. Se les han asignado distintos materiales, como madera, metal, metal pintado, plástico y caucho. En términos generales, cada material contiene todas o la mayoría de las texturas siguientes:

* **Albedo**, que es el mapa de colores del material y también se llama **Difuso** o **Color de base**.
* **Metálico**, que determina si un material es metálico y qué partes son metálicas. 
* **Rugosidad**, que determina cómo de rugosa o suave es una superficie.
También afecta a la nitidez o al desenfoque de los reflejos y los resaltados en una superficie.
* **Normal**, que agrega detalles a una superficie sin necesidad de agregar más polígonos. Algunos ejemplos de detalle podrían ser las picaduras y las abolladuras en una superficie de metal o el grano en la madera.
* **Oclusión de ambiente**, que se usa para agregar matices suaves y sombras de contacto a un modelo. Se trata de un mapa de escala de grises que indica qué áreas de un modelo reciben una iluminación completa (blanco) o una sombra completa (negro).

## <a name="prepare-the-scene"></a>Preparación de la escena
En **3D Studio Max**, el proceso para configurar un material PBR es el siguiente.

Para empezar, como verá en la escena de ejemplo, hemos creado varios objetos de cuadro, cada uno de los cuales representa un tipo de material diferente:

>[!TIP]
>Antes de empezar a crear recursos para ARR, es importante advertir que se usan **metros** para las medidas.  
>Por lo tanto, se recomienda establecer las **Unidades del sistema** de la escena en **Metros**. Además, es aconsejable que al exportar se establezcan las unidades en metros en la configuración de exportación de FBX.

En la imagen siguiente se muestran los pasos para establecer las unidades del sistema en metros en 3D Studio Max. En el menú principal, vaya a **Customize** > **Units Setup** > **System Units Setup** (Personalizar>Configuración de unidades>Configuración de unidades del sistema) y, en la lista desplegable **System Units Scale** (Escala de las unidades del sistema), seleccione **Meters** (Metros). 
![Unidades del sistema](media/3dsmax/system-units.jpg)

Con las unidades del sistema establecidas en metros, podemos empezar a crear nuestros modelos. En la escena de ejemplo, se crean varios objetos de cuadro, cada uno de los cuales representa un tipo de material distinto (por ejemplo, metal, caucho, plástico, etc). 

>[!TIP]
>Es aconsejable asignar un nombre apropiado al crear los recursos. Esto hará que sea más fácil encontrarlos más adelante si la escena tiene muchos objetos.

![Cambiar el nombre a los objetos](media/3dsmax/rename-objects.jpg)

## <a name="assign-materials"></a>Asignación de materiales

Con algunos objetos creados en nuestra escena (en este caso, varios cubos), podemos iniciar la configuración de PBR:

* En la barra de herramientas principal, haga clic en el icono **Material Editor** (Editor de materiales), como se muestra en la siguiente imagen. También puede presionar la tecla **M** en el teclado para abrir el editor. El editor de materiales tiene dos modos que se pueden seleccionar en la lista desplegable **Modes** (Modos): el modo **Compact material editor** (Editor de materiales compacto) y el modo **Slate material** (Material de pizarra). Dado que esta escena es relativamente sencilla, usaremos el **modo compacto**.

* En el editor de materiales, verá una serie de esferas: estas esferas son nuestros materiales. Asignaremos uno de estos materiales a cada objeto (cuadro) de nuestra escena. Para realizar esta asignación, seleccione primero uno de los objetos en la ventanilla principal. Con esta selección realizada, haga clic en la primera esfera de la ventana del editor de materiales. Una vez que se haya asignado a un objeto, se resaltará el material seleccionado, tal como se muestra en la siguiente imagen.

* Haga clic en el botón **Assign Material to Selection** (Asignar material a la selección), tal y como se muestra. Ahora se ha asignado el material seleccionado al objeto seleccionado.
![Asignar material](media/3dsmax/assign-material.jpg)

En el editor de materiales, puede elegir tipos de materiales de una amplia selección, en función del caso de uso. Normalmente, el tipo de material se establece en **Estándar** de forma predeterminada. Este material es un material básico que no es adecuado para la configuración de PBR, por lo que será necesario cambiar el tipo de material a un material de PBR. El material de **3DSMax** preferido para los proyectos de Azure Remote Rendering es el **material físico**.

* En el editor de materiales, haga clic en la pestaña **Standard** (Estándar) y, en el explorador de materiales y mapas que se abre, seleccione **Physical Material** (Material físico). Esta acción convertirá el material **estándar** asignado a un **material físico** de PBR.
![Material físico](media/3dsmax/physical-material.jpg)

* En el editor de materiales, ahora verá las propiedades del material físico (consulte a continuación) y podemos empezar a asignar texturas al recurso.
![Lista de texturas](media/3dsmax/textures-list.jpg)

Como se puede observar en la imagen anterior, hay una amplia gama de mapas y texturas que se pueden agregar al material. Sin embargo, para nuestros fines, usaremos solo cinco ranuras de textura en el material.

>[!TIP]
>Es recomendable asignar un nombre adecuado a los materiales, como se muestra en la imagen anterior.

Ahora podemos empezar a considerar la asignación de texturas a nuestro material. La forma de generar las texturas puede variar según las preferencias o incluso según el uso. Por ejemplo, puede que esté satisfecho con el uso de texturas de mosaico, que se pueden aplicar a cualquier recurso o puede que necesite que algunas partes específicas de un proyecto o recurso tengan su propio conjunto personalizado de texturas. Es posible que desee usar texturas de mosaico genéricas que se puedan obtener en línea o crearlas usted mismo en aplicaciones como **Photoshop**, **Quixel Suite** o **Substance Suite**. 

Antes de empezar a asignar las texturas, es necesario tener en cuenta nuestras coordenadas de texturas de recursos (UVW). Aunque se recomienda asegurarse de que el modelo se ha desajustado al aplicar texturas a un modelo (las texturas no se mostrarán correctamente sin el desajuste de UV adecuado), es importante para nuestros fines si tenemos previsto usar un mapa de **oclusión de ambiente** en nuestro modelo. A diferencia del **sombreador Stingray** de **Maya**, el **material físico** de **3DSMax** no tiene una ranura de textura de **oclusión de ambiente** dedicada. Por lo tanto, aplicaremos el mapa de AO a otra ranura y, para permitir que se use de forma independiente de las otras texturas (por ejemplo, las texturas de mosaico), lo asignaremos a un canal de mapa de UVW propio. 

Comenzaremos por asignar un **modificador para Desajustar UVW** al modelo, tal como se muestra a continuación:

* En el editor de propiedades de los objetos seleccionados, haga clic en la lista Modifier (Modificador) y, en la lista desplegable que se abre, desplácese hacia abajo y seleccione Unwrap UVW (Desajustar UVW). Esta acción aplicará un modificador para Desajustar UVW a nuestro recurso.
![Modificador para desajustar](media/3dsmax/unwrap-modifier.jpg)

* El canal del mapa se establece en uno. Normalmente, el desajuste principal se realiza en el canal del mapa uno. En nuestro caso, el objeto se ha desajustado sin coordenadas de textura (UV) superpuestas.
![UVW desajustado](media/3dsmax/unwrapped-uvw.jpg)

El siguiente paso consiste en crear un segundo canal de mapa UV.

* Cierre el editor de UV si está abierto y, en la sección canal del menú **Edit UV's** (Editar UV) cambie el número de canal a dos. El canal del mapa 2 es el canal esperado para los mapas de oclusión de ambiente. 

* En el cuadro de diálogo **Channel Change Warning** (Advertencia de cambio de canal) que se abre, tendrá la opción de **mover** los UV existentes en el canal 1 al nuevo canal 2 o bien la opción de **abandonar** los UV existentes que crearán un nuevo **desajuste de UV** automáticamente. Seleccione **Abandon** (Abandonar) únicamente si desea crear un nuevo **desajuste de UV** para el mapa de oclusión de ambiente que difiere de los UV del canal del mapa 1 (por ejemplo, si desea usar texturas de mosaico en el canal 1). Para nuestros fines, seleccionaremos **Move** (Mover) los UV desde el canal uno al canal 2, ya que no necesitamos editar el nuevo canal de UV.

>[!NOTE]
>Incluso si ha copiado (**opción Mover**) el desajuste UV del canal del mapa 1 en el canal del mapa 2, puede realizar las modificaciones necesarias en los nuevos UV del canal sin que ello afecte al canal del mapa original.

![Cambio de canal](media/3dsmax/channel-change.jpg)

Con el nuevo canal del mapa creado, podemos volver al material físico en el editor de materiales y empezar a agregarle las texturas. En primer lugar, agregaremos el mapa de oclusión de ambiente (**AO**), ya que hay un paso adicional para permitir que funcione correctamente. Una vez que el mapa de AO está conectado a nuestro material, es necesario indicarle que use el canal del mapa 2.

* Como se mencionó anteriormente, no hay ninguna ranura dedicada para mapas de AO en el **material físico de 3DSMax**. En su lugar, aplicaremos el mapa de AO a la ranura **Rugosidad difusa**.

* En la lista **Generic Maps** (Mapas genéricos) del material físico, haga clic en la ranura **No Map** (Sin mapa) de **Diffuse Roughness** (Rugosidad difusa) y cargue el mapa de AO.

* En las propiedades de las texturas de AO, verá el canal del mapa establecido en **1** de forma predeterminada. Cambie este valor a **2**. Esta acción completa los pasos necesarios para agregar el mapa de oclusión de ambiente.

>[!IMPORTANT]
>Este es un paso importante, especialmente si los UV del canal 2 son diferentes a los del canal 1, ya que la AO no se asignará correctamente con el canal equivocado seleccionado.

![Asignar mapa de AO](media/3dsmax/assign-ao-map.jpg)

Ahora abordaremos la asignación del mapa normal a nuestro material de PBR. Esta acción difiere ligeramente del caso de **Maya** en que el mapa normal no se aplica directamente a la ranura del mapa de rugosidad (no hay ranura de mapa normal en el material físico de **3DSMax** como tal), sino que se agrega a un modificador del mapa normal, que a su vez está conectado a la ranura **normales**.

* En la sección **Special Maps** (Mapas especiales) de las propiedades del material físico (en el editor de materiales), haga clic en la ranura **No Map** (Sin mapa) de **Bump Map** (Mapa de rugosidad). 

* En el explorador de materiales y mapas, busque y haga clic en **Normal Bump** (Rugosidad normal). Esta acción agregará un modificador **Normal Bump** (Rugosidad normal) a nuestro material.

* En el modificador **Normal Bump** (Rugosidad normal), haga clic en la ranura **No Map** (Sin mapa) de **Normal** y busque y cargue el mapa normal.

* Compruebe que el método está establecido en **Tangent** (Tangente) (debería ser así de forma predeterminada) y, si es necesario, alterne **Flip Green (Y)** (Girar verde [Y]).

![Rugosidad normal](media/3dsmax/normal-bump.jpg)
![Cargar mapa normal](media/3dsmax/load-normal-map.jpg)

Con el mapa normal asignado correctamente, podemos seguir con la asignación de las texturas restantes para completar la configuración del material físico. Este proceso es un proceso sencillo sin ninguna configuración especial a tener en cuenta. En la imagen siguiente se muestra el conjunto completo de texturas asignadas al material: ![Todas las texturas](media/3dsmax/all-textures.jpg)

Una vez que los materiales de PBR se han creado y configurado, es importante pensar en la creación de instancias de los objetos de la escena. La creación de instancias de objetos similares en la escena, como tuercas, tornillos, arandelas (básicamente objetos del mismo tipo), puede producir importantes ahorros en cuanto al tamaño del archivo. Las instancias de un objeto maestro pueden tener su propia escala, rotación y transformaciones, por lo que se pueden colocar donde sean necesarios en la escena. En **3D Studio Max**, el proceso de **creación de instancias** es sencillo.

* En la ventanilla principal, seleccione el objeto u objetos que desea exportar.

* Mantenga presionada la tecla **Mayús** y arrastre los recursos hacia arriba con la herramienta transformar (mover). 

* En el cuadro de diálogo **Clone Options** (Opciones de clonación) que se abre, establezca **Object** (Objeto) en **Instance** (Instancia) y haga clic en **OK** (Aceptar). 
![Crear instancia de un objeto](media/3dsmax/instance-object.jpg)

Esta acción creará una instancia del objeto que se puede desplazar, rotar o escalar con independencia de su elemento primario y de otras instancias de este.

>[!IMPORTANT]
>Sin embargo, cualquier cambio que realice en una instancia mientras se encuentre en modo sub-objeto se transmitirá a todas las instancias del objeto, por lo que si está trabajando con componentes de objetos con instancia (vértices, caras de polígono, etc.), asegúrese primero de que desea que los cambios que realice afecten a todas estas instancias. Recuerde que cualquier objeto creado como una instancia se puede convertir en un objeto único en cualquier momento. 

>[!TIP]
>El procedimiento recomendado con respecto a la creación de instancias de la escena es crearlas a medida que avanza, ya que sustituir más tarde las **copias** por los objetos con instancia es sumamente difícil. 

Un punto final a tener en cuenta antes de pasar al proceso de exportación es cómo desea empaquetar la escena o el recurso para compartirlos. Idealmente, si pasa el recurso a un cliente o a un miembro del equipo, querrá que puedan abrir y visualizar el recurso con una cantidad mínima de complicaciones. Por lo tanto, es importante mantener las rutas de acceso de las texturas de los recursos relativas respecto al archivo de la escena. Si las rutas de acceso de las texturas del recurso apuntan a una unidad local o a una ruta o ubicación absoluta, no se cargarán en la escena si se abren en otro equipo, incluso si el archivo **.max** está en la misma carpeta que las texturas. El uso de rutas de acceso relativas para las texturas en 3D Studio Max resuelve este problema y es bastante sencillo.

* En la barra de herramientas principal, vaya a **File** > **Reference** > **Asset Tracking Toggle** (Archivo>Referencia>Activar seguimiento de recursos). 

* En el explorador de seguimiento de recursos que se abre, verá todas o la mayoría de las texturas que ha aplicado a los materiales de PBR en la columna **Maps/Shaders** (Mapas/Sombreadores).

* Junto a ellas, en la columna **Full Path** (Ruta de acceso completa), verá la ruta de acceso del archivo con la ubicación de las texturas, que probablemente sea su ubicación en el equipo local.

* Por último, verá una columna llamada **Status** (Estado). Esta columna indica si se ha encontrado y aplicado una textura determinada a la escena o no, y marcará esta textura con uno de los siguientes términos **OK** (Correcto), **Found** (Encontrado) o **File Missing** (Falta el archivo). Los dos primeros indican que el archivo se ha encontrado y cargado, mientras que el último, obviamente, significa que el rastreador no encontró el archivo.
![Rutas de acceso de las texturas](media/3dsmax/texture-paths.jpg)

Es posible que observe que no se enumeran todas las texturas en el explorador de seguimiento de recursos cuando se abre por primera vez. No es necesario preocuparse por ello, ya que con la ejecución del proceso de búsqueda de rutas una o dos veces, suele encontrar todas las texturas de la escena. El proceso de búsqueda de rutas de acceso es el siguiente: 

* En la ventana del explorador de seguimiento de recursos, pulse **Mayús**+**clic** en la textura superior de la lista **Maps/Shaders** (Mapas/Sombreadores) y, con la tecla Mayús presionada, haga clic en la última textura de la lista. Esta acción seleccionará todas las texturas de la lista. Ahora, las texturas seleccionadas se resaltarán en azul (consulte la imagen anterior).

* Haga clic con el botón derecho en la selección y, en el menú emergente que se abre, seleccione **Set Path** (Establecer la ruta).

* En el cuadro **Specify Asset Path** (Especificar la ruta de acceso del recurso) que se abre, seleccione la ruta de acceso local a las texturas que se muestra, reemplácela por `.\` y haga clic en **OK** (Aceptar). 

* Después de un período de tiempo (que variará en función del número de texturas de la escena y del tamaño de la escena), el explorador de seguimiento de recursos debe resolverlo por sí mismo como se muestra a continuación (consulte la imagen).
![Resolver texturas](media/3dsmax/resolve-textures.jpg)

Observe que la columna **Full Path** (Ruta de acceso completa) ahora está en blanco. Significa que la escena ya no busca las texturas correspondientes en una ubicación específica (absoluta), sino que las encontrará siempre que el archivo max o el archivo FBX relacionado se encuentren en la misma carpeta que las texturas. 

>[!NOTE]
>En ocasiones, es posible que tenga que repetir este proceso un par de veces para buscar y resolver todas las texturas y rutas de acceso. Esto no es nada por lo que deba preocuparse, simplemente repítalo hasta que se tengan en cuenta todos los recursos pertinentes. También se puede dar el caso de que ya no se encuentren algunos archivos. En este caso, es suficiente con seleccionar todos los recursos de la lista y hacer clic en **Remove Missing Paths** (Eliminar las rutas de acceso que faltan) (consulte la imagen anterior).

## <a name="fbx-export"></a>Exportación de FBX

Una vez completado el seguimiento de recursos, ahora podemos pasar a la exportación de FBX. De nuevo, el proceso es sencillo y se puede realizar de dos maneras. 

>[!TIP]
>Es recomendable que, a menos que quiera exportar la escena completa, seleccione solo los recursos necesarios para la exportación. En escenas intensivas en recursos, la exportación puede tardar mucho tiempo, por lo que tiene sentido exportar solo aquello que se necesita.
>
>Es aconsejable que, si ha usado modificadores como **Turbosmooth** u **Open SubDiv**, los contraiga antes de la exportación, ya que pueden causar problemas durante la exportación. Guarde siempre la escena antes de hacerlo. 

* En la escena, seleccione los recursos que desea exportar y, en la barra de herramientas principal, vaya a **File** > **Export** > **Export Selected** (Archivo>Exportar>Exportar la selección).

* En el cuadro de diálogo **Select File to Export** (Seleccionar archivo para exportar), escriba o seleccione el nombre del archivo de salida y, en las opciones de **Save as Type** (Guardar como tipo), seleccione **Autodesk (*.fbx)** . Esta acción abrirá el menú de exportación de FBX. 

* Recuerde que si ha creado instancias en la escena, es importante que la opción **Preserve Instances** (Conservar instancias) esté activada en la configuración de la exportación de FBX. 
![Exportación de FBX](media/3dsmax/fbx-export.jpg)

Recuerde que anteriormente se mencionó que había un par de formas de exportar el archivo. Si la intención al exportar es que el archivo FBX se comparta junto con sus archivos de texturas en una carpeta o directorio, la configuración que se muestra en la imagen siguiente debería aplicarse y funcionar bien. Una vez que haya seleccionado la configuración, haga clic en **OK** (Aceptar).
![Configuración de FBX](media/3dsmax/fbx-settings.jpg)

Sin embargo, si prefiere no compartir carpetas o directorios grandes de texturas junto con el archivo FBX, puede elegir **Embed** (Insertar) las texturas en el archivo FBX. Significa que se agregará el recurso completo, texturas incluidas, a un solo archivo FBX. No obstante, tenga en cuenta que si combina la exportación en un único recurso, el archivo FBX resultante será considerablemente más grande.

>[!IMPORTANT]
>Si el archivo FBX resultante es mayor de 2,4 GB, la versión mínima de la configuración de exportación de FBX (consulte más arriba) debe ser la 2016 o posterior. Esto se debe a que las versiones más recientes tienen compatibilidad con 64 bits y, por tanto, admiten archivos de mayor tamaño.

* En la configuración de exportación de FBX, active la opción **Embed media (Insertar elementos multimedia) y, a continuación, haga clic en **OK** (Aceptar) para exportar con las texturas incluidas. 

Al exportar a FBX mientras usa el material físico, es probable que vea el siguiente elemento emergente de advertencia después de hacer clic en "OK" (Aceptar) en el cuadro de diálogo de exportación: ![Advertencias de la exportación](media/3dsmax/export-warnings.jpg)

Esta advertencia solo informa al usuario de que es posible que el material exportado no sea compatible con otros paquetes de software. Como el material físico es compatible con Azure Remote Rendering, no se preocupe por ello. Simplemente haga clic en **OK** (Aceptar) para completar el proceso y cerrar la ventana.

## <a name="conclusion"></a>Conclusión

En general, este tipo de materiales es más realista, ya que se basa en la física de la luz del mundo real. Crea un efecto envolvente adicional para que la escena parezca ser del mundo real.

## <a name="next-steps"></a>Pasos siguientes

Ahora ya sabe cómo configurar materiales con iluminación avanzada para los objetos de una escena. También sabe cómo exportar los objetos al formato FBX compatible con Azure Remote Rendering. El siguiente paso consiste en convertir el archivo FBX y visualizarlo en Azure Remote Rendering.

>[!div class="nextstepaction"]
>[Inicio rápido: Conversión de un modelo para su representación](../../quickstarts\convert-model.md)