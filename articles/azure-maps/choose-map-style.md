---
title: Cambio del estilo del mapa en Azure Maps | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre las funcionalidades relacionadas con el estilo disponibles en el SDK web de Microsoft Azure Maps.
author: philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: b1c5d9b5cd2b6b9bfecf8a0af79699061003eec1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335688"
---
# <a name="change-the-style-of-the-map"></a>Cambio del estilo del mapa

El mapa admite diferentes [opciones de estilo](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions) que se pueden establecer cuando el mapa se está inicializando o más adelante usando la función de Maps `setStyle`. En este artículo se muestra cómo usar estas opciones de estilo para personalizar la apariencia de los mapas. Aprenda a establecer un estilo al cargar un mapa y a establecer un estilo de mapa nuevo mediante el control selector de estilo.

## <a name="set-the-style-options"></a>Establecimiento de las opciones de estilo 

Las opciones de estilo se pueden pasar al mapa cuando este se inicializa o más tarde, al actualizarlo, usando la función de Maps `setStyle`.

```javascript
//Set the style options when creating the map.
var map = new atlas.Map('map', {
    renderWorldCopies: false,
    showBuildingModels: true

    //Additional map options.
};

//Update the style options at anytime using setStyle function.
map.setStyle({
    renderWorldCopies: true,
    showBuildingModels: false
});
```

La siguiente herramienta muestra el modo en que las distintas opciones de estilo cambian la manera en que el mapa se representa. Para ver los edificios en 3D, amplíe mucho una gran urbe. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Opciones de estilo de mapa" src="https://codepen.io/azuremaps/embed/eYNMjPb?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el lápiz <a href='https://codepen.io/azuremaps/pen/eYNMjPb'>Map style options</a> (Opciones de estilo de mapa) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="choose-a-base-map-style"></a>Elección de un estilo de mapa base

Una de las opciones de estilo de mapa más comunes sirve para cambiar el estilo del mapa base. Muchos de los [estilos de mapa admitidos en Azure Maps](supported-map-styles.md) están disponibles en el SDK para web. 

### <a name="set-base-map-style-on-map-load"></a>Establecimiento del estilo de mapa en la carga del mapa


El estilo de mapa se puede especificar al inicializar el mapa, estableciendo la opción `style`. En el código siguiente, la opción `style` del mapa se establece en `grayscale_dark` al inicializarse.

```javascript
var map = new atlas.Map('map', {
    style: 'grayscale_dark',

    //Additiona map options
);
```

<br/>

<iframe height='500' scrolling='no' title='Establecer el estilo al cargar un mapa' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>Setting the style on map load</a> (Establecer el estilo al cargar un mapa) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="update-the-base-map-style"></a>Actualización del estilo de mapa base

 El estilo de mapa se puede actualizar con la función `setStyle` y estableciendo la opción `style` en el estilo de mapa que quiera.

```javascript
map.setStyle({ style: 'satellite' });
```

En el código siguiente, después de cargar una instancia de mapa, el estilo de mapa se actualiza de `road` a `satellite` con la función [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#setstyle-styleoptions-).

<br/>

<iframe height='500' scrolling='no' title='Actualizar el estilo' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>Updating the style</a> (Actualizar el estilo) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="add-the-style-picker"></a>Adición del selector de estilos

El control de selector de estilos proporciona un botón con panel flotante fácil de utilizar que el usuario final puede emplear para cambiar el estilo del mapa. El selector de estilos tiene dos opciones de diseño diferentes. El selector de estilos usa de forma predeterminada el diseño `icons` y muestra todo el estilo de mapa como una fila horizontal de iconos. 

<center>

![Diseño de iconos del selector de estilos](media/choose-map-style/style-picker-icon-layout.png)</center>

La segunda opción de diseño se denomina `list` y muestra una lista desplazable de estilos de mapa.  

<center>

![Diseño de lista del selector de estilos](media/choose-map-style/style-picker-list-layout.png)</center>


En el siguiente código se muestra cómo crear una instancia del control de selector de estilos y agregarla a la esquina superior derecha del mapa. El selector de estilos está configurado en un estilo oscuro y muestra algunos estilos de mapa seleccionados por medio del nivel de lista.

```javascript
map.controls.add(new atlas.control.StyleControl({
    mapStyles: ['road', 'night', 'grayscale_dark', 'grayscale_light'],
    layout: 'list',
    style: 'dark'
}), {
    position: 'top-right'
}); 
```

Con el siguiente código se agrega al mapa un control de selector de estilos con la configuración predeterminada para que el usuario pueda cambiar fácilmente entre los distintos estilos de mapa. Alterne entre los estilos de mapa mediante el control de estilo de mapa situado cerca de la esquina superior derecha.

<br/>

<iframe height='500' scrolling='no' title='Agregar el selector de estilos' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Adding the style picker</a> (Agregar el selector de estilos) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> De forma predeterminada, el control selector de estilo muestra todos los estilos disponibles cuando se usa el plan de tarifa S0 de Azure Maps. Si desea reducir el número de estilos de esta lista, pase una matriz de los estilos que desea que aparezcan en la lista a la opción `mapStyle` del selector de estilo. Si está utilizando S1 y desea mostrar todos los estilos disponibles, establezca la opción `mapStyles` del selector de estilo en `"all"`.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [StyleOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions)

> [!div class="nextstepaction"]
> [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol)

> [!div class="nextstepaction"]
> [StyleControlOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.stylecontroloptions)

Adición de controles a los mapas:

> [!div class="nextstepaction"]
> [Incorporación de controles de mapa](map-add-controls.md)

> [!div class="nextstepaction"]
> [Adición de un anclaje](map-add-pin.md)
