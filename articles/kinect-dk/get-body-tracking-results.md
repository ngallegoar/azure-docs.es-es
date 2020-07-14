---
title: Obtención de resultados del seguimiento de personas en Azure Kinect
description: Obtenga información sobre cómo obtener los resultados del seguimiento corporal mediante el SDK de seguimiento de personas en Azure Kinect.
author: qm13
ms.prod: kinect-dk
ms.author: quentinm
ms.reviewer: yijwan
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, azure, sensor, sdk, cuerpo, corporal, personas seguimiento, articulación
ms.openlocfilehash: 1b62022242144d5db51455a32ac04b67c3e5dd7a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277038"
---
# <a name="get-body-tracking-results"></a>Obtención del resultados del seguimiento de personas

El SDK de seguimiento de personas usa un objeto de seguimiento corporal para procesar las capturas de Azure Kinect DK y genera resultados de seguimiento corporal. También mantiene el estado global del seguimiento, las colas de procesamiento y la cola de salida. El uso del seguimiento de personas tiene tres pasos:

- Crear un seguimiento
- Capturar imágenes de profundidad e IR con Azure Kinect DK
- Poner en cola la captura y sacar los resultados

## <a name="create-a-tracker"></a>Crear un seguimiento


El primer paso para usar el seguimiento corporal es crear un seguimiento, lo que requiere pasar la estructura [k4a_calibration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__calibration__t.html) de calibración del sensor. La calibración del sensor se puede consultar mediante la función [k4a_device_get_calibration()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4e43940d8d8db48da266c7a7842c8d78.html#ga4e43940d8d8db48da266c7a7842c8d78) del SDK del Sensor de Azure Kinect.

```C
k4a_calibration_t sensor_calibration;
if (K4A_RESULT_SUCCEEDED != k4a_device_get_calibration(device, device_config.depth_mode, K4A_COLOR_RESOLUTION_OFF, &sensor_calibration))
{
    printf("Get depth camera calibration failed!\n");
    return 0;
}

k4abt_tracker_t tracker = NULL;
k4abt_tracker_configuration_t tracker_config = K4ABT_TRACKER_CONFIG_DEFAULT;
if (K4A_RESULT_SUCCEEDED != k4abt_tracker_create(&sensor_calibration, tracker_config, &tracker))
{
    printf("Body tracker initialization failed!\n");
    return 0;
}
```

## <a name="capture-depth-and-ir-images"></a>Capturar imágenes de profundidad e IR

La captura de imágenes mediante Azure Kinect DK se trata en la página [Recuperación de imágenes](retrieve-images.md).

>[!NOTE]
> Se recomiendan los modos `K4A_DEPTH_MODE_NFOV_UNBINNED` o `K4A_DEPTH_MODE_WFOV_2X2BINNED` para obtener el mejor rendimiento y precisión. No use los modos `K4A_DEPTH_MODE_OFF` ni `K4A_DEPTH_MODE_PASSIVE_IR`.

Los modos de Azure Kinect DK admitidos se describen en la [especificación de hardware](hardware-specification.md) de Azure Kinect DK y en las enumeraciones de [k4a_depth_mode_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___enumerations_ga3507ee60c1ffe1909096e2080dd2a05d.html#ga3507ee60c1ffe1909096e2080dd2a05d).

```C
// Capture a depth frame
switch (k4a_device_get_capture(device, &capture, TIMEOUT_IN_MS))
{
case K4A_WAIT_RESULT_SUCCEEDED:
    break;
case K4A_WAIT_RESULT_TIMEOUT:
    printf("Timed out waiting for a capture\n");
    continue;
    break;
case K4A_WAIT_RESULT_FAILED:
    printf("Failed to read a capture\n");
    goto Exit;
}
```

## <a name="enqueue-the-capture-and-pop-the-results"></a>Poner en cola la captura y sacar los resultados

El rastreador mantiene internamente una cola de entrada y una cola de salida para procesar asincrónicamente las capturas de Azure Kinect DK de forma más eficaz. Use la función [k4abt_tracker_enqueue_capture()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga093becd9bb4a63f5f4d56f58097a7b1e.html#ga093becd9bb4a63f5f4d56f58097a7b1e) para agregar una nueva captura a la cola de entrada. Use la función [k4abt_tracker_pop_result()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gaaf446fb1579cbbe0b6af824ee0a7458b.html#gaaf446fb1579cbbe0b6af824ee0a7458b) para sacar un resultado de la cola de salida. El uso del valor del tiempo de espera depende de la aplicación y controla el tiempo de espera de la cola.

### <a name="real-time-processing"></a>Procesamiento en tiempo real
Use este patrón para las aplicaciones de un único subproceso que necesiten resultados en tiempo real y puedan admitir fotogramas descartados. El ejemplo `simple_3d_viewer` que se encuentra en [GitHub Azure-Kinect-Samples](https://github.com/microsoft/Azure-Kinect-Samples) es una muestra de procesamiento en tiempo real.

```C
k4a_wait_result_t queue_capture_result = k4abt_tracker_enqueue_capture(tracker, sensor_capture, 0);
k4a_capture_release(sensor_capture); // Remember to release the sensor capture once you finish using it
if (queue_capture_result == K4A_WAIT_RESULT_FAILED)
{
    printf("Error! Adding capture to tracker process queue failed!\n");
    break;
}

k4abt_frame_t body_frame = NULL;
k4a_wait_result_t pop_frame_result = k4abt_tracker_pop_result(tracker, &body_frame, 0);
if (pop_frame_result == K4A_WAIT_RESULT_SUCCEEDED)
{
    // Successfully popped the body tracking result. Start your processing
    ...

    k4abt_frame_release(body_frame); // Remember to release the body frame once you finish using it
}
```

### <a name="synchronous-processing"></a>Procesamiento sincrónico
Use este patrón para las aplicaciones que no necesiten resultados en tiempo real o no puedan admitir fotogramas descartados.

El rendimiento del procesamiento puede ser limitado.

El ejemplo `simple_sample.exe` que se encuentra en [GitHub Azure-Kinect-Samples](https://github.com/microsoft/Azure-Kinect-Samples) es una muestra de procesamiento sincrónico.

```C
k4a_wait_result_t queue_capture_result = k4abt_tracker_enqueue_capture(tracker, sensor_capture, K4A_WAIT_INFINITE);
k4a_capture_release(sensor_capture); // Remember to release the sensor capture once you finish using it
if (queue_capture_result != K4A_WAIT_RESULT_SUCCEEDED)
{
    // It should never hit timeout or error when K4A_WAIT_INFINITE is set.
    printf("Error! Adding capture to tracker process queue failed!\n");
    break;
}

k4abt_frame_t body_frame = NULL;
k4a_wait_result_t pop_frame_result = k4abt_tracker_pop_result(tracker, &body_frame, K4A_WAIT_INFINITE);
if (pop_frame_result != K4A_WAIT_RESULT_SUCCEEDED)
{
    // It should never hit timeout or error when K4A_WAIT_INFINITE is set.
    printf("Error! Popping body tracking result failed!\n");
    break;
}
// Successfully popped the body tracking result. Start your processing
...

k4abt_frame_release(body_frame); // Remember to release the body frame once you finish using it
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Acceso a los datos en el armazón corporal](access-data-body-frame.md)
