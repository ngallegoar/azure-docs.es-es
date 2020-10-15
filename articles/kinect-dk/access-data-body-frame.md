---
title: Acceso a los datos de Azure Kinect DK en un armazón corporal
description: Obtenga información sobre los datos de un armazón corporal y las funciones para tener acceso a esos datos en Azure Kinect DK.
author: qm13
ms.author: quentinm
ms.reviewer: yijwan
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: how-to
keywords: cuerpo, corporal, armazón, azure, kinect, corporal, seguimiento, sugerencias
ms.openlocfilehash: be44f59cb84e99129bf526575293eee69ca64598
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85277046"
---
# <a name="access-data-in-body-frame"></a>Acceso a los datos en el marco del cuerpo

En este artículo se describen los datos contenidos en un armazón corporal y las funciones para tener acceso a esos datos.

Se explican las siguientes funciones:

- [k4abt_frame_get_body_id()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92)
- [k4abt_frame_get_body_index_map()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga0e4f2d0d7e330d444de7070fb1fee4f6.html#ga0e4f2d0d7e330d444de7070fb1fee4f6)
- [k4abt_frame_get_body_skeleton()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gac7032ab06268253538556750775064fb.html#gac7032ab06268253538556750775064fb)
- [k4abt_frame_get_capture()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gad9eef11f6496bbfe997536c374217d9a.html#gad9eef11f6496bbfe997536c374217d9a)
- [k4abt_frame_get_num_bodies()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga29ab088b1a0d1a246bdb5542e21aa3c3.html#ga29ab088b1a0d1a246bdb5542e21aa3c3)
- [k4abt_frame_get_device_timestamp_usec()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga04be7b814b40296cd6b97044ed7283e4.html#ga04be7b814b40296cd6b97044ed7283e4)

## <a name="key-components-of-a-body-frame"></a>Componentes clave de un armazón corporal

Cada armazón corporal contiene una colección de estructuras del cuerpo, un mapa del índice corporal en 2D y la captura de entrada que generó este resultado.

![Componentes del armazón corporal](./media/how-to-guides/body-frame.png)

## <a name="access-the-collection-of-body-structs"></a>Acceso a la colección de estructuras del cuerpo

Es posible que se detecten varios cuerpos en una sola captura. Puede consultar el número de cuerpos mediante una llamada a la función [k4abt_frame_get_num_bodies()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga29ab088b1a0d1a246bdb5542e21aa3c3.html#ga29ab088b1a0d1a246bdb5542e21aa3c3).

```C
size_t num_bodies = k4abt_frame_get_num_bodies(body_frame);
```

Use las funciones [k4abt_frame_get_body_id()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92) y [k4abt_frame_get_body_skeleton()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gac7032ab06268253538556750775064fb.html#gac7032ab06268253538556750775064fb) para recorrer en iteración cada índice corporal para buscar el identificador corporal y la información de posición/orientación de articulaciones.

```C
for (size_t i = 0; i < num_bodies; i++)
{
    k4abt_skeleton_t skeleton;
    k4abt_frame_get_body_skeleton(body_frame, i, &skeleton);
    uint32_t id = k4abt_frame_get_body_id(body_frame, i);
}
```

## <a name="access-the-body-index-map"></a>Acceso al mapa del índice corporal

Utilice la función [k4abt_frame_get_body_index_map()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga0e4f2d0d7e330d444de7070fb1fee4f6.html#ga0e4f2d0d7e330d444de7070fb1fee4f6) para tener acceso al mapa del índice corporal. Consulte el [mapa del índice corporal](body-index-map.md) para obtener una explicación detallada del mapa del índice corporal. Asegúrese de liberar el mapa del índice corporal cuando ya no sea necesario.

```C
k4a_image_t body_index_map = k4abt_frame_get_body_index_map(body_frame);
...  // Do your work with the body index map
k4a_image_release(body_index_map);
```

## <a name="access-the-input-capture"></a>Acceso a la captura de entrada

El seguimiento de personas es una API asincrónica. Es posible que la captura original ya se haya liberado en el momento en que se extrae el resultado. Utilice la función [k4abt_frame_get_capture()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gad9eef11f6496bbfe997536c374217d9a.html#gad9eef11f6496bbfe997536c374217d9a) para consultar la captura de entrada utilizada para generar este resultado de seguimiento corporal. El recuento de referencias para k4a_capture_t aumenta cada vez que se llama a esta función. Use la función [k4a_capture_release()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0ed6f74ec403c3eac1b8ef3afb32cee6.html#ga0ed6f74ec403c3eac1b8ef3afb32cee6) cuando ya no se necesite la captura.

```C
k4a_capture_t input_capture = k4abt_frame_get_capture(body_frame);
... // Do your work with the input capture
k4a_capture_release(input_capture);
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[SDK de seguimiento de personas de Azure Kinect](https://microsoft.github.io/Azure-Kinect-Body-Tracking/)
