---
title: Problemas conocidos y solución de problemas de Azure Kinect
description: Obtenga información sobre algunos de los problemas conocidos y sugerencias para solucionar problemas al usar el SDK del sensor con Azure Kinect DK.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: solución de problemas, actualización, error, Kinect, comentarios, recuperación, registro, sugerencias
ms.openlocfilehash: 2db7b17ffc8b6595107dbc52fae719aa9f2d15db
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85277094"
---
# <a name="azure-kinect-known-issues-and-troubleshooting"></a>Problemas conocidos y solución de problemas de Azure Kinect

Esta página contiene problemas conocidos y sugerencias para solucionar problemas al usar el SDK del sensor con Azure Kinect DK. Consulte también las [páginas de soporte técnico del producto](https://aka.ms/kinectsupport) para solucionar los problemas específicos del hardware del producto.

## <a name="known-issues"></a>Problemas conocidos

- Problemas de compatibilidad con los controladores de host USB de ASMedia (por ejemplo, el conjunto de chips ASM1142)
  - En algunos casos, usar el controlador USB de Microsoft puede desbloquearlos
  - Muchos equipos también tienen controladores de host alternativos y cambiar al puerto USB3 puede ayudar

Para conocer más problemas relacionados con el SDK de sensor, consulte las [incidencias de GitHub.](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/issues)

## <a name="collecting-logs"></a>Recopilación de registros

El registro de K4A.dll se habilita a través de variables de entorno. De forma predeterminada, el registro se envía a stdout y solo se generan los errores y los mensajes críticos. Esta configuración se puede modificar para que el registro se envíe a un archivo. También se puede ajustar el nivel de detalle según sea necesario. A continuación, se muestra un ejemplo de cómo habilitar en Windows el envío de registros a un archivo denominado k4a.log y capturar los mensajes de advertencia y de nivel superior.

1. `set K4A_ENABLE_LOG_TO_A_FILE=k4a.log`
2. `set K4A_LOG_LEVEL=w`
3. Ejecute el escenario desde el símbolo del sistema (por ejemplo, iniciar el visor)
4. Vaya al archivo k4a.log y compártalo.

Para obtener más información, consulte el siguiente fragmento del archivo de encabezado:

```console
/**
* environment variables
* K4A_ENABLE_LOG_TO_A_FILE =
*    0    - completely disable logging to a file
*    log\custom.log - log all messages to the path and file specified - must end in '.log' to
*                     be considered a valid entry
*    ** When enabled this takes precedence over the value of K4A_ENABLE_LOG_TO_STDOUT
*
* K4A_ENABLE_LOG_TO_STDOUT =
*    0    - disable logging to stdout
*    all else  - log all messages to stdout
*
* K4A_LOG_LEVEL =
*    'c'  - log all messages of level 'critical' criticality
*    'e'  - log all messages of level 'error' or higher criticality
*    'w'  - log all messages of level 'warning' or higher criticality
*    'i'  - log all messages of level 'info' or higher criticality
*    't'  - log all messages of level 'trace' or higher criticality
*    DEFAULT - log all message of level 'error' or higher criticality
*/
```

El registro para el archivo K4ABT.dll del SDK de seguimiento de personas es similar, salvo que los usuarios deben modificar un conjunto diferente de nombres de variables de entorno:

```console
/**
* environment variables
* K4ABT_ENABLE_LOG_TO_A_FILE =
*    0    - completely disable logging to a file
*    log\custom.log - log all messages to the path and file specified - must end in '.log' to
*                     be considered a valid entry
*    ** When enabled this takes precedence over the value of K4A_ENABLE_LOG_TO_STDOUT
*
* K4ABT_ENABLE_LOG_TO_STDOUT =
*    0    - disable logging to stdout
*    all else  - log all messages to stdout
*
* K4ABT_LOG_LEVEL =
*    'c'  - log all messages of level 'critical' criticality
*    'e'  - log all messages of level 'error' or higher criticality
*    'w'  - log all messages of level 'warning' or higher criticality
*    'i'  - log all messages of level 'info' or higher criticality
*    't'  - log all messages of level 'trace' or higher criticality
*    DEFAULT - log all message of level 'error' or higher criticality
*/
```

## <a name="device-doesnt-enumerate-in-device-manager"></a>El dispositivo no aparece en el administrador de dispositivos

- Compruebe el LED de estado detrás del dispositivo; si parpadea en color ámbar, tiene un problema de conectividad USB y no está recibiendo suficiente energía. El cable de la fuente de alimentación debe estar conectado al adaptador de alimentación proporcionado. Aunque el cable de alimentación tiene conectada una entrada USB tipo A, el dispositivo requiere más energía de la que puede proporcionar el puerto USB de un equipo. Por lo tanto, no debe conectarlo a un puerto de PC ni a un concentrador USB.
- Compruebe que el cable de alimentación está conectado y que está usando el puerto USB3 para los datos.
- Intente cambiar el puerto USB3 para la conexión de datos (se recomienda usar un puerto USB cerca de la placa base; por ejemplo, en la parte posterior del equipo).
- Examine el cable; los cables dañados o de baja calidad pueden provocar una visualización no confiable (el dispositivo parece "parpadear" en el administrador de dispositivos).
- Si se ha conectado al equipo portátil y este funciona con batería, es posible que esté limitando la energía al puerto.
- Reinicie el equipo host.
- Si el problema persiste, puede existir un problema de compatibilidad.
- Si se produjo un error durante la actualización del firmware y el dispositivo no se ha recuperado por sí solo, realice un [restablecimiento de fábrica](https://support.microsoft.com/help/4494277/reset-azure-kinect-dk).

## <a name="azure-kinect-viewer-fails-to-open"></a>No se puede abrir el visor de Azure Kinect

- Compruebe primero que el dispositivo aparezca en el administrador de dispositivos de Windows.

    ![Cámaras de Azure Kinect en el administrador de dispositivos de Windows](./media/resources/viewer-fails.png)

- Compruebe si tiene cualquier otra aplicación que use el dispositivo (por ejemplo, la aplicación de cámara de Windows). Solo una aplicación puede tener acceso al dispositivo a la vez.
- Compruebe si hay mensajes de error en el registro k4aviewer.err.
- Abra la aplicación de cámara de Windows y compruebe que funciona.
- Apague el dispositivo y enciéndalo, espere a que el LED se apague antes usar el dispositivo.
- Reinicie el equipo host.
- Asegúrese de que está usando los controladores de gráficos más recientes en su equipo.
- Si usa su propia versión del SDK, pruebe a usar la versión publicada oficialmente para ver si corrige el problema.
- Si el problema persiste, [recopile registros](troubleshooting.md#collecting-logs) y envíe los comentarios.

## <a name="cannot-find-microphone"></a>No se puede encontrar el micrófono

- Compruebe primero que la matriz de micrófono se muestre en el administrador de dispositivos.
- Si un dispositivo se muestra y funciona correctamente en Windows, el problema puede deberse a que, después de actualizar el firmware, Windows haya asignado un id. de contenedor diferente a la cámara de profundidad.
- Para intentar restablecerlo, vaya al administrador de dispositivos, haga clic con el botón derecho en la "matriz de micrófono de Azure Kinect" y seleccione "Desinstalar dispositivo". Una vez que haya finalizado, desconecte y vuelva a conectar el sensor.

    ![Matriz de micrófono de Azure Kinect](./media/resources/mic-not-found.png)

- Después, reinicie el visor de Azure Kinect e inténtelo de nuevo.

## <a name="device-firmware-update-issues"></a>Problemas de actualización de firmware del dispositivo

- Si no se detecta el número de versión correcto después de la actualización, es posible que deba apagar el dispositivo y volver a encenderlo.
- Si se interrumpe la actualización del firmware, puede quedar en mal estado y no mostrarse. Conecte y vuelva a conectar el dispositivo y espere 60 segundos para ver si se puede recuperar.
De lo contrario, realice un [restablecimiento de fábrica](https://support.microsoft.com/help/4494277/reset-azure-kinect-dk).

## <a name="image-quality-issues"></a>Problemas de calidad de la imagen

- Inicie el [visor de Azure Kinect](azure-kinect-viewer.md) y compruebe la ubicación del dispositivo en caso de interferencia, o si el sensor está bloqueado, o si se ha modificado la lente.
- Pruebe otros modos operativos para reducir el número las opciones si el problema está ocurriendo en el modo específico.
- Para compartir los problemas de calidad de imagen con el equipo, puede:

1) Hacer una pausa en la vista [visor de Azure Kinect](azure-kinect-viewer.md) y realizar una captura de pantalla, o
2) Realizar grabaciones mediante [la grabadora de Azure Kinect](azure-kinect-recorder.md), por ejemplo, `k4arecorder.exe -l 5 -r 5 output.mkv`

## <a name="inconsistent-or-unexpected-device-timestamps"></a>Marcas de tiempo de dispositivo incoherentes o inesperadas

Realizar una llamada a ```k4a_device_set_color_control``` puede provocar temporalmente algunos cambios de tiempo en el dispositivo que pueden tardar algunas capturas para estabilizarse. Evite llamar a la API en el bucle de captura de imágenes para evitar que se restablezca el cálculo de tiempo interno con cada nueva imagen. En su lugar, llame a la API antes de iniciar la cámara o simplemente cuando necesite cambiar el valor dentro del bucle de captura de imagen. En concreto, evite llamar a ```k4a_device_set_color_control(K4A_COLOR_CONTROL_AUTO_EXPOSURE_PRIORITY)```.

## <a name="usb3-host-controller-compatibility"></a>Compatibilidad con el controlador de host USB3

Si el dispositivo no se muestra en el administrador de dispositivos, puede deberse a que está conectado a un controlador USB3 no compatible. 

En el caso de Azure Kinect DK en **Windows, Intel**, **Texas Instruments (TI)** y **Renesas** son los *controladores de host admitidos*. El SDK de Azure Kinect en plataformas Windows se basa en un id. de contenedor unificado y debe abarcar los dispositivos USB 2.0 y 3.0 para que el SDK pueda encontrar la profundidad, el color y los dispositivos de audio que se encuentran físicamente en el mismo dispositivo. En Linux, se pueden admitir más controladores de host, ya que la plataforma requiere menos el id. de contenedor y más los números de serie de los dispositivos. 

El tema de los controladores de host USB se complica aún más cuando un equipo tiene más de un controlador de host instalado. Cuando se mezclan los controladores de host, el usuario puede experimentar problemas en los que algunos puertos funcionan correctamente y otros no funcionan en absoluto. En función de cómo se conectan los puertos según el caso, puede ver que todos los puertos frontales tienen problemas con Azure Kinect

**Windows:** Para averiguar qué controlador de host tiene abierto el administrador de dispositivos

1. Ver -> Dispositivos por tipo 
2. Con Azure Kinect conectado, seleccione Cámaras -> Cámara de Azure Kinect 4K
3. Ver -> Dispositivos por conexión

![Solución de problemas del puerto USB](./media/resources/usb-troubleshooting.png)

Para comprender mejor qué puerto USB está conectado en su equipo, repita estos pasos para cada uno a medida que conecte Azure Kinect DK a diferentes puertos USB en el equipo.

## <a name="depth-camera-auto-powers-down"></a>La cámara de profundidad se apaga automáticamente

El láser que usa la cámara de profundidad para calcular los datos de profundidad de la imagen tiene una duración limitada. Para maximizar la duración de los láser, la cámara de profundidad detecta cuando no se están consumiendo datos de profundidad. La cámara de profundidad se apaga cuando el dispositivo transmite durante varios minutos, pero el equipo host no están leyendo los datos. También afecta a la sincronización de varios dispositivos en la que los dispositivos subordinados arrancan en un estado en el que la cámara de profundidad está transmitiendo y los fotogramas de profundidad ayudan activamente a esperar a que el dispositivo maestro empiece a sincronizar las capturas. Para evitar este problema en escenarios de captura de varios dispositivos, asegúrese de que el dispositivo maestro se inicia en el plazo de un minuto desde que se inició el primer subordinado. 

## <a name="using-body-tracking-sdk-with-unreal"></a>Uso del SDK de seguimiento de personas con Unreal

Para usar el SDK de seguimiento de personas con Unreal, asegúrese de que ha agregado `<SDK Installation Path>\tools` a la variable de entorno `PATH` y copiado `dnn_model_2_0.onnx` y `cudnn64_7.dll` en `Program Files/Epic Games/UE_4.23/Engine/Binaries/Win64`.

## <a name="next-steps"></a>Pasos siguientes

[Más información de soporte técnico](support.md)
