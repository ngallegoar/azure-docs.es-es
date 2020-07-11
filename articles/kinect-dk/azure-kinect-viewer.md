---
title: Azure Kinect Viewer
description: Entienda cómo visualizar todos los flujos de datos del dispositivo mediante el visor de Azure Kinect.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: azure, kinect, sensor, visor, visualización, profundidad, rgb, color, imu, audio, micrófono, nube de puntos
ms.openlocfilehash: 57cf7df831e97da4143a7f196b69a3a10609a017
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276931"
---
# <a name="azure-kinect-viewer"></a>Azure Kinect Viewer

El visor de Azure Kinect, que se encuentra en el directorio de herramientas instaladas como `k4aviewer.exe` (por ejemplo, `C:\Program Files\Azure Kinect SDK vX.Y.Z\tools\k4aviewer.exe`, donde `X.Y.Z` es la versión instalada del SDK), puede usarse para visualizar todos los flujos de datos del dispositivo a fin de:

* Comprobar que los sensores funcionan correctamente.
* Ayudar a posicionar el dispositivo.
* Experimentar con la configuración de la cámara.
* Leer la configuración del dispositivo.
* Reproducir grabaciones realizadas con la [Grabadora de Azure Kinect](azure-kinect-recorder.md).

Para obtener más información sobre el visor de Azure Kinect, vea [Cómo usar el vídeo de Azure Kinect](https://www.microsoft.com/videoplayer/embed/RE3hNwG).

El visor de Azure Kinect es de [código abierto](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/tools/k4aviewer) y se puede usar como ejemplo para usar las API.

## <a name="use-viewer"></a>Uso del visor

El visor puede funcionar en dos modos: con datos en directo del sensor o datos grabados ([Grabadora de Azure Kinect](azure-kinect-recorder.md)).

### <a name="start-application"></a>Inicio de la aplicación

Ejecute `k4aviewer.exe` para iniciar la aplicación.

![Comprobación de la versión del firmware del dispositivo con el visor](./media/how-to-guides/open-viewer.png)

### <a name="use-the-viewer-with-live-data"></a>Uso del visor con datos en directo

1. En la sección **Abrir dispositivo**, seleccione el **Número de serie** del dispositivo que se va a abrir. Luego seleccione **Actualizar** si el dispositivo no aparece.
2. Seleccione el botón **Abrir dispositivo**.
3. Seleccione **Iniciar** para iniciar la transmisión de datos con la configuración predeterminada.

### <a name="use-the-viewer-with-recorded-data"></a>Uso del visor con datos grabados

En la sección **Abrir grabación**, vaya al archivo grabado y selecciónelo.

## <a name="check-device-firmware-version"></a>Comprobación de la versión de firmware del dispositivo

Acceda a la versión de firmware del dispositivo en la ventana de configuración, como se muestra en la siguiente imagen.

![Comprobación de la versión del firmware del dispositivo con el visor](./media/how-to-guides/check-firmware-update.png)

Por ejemplo, en este caso, el ISP de la cámara de profundidad está ejecutando FW 1.5.63.

## <a name="depth-camera"></a>Cámara de profundidad

El visor de la cámara de profundidad muestra dos ventanas:

* Una se denomina *Brillo activo* y es una imagen de escala de grises que muestra el brillo de IR.
* La segunda se denomina *Profundidad* y tiene una representación en color de los datos de profundidad.

Mantenga el cursor en el píxel de la ventana de profundidad para ver el valor del sensor de profundidad, como se muestra a continuación.

![Vista de profundidad](./media/how-to-guides/depth-camera.png)

## <a name="rgb-camera"></a>Cámara RGB

En la imagen siguiente se muestra la vista de la cámara de color.

![Vista RGB](./media/how-to-guides/viewer-rgb-camera.png)

Puede controlar la configuración de la cámara RGB desde la ventana de configuración durante la transmisión.

![Controles de la cámara RGB](./media/how-to-guides/rgb-camera-settings.png)

## <a name="inertial-measurement-unit-imu"></a>Unidad de medición inercial (IMU)

La ventana IMU tiene dos componentes, un acelerómetro y un giroscopio.

La mitad superior es el acelerómetro y muestra la aceleración lineal en metros/segundo<sup>2</sup>.  Incluye la aceleración de la gravedad, así que si está descansando en una mesa, el eje Z probablemente muestre alrededor de -9,8 m/s<sup>2</sup>.

La mitad inferior es la parte del giroscopio y muestra el movimiento de rotación en radianes/segundo.

![Vista del sensor de movimiento](./media/how-to-guides/viewer-mu-settings.png)

## <a name="microphone-input"></a>Entrada de micrófono

La vista de micrófono muestra una representación del sonido que se escucha en cada micrófono. Si no hay sonido, el gráfico se muestra como vacío; de lo contrario, se ve una forma de onda azul oscuro con una forma de onda azul claro superpuesta sobre ella.

La onda oscura representa los valores mínimo y máximo observados por el micrófono en esa porción de tiempo. La onda clara representa la raíz cuadrada media de los valores observados por el micrófono en esa porción de tiempo.

![Vista de la entrada de micrófono](./media/how-to-guides/microphone-data.png)

## <a name="point-cloud-visualization"></a>Visualización de la nube de puntos

La profundidad visualizada en 3D permite moverse por la imagen mediante las teclas indicadas.

![Nube de puntos de profundidad](./media/how-to-guides/depth-point-cloud.png)

## <a name="synchronization-control"></a>Control de la sincronización

Puede usar el visor para configurar el dispositivo como modo independiente (predeterminado), maestro o subordinado al configurar la sincronización de varios dispositivos.
Al cambiar la configuración o insertar o quitar el cable de sincronización, seleccione **Actualizar** para actualizar.

![Control de la sincronización externa](./media/how-to-guides/sync-control.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Guía de configuración de la sincronización externa](https://support.microsoft.com/help/4494429/sync-multiple-azure-kinect-dk-devices)
