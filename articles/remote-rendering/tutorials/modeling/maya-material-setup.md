---
title: Configuración de materiales con representación física en Maya
description: En este artículo se explica cómo configurar materiales con representación física en Maya y exportarlos en formato FBX.
author: muxanickms
ms.author: misams
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: 5579994b0746a2de4b0f2ca927027ac709940024
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977413"
---
# <a name="tutorial-setting-up-physically-based-rendering-materials-in-maya"></a>Tutorial: Configuración de materiales con representación física en Maya

## <a name="overview"></a>Información general
En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Asignar materiales con el modelo de iluminación avanzada a los objetos de la escena.
> * Administrar la creación de instancias de objetos y materiales.
> * Exportar una escena a formato FBX y las opciones importantes que se pueden seleccionar.

La creación de [materiales con representación física (PBR)](../../overview/features/pbr-materials.md) en `Maya` es una tarea relativamente sencilla, en gran parte parecida a configurar PBR en otras aplicaciones de creación de contenido como `3DS Max`. El siguiente tutorial es una guía para la configuración básica del sombreador de PBR y la exportación a FBX de proyectos de ARR. 

La escena de ejemplo de este tutorial contiene una serie de objetos `Polygon Box` a los que se han asignado diversos materiales: madera, metal, metal pintado, plástico y caucho. En términos generales, cada material contiene todas las texturas siguientes, o la mayoría de ellas. 

* `Albedo`, es el mapa de colores de materiales, también denominado `Diffuse` o `BaseColor`.
* `Metalness`, que determina si un material es metálico y qué partes son metálicas. 
* `Roughness`, que determina lo regular o irregular que es una superficie, lo cual afecta a la nitidez o a la borrosidad de los reflejos y los resaltados en una superficie.
* `Normals`, que agrega detalles a una superficie, por ejemplo, las picaduras y las abolladuras en una superficie de metal o el grano de la madera sin tener que agregar más polígonos.
* `Ambient Occlusion`, que se usa para agregar matices suaves y sombras de contacto a un modelo. Se trata de un mapa de escala de grises que indica qué áreas de un modelo reciben una iluminación completa (blanco) o una sombra completa (negro). 

## <a name="prerequisites"></a>Requisitos previos
* `Autodesk Maya 2017` o más reciente

## <a name="setting-up-materials-in-the-scene"></a>Configuración de los materiales de la escena
En Maya, este es proceso para configurar un material de PBR:

Para empezar, como verá en la escena de ejemplo, hemos creado varios objetos Box, cada uno de los cuales representa un tipo de material diferente. Tenga en cuenta que, tal y como se muestra en la imagen siguiente, a cada uno de estos objetos se le ha asignado su propio nombre adecuado. 

> Antes de empezar a crear recursos para Azure Remote Rendering (ARR), es importante advertir que se usan metros para la medición y la dirección de arriba es el eje Y. Por lo tanto, se recomienda establecer las unidades de la escena de Maya en metros. Además, es aconsejable que al exportar se establezcan las unidades en metros en la configuración de exportación de FBX. 

> [!TIP]
Es una buena idea designar los recursos de modelos de forma adecuada, normalmente con la parte o el tipo de material pertinentes, ya que los nombres facilitan la navegación por las escenas con muchos objetos.

![Nombres de objetos](media/object-names.jpg)

Una vez que haya creado o adquirido las texturas (en función de sus necesidades, podría crear texturas únicas para un modelo en aplicaciones de creación de texturas como `Quixel Suite`, `Photoshop` o `Substance Suite`, o puede que use texturas de mosaicos genéricas de otros orígenes), puede aplicarlas a su modelo de la manera siguiente:

* En la ventanilla de la escena, seleccione el modelo o la geometría y haga clic con el botón derecho en ellos. En el menú que aparece, haga clic en `Assign New Material`.
* En las opciones de `Assign New Material`, vaya a `Maya`>`Stingray PBS`. Esta acción asignará un material de PBR al modelo. 

En `Maya 2020`, hay varios sombreadores de PBR disponibles: `Maya Standard Surface`, `Arnold Standard Surface` y `Stingray PBR`. `Maya Standard Surface Shader` todavía no se puede exportar a través de `FBX plugin 2020`, mientras que `Arnold Standard Surface Shader` se puede exportar con archivos FBX. En la mayoría de los demás aspectos, es idéntico a `Maya Standard Surface Shader` y es análogo a `Physical Material` en `3D Studio Max`.

**`The Stingray PBR Shader`** es compatible con muchas otras aplicaciones y coincide en gran medida con los requisitos de `ARR`; además, se admite desde `Maya 2017`. También es conveniente que este tipo de material se visualice en la ventanilla, de manera similar a lo que se visualizará más adelante en ARR.

![Material "Stingray"](media/stingray-material.jpg)

Una vez que el material se ha asignado al recurso y se ha designado correctamente, ahora puede pasar a asignar las diversas texturas. En las siguientes imágenes se detallan todos los tipos de texturas que encajan con el material de PBR. El material `Stingray PBR` permite seleccionar los atributos que se pueden activar, así que antes de poder realizar la acción de `plug in` en los mapas de textura, debe activar los atributos pertinentes: 

![Configuración de los materiales](media/material-setup.jpg)

> [!TIP]
Es aconsejable designar los materiales de forma adecuada, teniendo en cuenta su uso o tipo. Un material que se vaya a usar en solo una parte podría designarse por esa parte, mientras que uno que se pueda usar en un número más amplio de áreas se puede designar por sus propiedades o su tipo.

Asigne las texturas como se indica a continuación:

![Configuración de las texturas](media/texture-setup.jpg)

Una vez que los materiales de PBR se han creado y configurado, es importante pensar en la [creación de instancias de los objetos](../../how-tos/conversion/configure-model-conversion.md#instancing) de la escena. La creación de instancias de objetos similares en la escena, como tuercas, tornillos, arandelas (básicamente objetos del mismo tipo), puede producir importantes ahorros en cuanto al tamaño del archivo. Las instancias de un objeto maestro pueden tener su propia escala, rotación y transformaciones, por lo que se pueden colocar donde sean necesarios en la escena. En Maya, el proceso de creación de instancias es sencillo.

* En el menú `Edit`, vaya a `Duplicate Special` y abra `Options`. 
* En las opciones `Duplicate Special`, cambie `Geometry Type` de `Copy` a `Instance`. 
* Haga clic en `Duplicate Special`

![Instanciación](media/instancing.jpg)

Esta acción creará una instancia del objeto que se puede desplazar, rotar o escalar con independencia de su elemento primario y de otras instancias de este. 
>Sin embargo, cualquier cambio que realice en una instancia mientras se encuentre en modo de componente se transmitirá a todas las instancias del objeto, por lo que si está trabajando con componentes de objetos con instancia (vértices, caras de polígono, etc.), asegúrese primero de que desea que los cambios que realice afecten a todas estas instancias.

En la escena de ejemplo, se ha creado una instancia de cada objeto Box individual. Esta acción tendrá importancia cuando se exporte la escena a formato FBX.

![Información general de la escena](media/scene-overview.jpg)

>El procedimiento recomendado con respecto a la creación de instancias de la escena es crearlas a medida que avanza, ya que sustituir más tarde las copias por los objetos con instancia es sumamente difícil. 

## <a name="fbx-export-process"></a>Proceso de exportación de FBX

Ahora podemos pasar a la exportación de FBX de la escena o de los recursos de la escena. Por lo general, al exportar recursos, tiene sentido seleccionar solamente los objetos o recursos que prefiera de la escena. Si tiene 100 objetos en una escena, pero solo quiere usar 30 de ellos, no hay necesidad de exportar toda la escena. Por lo tanto, a menos que quiera hacerlo, realice la selección y vaya a:

* `File` > `Export Selection` y, en el cuadro de diálogo de exportación, diríjase a la parte inferior y establezca `Files of Type` en `FBX Export`. Esta ventana expondrá la configuración de exportación de FBX. La configuración principal de la exportación de FBX se resalta en rojo en la imagen siguiente.

![Exportación de FBX](media/FBX-exporting.jpg)

En función de sus requisitos (por ejemplo, puede que quiera enviar un recurso a un cliente, pero no quiera enviar un gran número de archivos de textura con el recurso), puede optar por insertar las texturas dentro del archivo FBX exportado. Esta opción significa que solo tiene un archivo para empaquetar, pero aumentará significativamente el tamaño del recurso de FBX. Para habilitar la opción para insertar texturas, active la opción `Embed Media`, como se muestra a continuación.

> [!TIP]
> Tenga en cuenta que, en este caso, el nombre del archivo refleja esta condición. Este es un buen procedimiento para mantener el seguimiento de los recursos. 

Cuando haya terminado de configurar los valores de exportación, haga clic en el botón "Export Selection" (Exportar selección) en la parte inferior derecha.

![Inserción de elementos multimedia](media/embedding-media.jpg)

## <a name="conclusion"></a>Conclusión

En general, este tipo de materiales es más realista, ya que se basa en la física de la luz del mundo real. Crea el efecto envolvente adicional de que la escena existe en el mundo real.

## <a name="next-steps"></a>Pasos siguientes

Ahora conoce la funcionalidad más importante para configurar materiales con iluminación avanzada de los objetos de una escena y exportarla al formato FBX compatible con ARR. El paso siguiente sería convertir el archivo FBX y visualizarlo en ARR.

> [!div class="nextstepaction"]
> [Inicio rápido: Conversión de un modelo para su representación](../../quickstarts\convert-model.md)