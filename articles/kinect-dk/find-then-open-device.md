---
title: Búsqueda y apertura del dispositivo Azure Kinect
description: Aprenda a buscar y abrir un dispositivo Azure Kinect mediante el SDK del sensor Azure Kinect.
author: cedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, azure, sensor, sdk, profundidad, rgb, dispositivo, buscar, abrir
ms.openlocfilehash: 67fc93b924d5d663bb43098969c54d1975bd5895
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276919"
---
# <a name="find-then-open-the-azure-kinect-device"></a>Búsqueda y apertura del dispositivo Azure Kinect

En este artículo se explica cómo buscar y luego abrir Azure Kinect DK. En el artículo se explica cómo controlar el caso de que haya varios dispositivos conectados al equipo.

También puede ver el [Ejemplo de enumeración del SDK](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/enumerate), donde se muestra cómo usar las funciones de este artículo.

Se explican las siguientes funciones:
 * [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc)
 * [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113)
 * [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22)
 * [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423)

## <a name="discover-the-number-of-connected-devices"></a>Detección del número de dispositivos conectados

En primer lugar, obtenga el número de dispositivos Azure Kinect conectados actualmente mediante [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc).

```C
uint32_t device_count = k4a_device_get_installed_count();

printf("Found %d connected devices:\n", device_count);
```

## <a name="open-a-device"></a>Apertura de un dispositivo

Para obtener información sobre un dispositivo o para leer datos de él, primero debe abrir un identificador para el dispositivo mediante [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113).

```C
k4a_device_t device = NULL;

for (uint8_t deviceIndex = 0; deviceIndex < device_count; deviceIndex++)
{
    if (K4A_RESULT_SUCCEEDED != k4a_device_open(deviceIndex, &device))
    {
        printf("%d: Failed to open device\n", deviceIndex);
        continue;
    }

    ...

    k4a_device_close(device);
}
```

El parámetro `index` de [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) indica qué dispositivo se va a abrir si hay más de uno conectado. Si solo espera que haya un dispositivo conectado, puede pasar un argumento de `K4A_DEVICE_DEFAULT` o 0 para indicar el primer dispositivo.

Siempre que abra un dispositivo, debe llamar a [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423) cuando haya terminado de usar el identificador. No se puede abrir ningún otro identificador para el mismo dispositivo hasta que se haya cerrado el identificador.

## <a name="identify-a-specific-device"></a>Identificación de un dispositivo concreto

El orden en que los dispositivos se enumeran por índice no cambia hasta que se asocian o desasocian. Para identificar un dispositivo físico, debe usar el número de serie del dispositivo.

Para leer el número de serie del dispositivo, use la función [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22) después de abrir un identificador.

En este ejemplo se muestra cómo asignar la cantidad adecuada de memoria para almacenar el número de serie.

```C
char *serial_number = NULL;
size_t serial_number_length = 0;

if (K4A_BUFFER_RESULT_TOO_SMALL != k4a_device_get_serialnum(device, NULL, &serial_number_length))
{
    printf("%d: Failed to get serial number length\n", deviceIndex);
    k4a_device_close(device);
    device = NULL;
    continue;
}

serial_number = malloc(serial_number_length);
if (serial_number == NULL)
{
    printf("%d: Failed to allocate memory for serial number (%zu bytes)\n", deviceIndex, serial_number_length);
    k4a_device_close(device);
    device = NULL;
    continue;
}

if (K4A_BUFFER_RESULT_SUCCEEDED != k4a_device_get_serialnum(device, serial_number, &serial_number_length))
{
    printf("%d: Failed to get serial number\n", deviceIndex);
    free(serial_number);
    serial_number = NULL;
    k4a_device_close(device);
    device = NULL;
    continue;
}

printf("%d: Device \"%s\"\n", deviceIndex, serial_number);
```

## <a name="open-the-default-device"></a>Apertura del dispositivo predeterminado

En la mayoría de las aplicaciones, solo hay un dispositivo Azure Kinect DK asociado al mismo equipo. Si solo necesita conectarse al dispositivo esperado único, puede llamar a [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) con `index` de `K4A_DEVICE_DEFAULT` para abrir el primer dispositivo.

```C
k4a_device_t device = NULL;
uint32_t device_count = k4a_device_get_installed_count();

if (device_count != 1)
{
    printf("Unexpected number of devices found (%d)\n", device_count);
    goto Exit;
}

if (K4A_RESULT_SUCCEEDED != k4a_device_open(K4A_DEVICE_DEFAULT, &device))
{
    printf("Failed to open device\n");
    goto Exit;
}
```

## <a name="next-steps"></a>Pasos siguientes

>[!div class="nextstepaction"]
>[Recuperación de imágenes](retrieve-images.md)

>[!div class="nextstepaction"]
>[Recuperación de muestras del IMU](retrieve-imu-samples.md)

