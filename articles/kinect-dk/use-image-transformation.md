---
title: Uso de transformaciones de imágenes del SDK del sensor Azure Kinect
description: Aprenda a usar las funciones de transformación de imágenes del SDK del sensor Azure Kinect.
author: cdedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.topic: conceptual
ms.date: 06/26/2019
keywords: kinect, azure, sensor, sdk, sistema de coordenadas, calibración, proyectar, desproyectar, transformación, rgb-d, nube de puntos
ms.openlocfilehash: df7f2aa13c0e9c0241494e96e720b30f3ff1d8a5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276906"
---
# <a name="use-azure-kinect-sensor-sdk-image-transformations"></a>Uso de transformaciones de imágenes del SDK del sensor Azure Kinect

Siga las funciones específicas para usar y transformar imágenes entre sistemas de cámaras coordinados de Azure Kinect DK.

## <a name="k4a_transformation-functions"></a>Funciones k4a_transformation

 Todas las funciones que tienen el prefijo *k4a_transformation* operan en imágenes completas. Requieren el identificador de transformación [k4a_transformation_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__transformation__t.html) obtenido a través de [k4a_transformation_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga853a1a5b6d521bbdf523a69e890c4f10.html#ga853a1a5b6d521bbdf523a69e890c4f10) y se desasignan por medio de [k4a_transformation_destroy()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7d3ecaae66f26c1a89da9042b1bc6d44.html#ga7d3ecaae66f26c1a89da9042b1bc6d44). También puede ver el [Ejemplo de transformación](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/transformation) del SDK, donde se muestra cómo usar las tres funciones de este tema.

Se explican las siguientes funciones:

* [`k4a_transformation_depth_image_to_color_camera()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514)
* [`k4a_transformation_depth_image_to_color_camera_custom()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gac00dd00e7612a86382e3d0a130f276bb.html#gac00dd00e7612a86382e3d0a130f276bb)
* [`k4a_transformation_color_image_to_depth_camera()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf3a941f07bb0185cd7a72699a648fc29.html#gaf3a941f07bb0185cd7a72699a648fc29)
* [`k4a_transformation_depth_image_to_point_cloud()`](
https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70)

### <a name="k4a_transformation_depth_image_to_color_camera"></a>k4a_transformation_depth_image_to_color_camera

#### <a name="overview"></a>Información general

 La función [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) transforma el mapa de profundidad desde el punto de vista de la cámara de profundidad al punto de vista de la cámara de color. Esta función está diseñada para generar las denominadas imágenes RGB-D, donde D representa un canal de imagen adicional que graba el valor de profundidad. Como se ve en la ilustración siguiente, la imagen de color y la salida de [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) parece que se hubieran obtenido desde el mismo punto de vista, es decir, el punto de vista de la cámara de color.

   ![Transformación de imagen](./media/how-to-guides/image-transformation.png)

#### <a name="implementation"></a>Implementación

 Esta función de transformación es más compleja que una simple llamada a [k4a_calibration_2d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) para cada píxel. Deforma una malla de triángulo de la geometría de la cámara de profundidad en la geometría de la cámara de color. La malla de triángulo se usa para evitar que se generen huecos en la imagen de profundidad transformada. Un búfer Z garantiza que las oclusiones se controlen correctamente. La aceleración de GPU está habilitada de forma predeterminada para esta función.

#### <a name="parameters"></a>Parámetros

 Los parámetros de entrada son el identificador de transformación y una imagen de profundidad. La resolución de la imagen de profundidad debe coincidir con el ```depth_mode``` especificado durante la creación del identificador de transformación. Por ejemplo, si el identificador de transformación se ha creado mediante el modo ```K4A_DEPTH_MODE_WFOV_UNBINNED``` 1024 x 1024, la resolución de la imagen de profundidad debe ser de 1024 x 1024 píxeles. El resultado es una imagen de profundidad transformada que el usuario debe asignar por medio de una llamada a [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef). La resolución de la imagen de profundidad transformada debe coincidir con el ```color_resolution``` especificado durante la creación del identificador de transformación. Por ejemplo, si la resolución de color se ha establecido en `K4A_COLOR_RESOLUTION_1080P`, la resolución de la imagen de salida debe ser de 1920 x 1080 píxeles. El intervalo de la imagen de salida se establece en `width * sizeof(uint16_t)`, ya que la imagen almacena valores de profundidad de 16 bits.

### <a name="k4a_transformation_depth_image_to_color_camera_custom"></a>k4a_transformation_depth_image_to_color_camera_custom

#### <a name="overview"></a>Información general

 La función [k4a_transformation_depth_image_to_color_camera_custom()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gac00dd00e7612a86382e3d0a130f276bb.html#gac00dd00e7612a86382e3d0a130f276bb) transforma el mapa de profundidad y una imagen personalizada desde el punto de vista de la cámara de profundidad al punto de vista de la cámara de color. Como extensión de [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514), esta función está diseñada para generar una imagen personalizada correspondiente para la que cada píxel coincide con las coordenadas de píxeles correspondientes de la cámara de color adicionales a la imagen de profundidad transformada.

#### <a name="implementation"></a>Implementación

 Esta función de transformación genera la imagen de profundidad transformada de la misma manera que [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514). Para transformar la imagen personalizada, esta función proporciona opciones de uso de la interpolación lineal o la interpolación por el vecino más cercano. Con la interpolación lineal se podrían crear nuevos valores en la imagen personalizada transformada. Con la interpolación por el vecino más cercano se evita que los valores que no están presentes en la imagen original aparezcan en la imagen de salida, pero la imagen es menos homogénea. La imagen personalizada debe ser de 8 o 16 bits de canal único. La aceleración de GPU está habilitada de forma predeterminada para esta función.

#### <a name="parameters"></a>Parámetros

 Los parámetros de entrada son el identificador de transformación, una imagen de profundidad, una imagen personalizada y el tipo de interpolación. La resolución de la imagen de profundidad y la imagen personalizada debe coincidir con el `depth_mode` especificado durante la creación del identificador de transformación. Por ejemplo, si el identificador de transformación se ha creado mediante el modo `K4A_DEPTH_MODE_WFOV_UNBINNED` 1024 x 1024, la resolución de la imagen de profundidad y la imagen personalizada debe ser de 1024 x 1024 píxeles. `interpolation_type` debe ser `K4A_TRANSFORMATION_INTERPOLATION_TYPE_LINEAR` o `K4A_TRANSFORMATION_INTERPOLATION_TYPE_NEAREST`. El resultado es una imagen de profundidad transformada y una imagen personalizada transformada que el usuario debe asignar por medio de una llamada a [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef). La resolución de la imagen de profundidad transformada y la imagen personalizada transformada debe coincidir con el `color_resolution` especificado durante la creación del identificador de transformación. Por ejemplo, si la resolución de color se ha establecido en `K4A_COLOR_RESOLUTION_1080P`, la resolución de la imagen de salida debe ser de 1920 x 1080 píxeles. El intervalo de la imagen de profundidad de salida se establece en `width * sizeof(uint16_t)`, ya que la imagen almacena valores de profundidad de 16 bits. La imagen personalizada de entrada y la imagen personalizada transformada deben tener el formato `K4A_IMAGE_FORMAT_CUSTOM8` o `K4A_IMAGE_FORMAT_CUSTOM16` y el intervalo de imagen correspondiente debe establecerse en consecuencia. 

### <a name="k4a_transformation_color_image_to_depth_camera"></a>k4a_transformation_color_image_to_depth_camera

#### <a name="overview"></a>Información general

 La función [k4a_transformation_color_image_to_depth_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf3a941f07bb0185cd7a72699a648fc29.html#gaf3a941f07bb0185cd7a72699a648fc29) transforma la imagen de color desde el punto de vista de la cámara de color al punto de vista de la cámara de profundidad (vea la ilustración anterior). Se puede usar para generar imágenes RGB-D.

#### <a name="implementation"></a>Implementación

 En cada píxel del mapa de profundidad, la función usa el valor de profundidad del píxel para calcular la coordenada de subpíxeles correspondiente de la imagen de color. Luego se busca el valor de color de la imagen de color en esta coordenada. La interpolación bilineal se realiza en la imagen de color para obtener el valor de color en la precisión de subpíxel. Un píxel que no tiene una lectura de profundidad asociada se asigna a un valor BGRA de `[0,0,0,0]` en la imagen de salida. La aceleración de GPU está habilitada de forma predeterminada para esta función. Como este método produce huecos en la imagen de color transformada y no controla las oclusiones, se recomienda usar la función [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) en su lugar.

#### <a name="parameters"></a>Parámetros

Los parámetros de entrada son el identificador de transformación, una imagen de profundidad y una imagen de color. Las resoluciones de las imágenes de profundidad y de color deben coincidir con el depth_mode y la color_resolution especificados durante la creación del identificador de transformación. El resultado es una imagen de color transformada que el usuario debe asignar por medio de una llamada a [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef). La resolución de la imagen de color transformada debe coincidir con la depth_resolution especificada durante la creación del identificador de transformación. La imagen de salida almacena cuatro valores de 8 bits que representan el BGRA de cada píxel. Por lo tanto, el intervalo de la imagen es ```width * 4 * sizeof(uint8_t)```. El orden de los datos es píxel intercalado, es decir, valor azul - píxel 0, valor verde - píxel 0, valor rojo - píxel 0, valor alfa - píxel 0, valor azul - píxel 1 y así sucesivamente.

### <a name="k4a_transformation_depth_image_to_point_cloud"></a>k4a_transformation_depth_image_to_point_cloud

#### <a name="overview"></a>Información general

La función [k4a_transformation_depth_image_to_point_cloud()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70) convierte un mapa de profundidad 2D tomado por una cámara en una nube de puntos 3D en el sistema de coordenadas de la misma cámara. Por lo tanto, la cámara puede ser la de profundidad o de color.

#### <a name="implementation"></a>Implementación

 La función proporciona resultados equivalentes a la ejecución de [k4a_calibration_2d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) para cada píxel, pero es más eficaz a nivel computacional. Cuando se llama a [k4a_transformation_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga853a1a5b6d521bbdf523a69e890c4f10.html#ga853a1a5b6d521bbdf523a69e890c4f10), se calcula previamente lo que se denomina una tabla de búsqueda xy que almacena factores de escala x e y para cada píxel de la imagen. Cuando se llama a [k4a_transformation_depth_image_to_point_cloud()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70), se obtiene la coordenada X 3D de un píxel al multiplicar el factor de escala x del píxel por la coordenada Z del píxel. Del mismo modo, la coordenada Y 3D se calcula al multiplicar por el factor de escala y. En el [Ejemplo FastPointCloud](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/fastpointcloud) del SDK se muestra cómo se calcula la tabla xy. Los usuarios pueden seguir el código del ejemplo para implementar su propia versión de esta función, por ejemplo, para acelerar su canalización de GPU.

#### <a name="parameters"></a>Parámetros

 Los parámetros de entrada son el identificador de transformación, un especificador de cámara y una imagen de profundidad. Si el especificador de cámara está establecido en la profundidad, la resolución de la imagen de profundidad debe coincidir con el depth_mode especificado durante la creación del identificador de transformación. De lo contrario, si el especificador está establecido en la cámara de color, la resolución debe coincidir con la resolución de la color_resolution elegida. El parámetro de salida es una imagen XYZ que el usuario debe asignar por medio de una llamada a [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef). La resolución de imagen XYZ debe coincidir con la resolución de la asignación de profundidad de entrada. Se almacenan tres valores de coordenadas de 16 bits con signo en milímetros para cada píxel. Por lo tanto, el intervalo de imagen XYZ se establece en `width * 3 * sizeof(int16_t)`. El orden de los datos es píxel intercalado, es decir, coordenada X – píxel 0, coordenada Y – píxel 0, coordenada Z – píxel 0, coordenada X – píxel 1y así sucesivamente. Si un píxel no se puede convertir en 3D, la función asigna los valores `[0,0,0]` al píxel.

## <a name="samples"></a>Ejemplos

[Ejemplo de transformación](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/transformation)

## <a name="next-steps"></a>Pasos siguientes

Ya sabe cómo usar las funciones de transformación de imágenes del SDK del sensor Azure Kinect y también puede obtener información sobre

>[!div class="nextstepaction"]
>[Funciones de calibración del SDK del sensor Azure Kinect](use-calibration-functions.md)

También puede revisar

[Sistemas de coordenadas](coordinate-systems.md)
