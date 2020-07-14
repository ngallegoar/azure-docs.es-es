---
title: Uso de las funciones de calibración de Azure Kinect
description: Aprenda a usar las funciones de calibración de Azure Kinect DK.
author: cdedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, azure, sensor, sdk, coordinate system, calibration, functions, camera, intrinsic, extrinsic, project, unproject, transformation, rgb-d, point cloud
ms.openlocfilehash: a501765222154bde4ed35e878a9d18042893fca2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276942"
---
# <a name="use-azure-kinect-calibration-functions"></a>Uso de las funciones de calibración de Azure Kinect

Las funciones de calibración permiten transformar los puntos entre los sistemas de coordenadas de cada sensor del dispositivo Azure Kinect. Las aplicaciones que requieren la conversión de imágenes completas pueden aprovechar las operaciones aceleradas disponibles en las [funciones de transformación](use-image-transformation.md).

## <a name="retrieve-calibration-data"></a>Recuperación de los datos de calibración

Es necesario recuperar la calibración del dispositivo para realizar las transformaciones del sistema de coordenadas. Los datos de calibración se almacenan en el tipo de datos [k4a_calibration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__calibration__t.html#details). Se obtienen del dispositivo a través de la función [k4a_device_get_calibration ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4e43940d8d8db48da266c7a7842c8d78.html#ga4e43940d8d8db48da266c7a7842c8d78). Los datos de calibración no solo son específicos de cada dispositivo, sino también del modo de funcionamiento de las cámaras. Por lo tanto [k4a_device_get_calibration ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4e43940d8d8db48da266c7a7842c8d78.html#ga4e43940d8d8db48da266c7a7842c8d78) requiere los parámetros `depth_mode` y `color_resolution` como entrada.

### <a name="opencv-compatibility"></a>Compatibilidad con OpenCV

Los parámetros de calibración son compatibles con [OpenCV](https://opencv.org/). Para más información sobre cada parámetro de calibración de la cámara, consulte también la [documentación de OpenCV](https://docs.opencv.org/3.2.0/d9/d0c/group__calib3d.html#gga7041b2a9c8f9f8ee93a2796981bc5546a204766e24f2e413e7a7c9f8b9e93f16c). Consulte también el [ejemplo de compatibilidad de OpenCV](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/opencv_compatibility) del SDK que muestra la conversión entre el tipo [k4a_calibration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__calibration__t.html#details) y las estructuras de datos OpenCV correspondientes.

## <a name="coordinate-transformation-functions"></a>Funciones de transformación de coordenadas

En la figura siguiente se muestran los diferentes sistemas de coordenadas de Azure Kinect, así como las funciones para realizar las conversiones entre ellos. Para que la figura sea sencilla, se omiten los sistemas de coordenadas 3D del giroscopio y del acelerómetro.

   ![Transformación de las coordenadas](./media/how-to-guides/coordinate-transformation.png)

Comentario sobre la distorsión de la lente: las coordenadas 2D siempre hacen referencia a la imagen distorsionada en el SDK. En el [ejemplo de eliminación de distorsión](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/undistort) del SDK se muestra la eliminación de distorsión de la imagen. En general, los puntos 3D nunca se verán afectados por la distorsión de la lente.

### <a name="convert-between-3d-coordinate-systems"></a>Conversión entre sistemas de coordenadas 3D

La función [k4a_calibration_3d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaacd1eab997ef964b5e436afa5295726e.html#gaacd1eab997ef964b5e436afa5295726e) convierte un punto 3D del sistema de coordenadas de origen en un punto 3D del sistema de coordenadas de destino mediante la calibración extrínseca de la cámara. El origen y el destino se pueden establecer en cualquiera de los cuatro sistemas de coordenadas 3D, es decir, cámara de color, cámara de profundidad, giroscopio o acelerómetro. Si el origen y el destino son idénticos, el punto 3D de entrada sin modificar se devuelve como salida.

### <a name="convert-between-2d-and-3d-coordinate-systems"></a>Conversión entre sistemas de coordenadas 2D y 3D

La función [k4a_calibration_3d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ed8b51d727425caa942aab190fc2ba9.html#ga2ed8b51d727425caa942aab190fc2ba9) convierte un punto 3D del sistema de coordenadas de origen en una coordenada de píxeles 2D de la cámara de destino. Esta función se conoce a menudo como función Project. Aunque el origen se puede establecer en cualquiera de los cuatro sistemas de coordenadas 3D, el destino debe ser la cámara de profundidad o la de color. Si el origen y el destino son diferentes, el punto 3D de entrada se pasa al sistema de coordenadas 3D de la cámara de destino mediante [k4a_calibration_3d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaacd1eab997ef964b5e436afa5295726e.html#gaacd1eab997ef964b5e436afa5295726e). Una vez que el punto 3D se representa en el sistema de coordenadas de la cámara de destino, las coordenadas de píxeles 2D correspondientes se calculan mediante la calibración intrínseca de la cámara de destino. Si un punto 3D no se encuentra dentro del área visible de la cámara de destino, el valor válido se establece en 0.

La función [k4a_calibration_2d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga664602bdb48dab38117a6c1d14b880de.html#ga664602bdb48dab38117a6c1d14b880de) convierte una coordenada de píxeles 2D de la cámara de origen en un punto 3D del sistema de coordenadas de la cámara de destino. El origen debe ser una cámara de color o de profundidad. El destino se puede establecer en cualquiera de los cuatro sistemas de coordenadas 3D. Además de la coordenada de píxeles 2D, se requiere el valor de profundidad del píxel (en milímetros) de la imagen de la cámara de origen como entrada a la función; una manera de obtener el valor de profundidad de la geometría de la cámara de color es usar la función [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514). La función calcula el rayo 3D que lleva del punto focal de la cámara de origen a la coordenada de píxeles especificada mediante la calibración intrínseca de la cámara de origen. El valor de profundidad se usa entonces para buscar la ubicación exacta del punto 3D en este rayo. Esta operación a menudo se conoce como función Unproject. Si las cámaras de origen y destino son diferentes, la función pasa el punto 3D al sistema de coordenadas del destino a través de [k4a_calibration_3d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaacd1eab997ef964b5e436afa5295726e.html#gaacd1eab997ef964b5e436afa5295726e). Si una coordenada de píxeles 2D no se encuentra dentro del área visible de la cámara de origen, el valor válido se establece en 0.

### <a name="converting-between-2d-coordinate-systems"></a>Conversión entre sistemas de coordenadas 2D

La función [k4a_calibration_2d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) convierte una coordenada de píxeles 2D de la cámara de origen en una coordenada de píxeles 2D de la cámara de destino. El origen y el destino deben estar establecidos en la cámara de color o de profundidad. La función requiere el valor de profundidad del píxel (en milímetros) de la imagen de la cámara de origen como entrada; una manera de obtener el valor de profundidad de la geometría de la cámara de color es usar la función [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514). Esta función llama a [k4a_calibration_2d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga664602bdb48dab38117a6c1d14b880de.html#ga664602bdb48dab38117a6c1d14b880de) para realizar la conversión a un punto 3D del sistema de la cámara de origen. Luego, llama a [k4a_calibration_3d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ed8b51d727425caa942aab190fc2ba9.html#ga2ed8b51d727425caa942aab190fc2ba9) para realizar la conversión a una coordenada de píxeles 2D de la imagen de la cámara de destino. El valor válido se establece en 0, si [k4a_calibration_2d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga664602bdb48dab38117a6c1d14b880de.html#ga664602bdb48dab38117a6c1d14b880ded) o [k4a_calibration_3d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ed8b51d727425caa942aab190fc2ba9.html#ga2ed8b51d727425caa942aab190fc2ba9) devuelven un resultado no válido.

## <a name="related-samples"></a>Muestras relacionadas

- [Ejemplo de compatibilidad con OpenCV](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/opencv_compatibility)
- [Ejemplo de eliminación de distorsión](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/undistort)
- [Ejemplo de nube de punto rápido](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/fastpointcloud)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya sabe cómo calibrar la cámara, también puede aprender a:
>[!div class="nextstepaction"]
>[Capturar la sincronización del dispositivo](capture-device-synchronization.md).

También puede revisar:

[Sistemas de coordenadas](coordinate-systems.md)
