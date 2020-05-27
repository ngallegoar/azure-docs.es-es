---
title: Implementación de estilos dinámicos para planos interiores de Azure Maps Creator
description: Aprenda a implementar estilos dinámicos para planos interiores de Creator
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 6edc114a2d69dfe8f1e6e5d3c0a2d4af26dbad67
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596604"
---
# <a name="implement-dynamic-styling-for-creator-indoor-maps"></a>Implementación de estilos dinámicos para planos interiores de Creator

El [servicio de estado de características](https://docs.microsoft.com/rest/api/maps/featurestate/featurestate) de Azure Maps Creator le permite aplicar estilos basados en las propiedades dinámicas de las características de datos de los planos interiores.  Por ejemplo, puede representar las salas de reuniones de la instalación con un color específico para reflejar el estado de ocupación. En este artículo, le mostraremos cómo representar dinámicamente las características de los mapas interiores con el [servicio de estado de características](https://docs.microsoft.com/rest/api/maps/featurestate/featurestate) y el [módulo web de interiores](how-to-use-indoor-module.md).

## <a name="prerequisites"></a>Prerrequisitos

1. [Cree una cuenta de Azure Maps](quick-demo-map-app.md#create-an-account-with-azure-maps)
2. [Obtenga una clave de suscripción principal](quick-demo-map-app.md#get-the-primary-key-for-your-account), también conocida como clave principal o clave de suscripción.
3. [Cree un recurso de Creator](how-to-manage-creator.md)
4. Descargue el [paquete de dibujos de ejemplo](https://github.com/Azure-Samples/am-creator-indoor-data-examples).
5. [Cree un plano interior](tutorial-creator-indoor-maps.md) para obtener un `tilesetId` y un `statesetId`.
6. Cree una aplicación web siguiendo los pasos descritos en [Cómo usar el módulo de planos interiores](how-to-use-indoor-module.md).

En este tutorial se usa la aplicación [Postman](https://www.postman.com/), pero puede elegir un entorno de desarrollo de API diferente.

## <a name="implement-dynamic-styling"></a>Implementación de un estilo dinámico

Cuando haya completado los requisitos previos, debería tener una aplicación web simple configurada con su clave de suscripción, `tilesetId`y `statesetId`.

### <a name="select-features"></a>Selección de características

Para implementar los estilos dinámicos, se debe hacer referencia a una característica, como una sala de reuniones o conferencias, por su `id` de característica. Usará el `id` de característica para actualizar la propiedad dinámica o el *estado* de esa característica. Para ver las características definidas en un conjunto de datos, puede usar uno de los métodos siguientes:

* API de WFS (Web Feature Service). Los conjuntos de datos se pueden consultar mediante la API de WFS. WFS sigue las características de la API de Open Geospatial Consortium. La API de WFS es útil para consultar características dentro de un conjunto de datos. Por ejemplo, puede usar WFS para buscar todas las salas de reuniones de tamaño medio de una instalación y una planta determinadas.

* Implemente código personalizado que permita a un usuario seleccionar características en un mapa mediante la aplicación web. En este artículo, usaremos esta opción.  

El script siguiente implementa el evento de clic del mouse. El código recupera el `id` característica según el punto en el que se hizo clic. En la aplicación, puede insertar el código debajo del bloque de código del administrador de interiores. Ejecute la aplicación y compruebe la consola para obtener el `id` característica del punto en el que hizo clic.

```javascript
/* Upon a mouse click, log the feature properties to the browser's console. */
map.events.add("click", function(e){

    var features = map.layers.getRenderedShapes(e.position, "indoor")

    var result = features.reduce(function (ids, feature) {
        if (feature.layer.id == "indoor_unit_office") {
            console.log(feature);
        }
    }, []);
});
```

El tutorial [Creación de un plano interior](tutorial-creator-indoor-maps.md) configuró el conjunto de estados de características para aceptar las actualizaciones de estado de `occupancy`.

En la siguiente sección, estableceremos el *estado* de ocupación de la oficina `UNIT26` en `true`. mientras que la oficina `UNIT27` se establecerá en `false`.

### <a name="set-occupancy-status"></a>Establecimiento del estado de ocupación

 Ahora actualizaremos el estado de las dos oficinas, `UNIT26` y `UNIT27`:

1. En la aplicación Postman, seleccione**New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un **nombre de solicitud** y seleccione una colección. Haga clic en **Guardar**

2. Use la [API de estados de actualización de características](https://docs.microsoft.com/rest/api/maps/featurestate/updatestatespreview) para actualizar el estado. Pase el identificador del conjunto de estados y `UNIT26` para una de las dos unidades. Anexe la clave de suscripción de Azure Maps. Esta es la URL de una solicitud **POST** para actualizar el estado:

    ```http
    https://atlas.microsoft.com/featureState/state?api-version=1.0&statesetID={statesetId}&featureID=UNIT26&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

3. En el **encabezado** de la solicitud **POST**, establezca `Content-Type` en `application/json`. En el **cuerpo** de la solicitud **POST**, escriba el siguiente JSON con las actualizaciones de características. La actualización se guardará solo si la marca de tiempo enviada es posterior a la marca de tiempo usada en las solicitudes de actualización de estado de características anteriores para el mismo `ID` de característica. Pase el `keyName` "ocupado" para actualizar su valor.

    ```json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": true,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

4. Vuelva a seguir los pasos 2 y 3 con `UNIT27`, con el siguiente JSON.

    ``` json
    {
        "states": [
            {
                "keyName": "occupied",
                "value": false,
                "eventTimestamp": "2019-11-14T17:10:20"
            }
        ]
    }
    ```

### <a name="visualize-dynamic-styles-on-a-map"></a>Visualización de estilos dinámicos en un mapa

La aplicación web que abrió anteriormente en un explorador debería reflejar ahora el estado actualizado de las características del mapa. `UNIT27`(151) debería aparecer en verde y `UNIT26`(157) debería aparecer en rojo.

![Sala libre en verde y sala ocupada en rojo](./media/indoor-map-dynamic-styling/room-state.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea:

> [!div class="nextstepaction"]
> [Uso de Creator para mapas interiores](creator-indoor-maps.md)

Consulte las referencias de las API mencionadas en este artículo:

> [!div class="nextstepaction"]
> [Data Upload](creator-indoor-maps.md#upload-a-drawing-package)

> [!div class="nextstepaction"]
> [Conversión de datos](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Dataset](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Conjunto de mosaicos](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Conjunto de estados de características](creator-indoor-maps.md#feature-statesets)

> [!div class="nextstepaction"]
> [Servicio WFS](creator-indoor-maps.md#web-feature-service-api)

