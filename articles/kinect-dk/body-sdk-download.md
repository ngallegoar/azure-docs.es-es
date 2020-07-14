---
title: Descarga del SDK de seguimiento de personas de Azure Kinect
description: Aprenda a descargar cada versión del SDK del Sensor de Azure Kinect en Windows o Linux.
author: qm13
ms.author: quentinm
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: azure, kinect, sdk, actualización de descarga, más reciente, disponible, instalar, instalación, cuerpo, corporal, personas, seguimiento
ms.openlocfilehash: e3f8233d208e2a45c1af9a52a76b6064b15bfe4b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85277070"
---
# <a name="download-azure-kinect-body-tracking-sdk"></a>Descarga del SDK de seguimiento de personas de Azure Kinect

En este documento se proporcionan vínculos para instalar cada versión del SDK de seguimiento de personas de Azure Kinect.

## <a name="azure-kinect-body-tracking-sdk-contents"></a>Contenido del SDK de seguimiento de personas de Azure Kinect

- Encabezados y bibliotecas para compilar una aplicación de seguimiento de personas mediante Azure Kinect DK.
- Archivos DLL redistribuibles necesarios para las aplicaciones de seguimiento de personas que usan Azure Kinect DK.
- Aplicaciones de seguimiento de cuerpos de muestra.

## <a name="windows-download-links"></a>Vínculo de descarga para Windows

Versión       | Descargar
--------------|----------
1.0.1 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100942) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.1)
1.0.0 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100848) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.0)
0.9.5 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100636) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.5)
0.9.4 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100415) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.4)
0.9.3 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100307) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.3)
0.9.2 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100128) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.2)
0.9.1 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100063) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.1)
0.9.0 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=58402) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/0.9.0)

## <a name="linux-installation-instructions"></a>Instrucciones de instalación para Linux

Actualmente, la única distribución admitida es Ubuntu 18.04. Para solicitar soporte técnico para otras distribuciones, consulte [esta página](https://aka.ms/azurekinectfeedback).

En primer lugar, deberá configurar el [repositorio de paquetes de Microsoft](https://packages.microsoft.com/), siguiendo las instrucciones de [este artículo](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software).

El paquete `libk4abt<major>.<minor>-dev` contiene los encabezados y los archivos CMake que se van a compilar en `libk4abt`.
El paquete `libk4abt<major>.<minor>` contiene los objetos compartidos necesarios para ejecutar archivos ejecutables que dependen de `libk4abt`, así como el visor de ejemplos.

Los tutoriales básicos requieren el paquete `libk4abt<major>.<minor>-dev`. Para instalarlo, ejecute

`sudo apt install libk4abt1.0-dev`

Si el comando se ejecuta correctamente, el SDK está listo para su uso.

> [!NOTE]
> Cuando instale el SDK, recuerde la ruta de acceso de instalación. Por ejemplo, "C:\Archivos de programa\Azure Kinect Body Tracking SDK 1.0.0". Encontrará las muestras a las que se hace referencia en los artículos en esta ruta de acceso.
> Los ejemplos de seguimiento de personas se encuentran en la carpeta [body-tracking-samples](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples) del repositorio Azure-Kinect-Samples. Encontrará las muestras a las que se hace referencia aquí.

## <a name="change-log"></a>Registro de cambios

### <a name="v101"></a>v1.0.1
* [Corrección de errores] Se corrigió el problema por el que el SDK se bloquea si se carga onnxruntime.dll desde la ruta de acceso en Windows compilación 19025 o posterior: [Vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/932)

### <a name="v100"></a>v1.0.0
* [Característica] Se agrega el contenedor de C# el instalador de msi.
* [Corrección de errores] Se corrigió el problema por el que la rotación de encabezado no se puede detectar correctamente: [Vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/997)
* [Corrección de errores] Se corrigió el problema por el que el uso de CPU alcanza el 100 % en una máquina Linux: [Vínculo](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1007)
* [Muestras] Se agregaron dos muestras al repositorio de muestras. En la muestra 1 se demuestra cómo transformar los resultados del seguimiento de personas del espacio de profundidad al espacio de colores: [vínculo](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/camera_space_transform_sample); en la muestra 2 se demuestra cómo detectar el plano de planta: [vínculo](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/floor_detector_sample)

### <a name="v095"></a>v0.9.5
* [Característica] Compatibilidad con C# El contenedor C# está empaquetado en el paquete NuGet.
* [Característica] Compatibilidad con varios seguimientos. Se permite la creación de varios seguimientos. Ahora el usuario puede crear varios seguimientos para realizar un seguimiento de personas desde distintos dispositivos de Azure Kinect.
* [Característica] Compatibilidad de procesamiento de varios subprocesos para el modo de CPU. Al ejecutarse en modo de CPU, se usarán todos los núcleos para maximizar la velocidad.
* [Característica] Se agregó `gpu_device_id` a la estructura `k4abt_tracker_configuration_t`. Permita a los usuarios especificar un dispositivo GPU distinto del predeterminado para ejecutar el algoritmo de seguimiento de personas.
* [Corrección de errores/cambio importante] Se corrigió un error tipográfico en un nombre de articulación. Se cambió el nombre de la articulación de `K4ABT_JOINT_SPINE_NAVAL` a `K4ABT_JOINT_SPINE_NAVEL`.

### <a name="v094"></a>v0.9.4
* [Característica] Se agregó compatibilidad con articulaciones de la mano. El SDK proporcionará información para tres articulaciones adicionales para cada mano: HAND, HANDTIP, THUMB.
* [Característica] Se agregó el nivel de confianza de predicción para cada articulación detectada.
* [Característica] Se agregó compatibilidad con el modo de CPU. Al cambiar el valor de `cpu_only_mode` en `k4abt_tracker_configuration_t`, el SDK ahora se puede ejecutar en modo de CPU, lo que no requiere que el usuario tenga una tarjeta gráfica eficaz.

### <a name="v093"></a>v0.9.3
* [Característica] Se publicó un nuevo modelo DNN dnn_model_2_0.onnx, que mejora en gran medida la solidez del seguimiento de personas.
* [Característica] Se deshabilitó el suavizado temporal de forma predeterminada. Las articulaciones sometidas a seguimiento tendrán mayor capacidad de respuesta.
* [Característica] Se mejoró la precisión del mapa del índice corporal.
* [Corrección de errores] Se corrigió el error por el que el valor de la orientación del sensor no es efectivo.
* [Corrección de errores] Se cambió el tipo de body_index_map de K4A_IMAGE_FORMAT_CUSTOM a K4A_IMAGE_FORMAT_CUSTOM8.
* [Problema conocido] Dos cuerpos cercanos se pueden combinar en un único segmento de instancia.

### <a name="v092"></a>v0.9.2
* [Cambio importante] Actualización para depender del SDK de Sensor Azure Kinect 1.2.0 más reciente.
* [Cambio de API] La función `k4abt_tracker_create` empezará a tomar una entrada `k4abt_tracker_configuration_t`. 
* [Cambio de API] Cambio de la API `k4abt_frame_get_timestamp_usec` a `k4abt_frame_get_device_timestamp_usec` para que sea más específica y coherente con el SDK del Sensor 1.2.0.
* [Característica] Los usuarios pueden especificar la orientación de montaje del sensor al crear el seguimiento para conseguir resultados de seguimiento de personas más precisos al montarlos en ángulos diferentes.
* [Característica] Se proporciona una nueva API `k4abt_tracker_set_temporal_smoothing` para cambiar la cantidad de suavizado temporal que el usuario quiere aplicar.
* [Característica] Se agregó el contenedor de C++ k4abt.hpp.
* [Característica] Se agregó el encabezado de definición de la versión k4abtversion.h.
* [Corrección de errores] Se corrigió el error que causaba un uso extremadamente alto de CPU.
* [Corrección de errores] Se corrigió el error de bloqueo del registrador.

### <a name="v091"></a>v0.9.1
* [Corrección de errores] Se corrigió la pérdida de memoria al destruir el seguimiento.
* [Corrección de errores] Mejores mensajes de error para las dependencias que faltan.
* [Corrección de errores] Error sin bloqueo al crear una segunda instancia de seguimiento.
* [Corrección de errores] Las variables de entorno del registrador ahora funcionan correctamente.
* Compatibilidad de Linux

### <a name="v090"></a>v0.9.0

* [Cambio importante] Se degradó la dependencia del SDK a CUDA 10.0 (de CUDA 10.1). El tiempo de ejecución de ONNX admite oficialmente solo hasta CUDA 10.0.
* [Cambio importante] Se cambió al tiempo de ejecución de ONNX en lugar del tiempo de ejecución de Tensorflow. Reduce el tiempo de inicio del primer fotograma y el uso de memoria. También reduce el tamaño binario del SDK.
* [Cambio de API] Se cambió el nombre de `k4abt_tracker_queue_capture()` a `k4abt_tracker_enqueue_capture()`.
* [Cambio de API] `k4abt_frame_get_body()` se dividió en dos funciones independientes: `k4abt_frame_get_body_skeleton()` y `k4abt_frame_get_body_id()`. Ahora puede consultar el identificador del cuerpo sin tener que copiar siempre toda la estructura del esqueleto.
* [Cambio de API] Se agregó la función `k4abt_frame_get_timestamp_usec()` para simplificar los pasos para que los usuarios consulten la marca de tiempo del armazón corporal.
* Se mejoró aún más la precisión del algoritmo de seguimiento de personas y confiabilidad del seguimiento.

## <a name="next-steps"></a>Pasos siguientes

- [Información general de Azure Kinect DK](about-azure-kinect-dk.md)

- [Configuración de Azure Kinect DK](set-up-azure-kinect-dk.md)

- [Configuración del seguimiento corporal de Azure Kinect](body-sdk-setup.md)
