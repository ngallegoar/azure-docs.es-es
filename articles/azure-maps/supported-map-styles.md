---
title: Estilos de mapa admitidos | Microsoft Azure Maps
description: En este artículo aprenderá sobre los distintos estilos de representación de mapas compatibles con Microsoft Azure Maps.
author: philmea
ms.author: philmea
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2eafe3c16a89723d55ec52fde785e9ec69e45e0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334038"
---
# <a name="azure-maps-supported-map-styles"></a>Estilos de mapa admitidos en Azure Maps
Azure Maps es compatible con varios estilos de mapa integrados, tal como se describe a continuación.

## <a name="road"></a>carreteras
Un mapa de **carreteras** es un mapa estándar que muestra las carreteras y características naturales y artificiales con las etiquetas de esas características.

![estilo de mapa de carreteras](./media/supported-map-styles/road.png)

**API correspondientes:**
* [Imagen de mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="blank-and-blank_accessible"></a>blank y blank_accessible

Los estilos de mapa **blank** y **blank_accessible** proporcionan un lienzo en blanco en el que visualizar los datos. El estilo **blank_accessible** seguirá proporcionando actualizaciones del lector de pantalla con detalles de la ubicación del mapa, aunque no se muestre el mapa base.

> [!Note]
> En el SDK web puede cambiar el color de fondo del mapa estableciendo el estilo CSS `background-color` del elemento DIV del mapa.

**API correspondientes:**
* Control de mapa de SDK web

## <a name="satellite"></a>satélite 
El mapa estilo **satélite** es una combinación de imágenes aéreas o por satélite.

![estilo de mapa de mosaico satelital](./media/supported-map-styles/satellite.png)

**API correspondientes:**
* [Mosaico de satélite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="satellite_road_labels"></a>satellite_road_labels
Este estilo de mapa es un híbrido de carreteras y etiquetas que se superpone a imágenes aéreas o por satélite.

![estilo de mapa satellite_road_labels](./media/supported-map-styles/satellite-road-labels.png)

**API correspondientes:**
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="grayscale_dark"></a>grayscale_dark
**escala de grises oscuros** es una versión oscura del estilo de mapa de carreteras.

![estilo de mapa gray_scale](./media/supported-map-styles/grayscale-dark.png)

**API correspondientes:**
* [Imagen de mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa de SDK web 
* Control de mapa de Android


## <a name="grayscale_light"></a>grayscale_light
**escala de grises claros** es una versión clara del estilo de mapa de carreteras.

![estilo de mapa de escala de grises claros](./media/supported-map-styles/grayscale-light.png)

**API correspondientes:**
* Control de mapa de SDK web
* Control de mapa de Android


## <a name="night"></a>noche
**noche** es una versión oscura del estilo de mapa de carreteras con símbolos y carreteras en color.

![estilo de mapa de noche](./media/supported-map-styles/night.png)

**API correspondientes:**
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="road_shaded_relief"></a>road_shaded_relief
**relieve sombreado del camino** es un estilo principal de Azure Maps completado con los contornos de la tierra.

![estilo de mapa de relieve sombreado](./media/supported-map-styles/shaded-relief.png)

**API correspondientes:**
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="high_contrast_dark"></a>high_contrast_dark

**high_contrast_dark** es un estilo de mapa oscuro con un contraste mayor que el de los demás estilos.

![estilo de mapa oscuro de contraste alto](./media/supported-map-styles/high-contrast-dark.png)

**API correspondientes:**
* Control de mapa de SDK web

## <a name="next-steps"></a>Pasos siguientes

Aprenda a establecer un estilo de mapa en Azure Maps:

> [!div class="nextstepaction"]
> [Elección de un estilo de mapa](https://docs.microsoft.com/azure/azure-maps/choose-map-style)
