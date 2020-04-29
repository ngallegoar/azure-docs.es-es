---
title: Plantillas de imagen en el SDK web de Azure Maps | Microsoft Azure Maps
description: En este artículo aprenderá a usar plantillas de imagen con marcadores HTML y varias capas en el SDK web de Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 8/6/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: ee8e8ee4ca64de0390b6fa34e36fb4d06348a8ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80804816"
---
# <a name="how-to-use-image-templates"></a>Uso de plantillas de imagen

Las imágenes se pueden usar con los marcadores HTML y varias capas dentro del SDK web de Azure Maps:

 - Las capas de símbolos pueden representar puntos en el mapa con un icono de imagen. Los símbolos también se pueden representar a lo largo de una ruta de acceso de líneas.
 - Las capas de polígono se pueden representar con una imagen de patrón de relleno. 
 - Los marcadores HTML pueden representar puntos mediante imágenes y otros elementos HTML.

Con el fin de garantizar un buen rendimiento con las capas, estas cargue las imágenes en el recurso de sprite de imagen de mapa antes de la representación. De forma predeterminada, el objeto [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions) de SymbolLayer carga previamente un par de imágenes de marcador en algunos colores en el sprite de imagen de mapa. Estas imágenes de marcador y mucho más están disponibles como plantillas SVG. Se pueden usar para crear imágenes con escalas personalizadas o como color principal y secundario del cliente. En total se proporcionan 42 plantillas de imagen: 27 iconos de símbolos y 15 patrones de relleno de polígonos.

Se pueden agregar plantillas de imagen a los recursos de sprite de imagen de mapa mediante la función `map.imageSprite.createFromTemplate`. Esta función permite pasar hasta cinco parámetros;

```javascript
createFromTemplate(id: string, templateName: string, color?: string, secondaryColor?: string, scale?: number): Promise<void>
```

`id` es un identificador único que usted crea. `id` se asigna a la imagen cuando se agrega al sprite de imagen del mapa. Use este identificador en las capas para especificar el recurso de imagen que se va a representar. `templateName` especifica la plantilla de imagen que se va a usar. La opción `color` establece el color principal de la imagen y las opciones `secondaryColor` establecen el color secundario de la imagen. La opción `scale` escala la plantilla de imagen antes de aplicarla al sprite de imagen. Cuando la imagen se aplica al sprite de imagen, se convierte en un PNG. Para garantizar una representación nítida, es mejor escalar verticalmente la plantilla de imagen antes de agregarla al sprite que hacerlo en una capa.

Esta función carga de forma asincrónica la imagen en el sprite de imagen. Por lo tanto, devuelve una promesa que puede esperar que esta función realice.

En el código siguiente se muestra cómo crear una imagen a partir de una de las plantillas integradas y utilizarla con una capa de símbolos.

```javascript
map.imageSprite.createFromTemplate('myTemplatedIcon', 'marker-flat', 'teal', '#fff').then(function () {

    //Add a symbol layer that uses the custom created icon.
    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            image: 'myTemplatedIcon'
        }
    }));
});
```

## <a name="use-an-image-template-with-a-symbol-layer"></a>Uso de una plantilla de imagen con una capa de símbolos

Cuando se carga una plantilla de imagen en el sprite de imagen de mapa, se puede representar como un símbolo en una capa de símbolos haciendo referencia al identificador de recurso de imagen en la opción `image` de `iconOptions`.

En el ejemplo siguiente se representa una capa de símbolos mediante la plantilla de imagen `marker-flat` con un color principal verde azulado y un color secundario blanco. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Capa de símbolos con la plantilla de iconos integrada" src="//codepen.io/azuremaps/embed/VoQMPp/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) de una <a href='https://codepen.io/azuremaps/pen/VoQMPp/'>capa de símbolos con plantilla de iconos integrada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="use-an-image-template-along-a-lines-path"></a>Uso de una plantilla de imagen a lo largo de una ruta de acceso de líneas

Cuando se carga una plantilla de imagen en el sprite de imagen de mapa, se puede representar a lo largo de la ruta de acceso de una línea mediante la adición de un objeto LineString a un origen de datos y con una capa de símbolos con una opción `lineSpacing` y haciendo referencia al identificador del recurso de imagen en la opción `image` de `iconOptions`. 

En el ejemplo siguiente se representa una línea rosa en el mapa y se usa una capa de símbolos mediante la plantilla de imagen `car` con un color principal azul Dodger y un color secundario blanco. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Capa de línea con la plantilla de iconos integrada" src="//codepen.io/azuremaps/embed/KOQvJe/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) de una <a href='https://codepen.io/azuremaps/pen/KOQvJe/'>capa de línea con plantilla de iconos integrada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Si la plantilla de imagen apunta hacia arriba, establezca la opción de icono `rotation` de la capa de símbolos en 90 si desea que apunte con la misma dirección que la línea.

## <a name="use-an-image-template-with-a-polygon-layer"></a>Uso de una plantilla de imagen con una capa de polígono

Cuando se carga una plantilla de imagen en el sprite de imagen de mapa, se puede representar como un patrón de relleno en una capa de polígono haciendo referencia al identificador de recurso de imagen en la opción `fillPattern` de la capa.

En el ejemplo siguiente se representa una capa de polígono mediante la plantilla de imagen `dot` con un color principal rojo y un color secundario transparente.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Polígono de relleno con la plantilla de iconos integrada" src="//codepen.io/azuremaps/embed/WVMEmz/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) de un <a href='https://codepen.io/azuremaps/pen/WVMEmz/'>polígono de relleno con plantilla de iconos integrada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Establecer el color secundario de los patrones de relleno facilita la visualización del mapa subyacente y seguirá proporcionando el patrón principal. 

## <a name="use-an-image-template-with-an-html-marker"></a>Uso de una plantilla de imagen con un marcador HTML

Se puede recuperar una plantilla de imagen con la función `altas.getImageTemplate` y usarse como contenido de un marcador HTML. La plantilla se puede pasar a la opción del marcador `htmlContent` y, a continuación, personalizarse con las opciones `color`, `secondaryColor` y `text`.

En el ejemplo siguiente se usa la plantilla `marker-arrow` con un color principal rojo, un color secundario rosa y un valor de texto de "00".

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Marcador HTML con plantilla de iconos integrada" src="//codepen.io/azuremaps/embed/EqQvzq/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) de un <a href='https://codepen.io/azuremaps/pen/EqQvzq/'>marcador HTML con plantilla de iconos integrada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="create-custom-reusable-templates"></a>Creación de plantillas reutilizables personalizadas

Si la aplicación usa el mismo icono con iconos diferentes o si va a crear un módulo que agregue plantillas de imagen adicionales, puede agregar y recuperar fácilmente estos iconos desde el SDK web de Azure Maps. Utilice las siguientes funciones estáticas en el espacio de nombres `atlas`.

| Nombre | Tipo de valor devuelto | Descripción | 
|-|-|-|
| `addImageTemplate(templateName: string, template: string, override: boolean)` | | Agrega una plantilla de imagen SVG personalizada al espacio de nombres atlas. |
|  `getImageTemplate(templateName: string, scale?: number)`| string | Recupera una plantilla de SVG por nombre. |
| `getAllImageTemplateNames()` | string[] |  Recupera una plantilla de SVG por nombre. |

Las plantillas de imagen SVG admiten los siguientes valores del marcador de posición:

| Marcador de posición | Descripción |
|-|-|
| `{color}` | El color principal. | 
| `{secondaryColor}` | El color secundario. | 
| `{scale}` | La imagen SVG se convierte en una imagen PNG cuando se agrega al sprite de imagen de mapa. Este marcador de posición se puede usar para escalar una plantilla antes de que se convierta para asegurarse de que se representa claramente. | 
| `{text}` | La ubicación para representar texto cuando se usa con un marcador HTML. |

En el ejemplo siguiente se muestra cómo tomar una plantilla SVG y agregarla al SDK web de Azure Maps como plantilla de icono reutilizable. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Adición de una plantilla de icono personalizada al espacio de nombres atlas" src="//codepen.io/azuremaps/embed/NQyvEX/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) para <a href='https://codepen.io/azuremaps/pen/NQyvEX/'>agregar una plantilla de icono personalizada al espacio de nombres atlas</a> de Azure Maps(<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="list-of-image-templates"></a>Lista de plantillas de imagen

En esta tabla se enumeran todas las plantillas de imagen disponibles actualmente en el SDK web de Azure Maps. El nombre de la plantilla se encuentra encima de cada imagen. De forma predeterminada, el color principal es el azul y el secundario es el blanco. Para que el color secundario sea más fácil de ver en un fondo blanco, las siguientes imágenes tienen el color secundario establecido en negro.

**Plantillas de iconos de símbolos**

|||||
|:-:|:-:|:-:|:-:|
| marker | marker-thick | marker-circle | marker-flat |
|![marker icon](./media/image-templates/marker.png)|![marker-thick icon](./media/image-templates/marker-thick.png)|![marker-circle icon](./media/image-templates/marker-circle.png)|![marker-flat icon](./media/image-templates/marker-flat.png)|
||||
| marker-square | marker-square-cluster | marker-arrow | marker-ball-pin | 
|![marker-square icon](./media/image-templates/marker-square.png)|![marker-square-cluster icon](./media/image-templates/marker-square-cluster.png)|![marker-arrow icon](./media/image-templates/marker-arrow.png)|![marker-ball-pin icon](./media/image-templates/marker-ball-pin.png)|
||||
| marker-square-rounded | marker-square-rounded-cluster | Marca | flag-triangle |
| ![marker-square-rounded icon](./media/image-templates/marker-square-rounded.png) | ![marker-square-rounded-cluster icon](./media/image-templates/marker-square-rounded-cluster.png) | ![flag icon](./media/image-templates/flag.png) | ![flag-triangle icon](./media/image-templates/flag-triangle.png) |
||||
| triangle | triangle-thick | triangle-arrow-up | triangle-arrow-left |
| ![triangle icon](./media/image-templates/triangle.png) | ![triangle-thick icon](./media/image-templates/triangle-thick.png) | ![triangle-arrow-up icon](./media/image-templates/triangle-arrow-up.png) | ![triangle-arrow-left icon](./media/image-templates/triangle-arrow-left.png) |
||||
| hexagon | hexagon-thick | hexagon-rounded | hexagon-rounded-thick |
| ![icono de hexágono](./media/image-templates/hexagon.png) | ![hexagon-thick icon](./media/image-templates/hexagon-thick.png) | ![hexagon-rounded icon](./media/image-templates/hexagon-rounded.png) | ![hexagon-rounded-thick icon](./media/image-templates/hexagon-rounded-thick.png) |
||||
| pin | pin-round | rounded-square | rounded-square-thick |
| ![pin icon](./media/image-templates/pin.png) | ![pin-round icon](./media/image-templates/pin-round.png) | ![rounded-square icon](./media/image-templates/rounded-square.png) | ![rounded-square-thick icon](./media/image-templates/rounded-square-thick.png) |
||||
| arrow-up | arrow-up-thin | automóvil ||
| ![arrow-up icon](./media/image-templates/arrow-up.png) | ![arrow-up-thin icon](./media/image-templates/arrow-up-thin.png) | ![car icon](./media/image-templates/car.png) | |

**Plantillas de patrón de relleno poligonal**

|||||
|:-:|:-:|:-:|:-:|
| checker | checker-rotated | circles | circles-spaced |
| ![checker icon](./media/image-templates/checker.png) | ![checker-rotated icon](./media/image-templates/checker-rotated.png) | ![circles icon](./media/image-templates/circles.png) | ![circles-spaced icon](./media/image-templates/circles-spaced.png) |
|||||
| diagonal-lines-up | diagonal-lines-down | diagonal-stripes-up | diagonal-stripes-down |
| ![diagonal-lines-up icon](./media/image-templates/diagonal-lines-up.png) | ![diagonal-lines-down icon](./media/image-templates/diagonal-lines-down.png) | ![diagonal-stripes-up icon](./media/image-templates/diagonal-stripes-up.png) | ![diagonal-stripes-down icon](./media/image-templates/diagonal-stripes-down.png) |
|||||
| grid-lines | rotated-grid-lines | rotated-grid-stripes | x-fill |
| ![grid-lines icon](./media/image-templates/grid-lines.png) | ![rotated-grid-lines icon](./media/image-templates/rotated-grid-lines.png) | ![rotated-grid-stripes icon](./media/image-templates/rotated-grid-stripes.png) | ![x-fill icon](./media/image-templates/x-fill.png) |
|||||
| zig-zag | zig-zag-vertical | dots |  |
| ![zig-zag icon](./media/image-templates/zig-zag.png) | ![zig-zag-vertical icon](./media/image-templates/zig-zag-vertical.png) | ![dots icon](./media/image-templates/dots.png) | |

## <a name="try-it-now-tool"></a>Pruebe ahora la herramienta

Con la siguiente herramienta, puede representar las distintas plantillas de imagen integradas de varias maneras y personalizar la escala y los colores principales y secundarios.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Opciones de plantilla de iconos" src="//codepen.io/azuremaps/embed/NQyaaO/?height=500&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) de las <a href='https://codepen.io/azuremaps/pen/NQyaaO/'>opciones de plantilla de icono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [ImageSpriteManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagespritemanager)

> [!div class="nextstepaction"]
> [Espacio de nombres atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-maps-typescript-latest#functions
)

Consulte los artículos siguientes para obtener más ejemplos de código donde se pueden usar las plantillas de imagen:

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](map-add-pin.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](map-add-shape.md)

> [!div class="nextstepaction"]
> [Adición de marcadores HTML](map-add-bubble-layer.md)
