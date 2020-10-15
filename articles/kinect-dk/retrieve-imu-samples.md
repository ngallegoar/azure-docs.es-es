---
title: Recuperación de muestras de IMU de Azure Kinect
description: Aprenda a recuperar muestras de IMU de Azure Kinect mediante el SDK de Azure Kinect.
author: qm13
ms.author: cedmonds
ms.date: 06/26/2019
ms.prod: kinect-dk
ms.topic: conceptual
keywords: kinect, azure, configure, depth, color, RBG, camera, sensor, sdk, IMU, motion sensor, motion, gyroscope, gyro, accelerometer, FPS
ms.openlocfilehash: 649dd5b9de62d43d59e74d53adff1ec7de8dfd32
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276943"
---
# <a name="retrieve-azure-kinect-imu-samples"></a>Recuperación de muestras de IMU de Azure Kinect

El dispositivo Azure Kinect proporciona acceso a las unidades de movimiento de inercia (IMU), que abarcan los tipos de acelerómetro y giroscopio. Para acceder a las muestras de IMU, primero debe abrir y configurar el dispositivo y, luego, capturar los datos de IMU. Para más información, consulte [Búsqueda y apertura del dispositivo](find-then-open-device.md).

Las muestras de IMU se generan con una frecuencia mucho mayor que las imágenes. Las muestras se notifican al host a una velocidad inferior a la que se muestrean. Al esperar una muestra de IMU, es frecuente que varias muestras estén disponibles al mismo tiempo.

Consulte la [especificación de hardware](hardware-specification.md) de Azure Kinect DK para más información sobre la tasa de informes de IMU.

## <a name="configure-and-start-cameras"></a>Configuración e inicio de las cámaras

> [!NOTE]
> Los sensores de IMU solo pueden funcionar cuando las cámaras de color y profundidad están en ejecución. Los sensores de IMU no pueden funcionar solos.

Para iniciar las cámaras, use [k4a_device_start_cameras ()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a).

```C
k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
config.camera_fps = K4A_FRAMES_PER_SECOND_30;
config.color_format = K4A_IMAGE_FORMAT_COLOR_MJPG;
config.color_resolution = K4A_COLOR_RESOLUTION_2160P;

if (K4A_RESULT_SUCCEEDED != k4a_device_start_cameras(device, &config))
{
    printf("Failed to start cameras\n");
    goto Exit;
}

if (K4A_RESULT_SUCCEEDED != k4a_device_start_imu(device))
{
    printf("Failed to start imu\n");
    goto Exit;
}
```

## <a name="access-imu-samples"></a>Acceso a las muestras de IMU

 Cada muestra [k4a_imu_sample_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__imu__sample__t.html#details) contiene una lectura del acelerómetro y del giroscopio capturada casi al mismo tiempo.

Puede obtener las muestras de IMU en el mismo subproceso del que obtiene las capturas de imagen o en subprocesos independientes.

Para recuperar muestras de IMU tan pronto como estén disponibles, puede llamar a [`k4a_device_get_imu_sample()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8e5913b3bb94a453c7143bbd6e399a0e.html#ga8e5913b3bb94a453c7143bbd6e399a0e) en su propio subproceso. La API también tiene suficiente cola interna para permitirle buscar solo muestras después de que se devuelve cada captura de imagen.

Dado que hay alguna cola interna de muestras de IMU, puede usar el siguiente patrón sin descartar ningún dato:

1. Espere una captura, a cualquier velocidad de fotogramas.
2. Procese la captura.
3. Recupere todas las muestras de IMU de la cola.
4. Repita el mismo procedimiento y espere la captura siguiente.

Para recuperar todas las muestras de IMU que ahora están en la cola, puede llamar a [`k4a_device_get_imu_sample()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8e5913b3bb94a453c7143bbd6e399a0e.html#ga8e5913b3bb94a453c7143bbd6e399a0e) con un valor de `timeout_in_ms` de 0 en un bucle hasta que la función devuelva `K4A_WAIT_RESULT_TIMEOUT`. `K4A_WAIT_RESULT_TIMEOUT` indica que no hay ninguna muestra en la cola y que ninguna ha llegado en el tiempo de espera especificado.

## <a name="usage-example"></a>Ejemplo de uso

```C
k4a_imu_sample_t imu_sample;

// Capture a imu sample
switch (k4a_device_get_imu_sample(device, &imu_sample, TIMEOUT_IN_MS))
{
case K4A_WAIT_RESULT_SUCCEEDED:
    break;
case K4A_WAIT_RESULT_TIMEOUT:
    printf("Timed out waiting for a imu sample\n");
    continue;
    break;
case K4A_WAIT_RESULT_FAILED:
    printf("Failed to read a imu sample\n");
    goto Exit;
}

// Access the accelerometer readings
if (imu_sample != NULL)
{
    printf(" | Accelerometer temperature:%.2f x:%.4f y:%.4f z: %.4f\n",
            imu_sample.temperature,
            imu_sample.acc_sample.xyz.x,
            imu_sample.acc_sample.xyz.y,
            imu_sample.acc_sample.xyz.z);
}

```

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo trabajar con muestras de IMU, también puede:
>[!div class="nextstepaction"]
>[Acceder a los datos de entrada del micrófono](access-mics.md)
