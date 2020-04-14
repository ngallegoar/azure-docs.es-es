---
title: Solución de problemas
description: Información de solución de problemas de Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/25/2020
ms.topic: troubleshooting
ms.openlocfilehash: 7ee219ae5ace0f0da398cc542f410d3c895c8bd4
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80678830"
---
# <a name="troubleshoot"></a>Solución de problemas

En esta página se enumeran los problemas comunes que interfieren con Azure Remote Rendering y las maneras de resolverlos.

## <a name="client-cant-connect-to-server"></a>El cliente no se puede conectar al servidor

Asegúrese de que los firewalls (en el dispositivo, dentro de los enrutadores, etc.) no bloqueen los puertos siguientes:

* **50051 (TCP)** : necesario para la conexión inicial (protocolo de enlace HTTP)
* **8266 (TCP + UDP)** : necesario para la transferencia de datos
* **5000 (TCP)** , **5433 (TCP)** , **8443 (TCP)** : necesario para [ArrInspector](tools/arr-inspector.md)

## <a name="error-disconnected-videoformatnotavailable"></a>Error "Desconectado: VideoFormatNotAvailable"

Compruebe que la GPU admita la descodificación de vídeo de hardware. Consulte [Equipo de desarrollo](../overview/system-requirements.md#development-pc).

Si está trabajando en un equipo portátil con dos GPU, es posible que la GPU que se ejecuta de forma predeterminada no proporcione la funcionalidad de descodificación de vídeo de hardware. Si es así, intente forzar la aplicación para que use la otra GPU. Esto suele ser posible en la configuración del controlador de GPU.

## <a name="h265-codec-not-available"></a>Códec H265 no disponible

Hay dos razones por las que el servidor puede rechazar la conexión debido a un error de **códec no disponible**.

**El códec H265 no está instalado:**

En primer lugar, asegúrese de instalar **Extensiones de vídeo HEVC** como se menciona en la sección [Software](../overview/system-requirements.md#software) de los requisitos del sistema.

Si sigue experimentando problemas, asegúrese de que la tarjeta gráfica admite H265 y de que tiene instalado el controlador de gráficos más reciente. Consulte la sección [Equipo de desarrollo](../overview/system-requirements.md#development-pc) de los requisitos del sistema para obtener información específica del proveedor.

**El códec está instalado, pero no se puede usar:**

La razón de este problema es una configuración de seguridad incorrecta en los archivos DLL. Este problema no se manifiesta al intentar ver vídeos codificados con H265. La reinstalación del códec tampoco soluciona el problema. En su lugar, realice los pasos siguientes:

1. Abra una instancia de **PowerShell con derechos de administrador** y ejecute el comando:

    ```PowerShell
    Get-AppxPackage -Name Microsoft.HEVCVideoExtension
    ```
  
    Ese comando debería generar el objeto `InstallLocation` del códec, similar al siguiente:
  
    ```cmd
    InstallLocation   : C:\Program Files\WindowsApps\Microsoft.HEVCVideoExtension_1.0.23254.0_x64__5wasdgertewe
    ```

1. Abra esa carpeta en el Explorador de Windows.
1. Debería contener las subcarpetas **x86** y **x64**. Haga clic con el botón derecho en una de las carpetas y elija **Propiedades**.
    1. Seleccione la pestaña **Seguridad** y, después, haga clic en el botón **Opciones avanzadas**.
    1. Haga clic en **Cambiar** en **Propietario**.
    1. Escriba **Administradores** en el campo de texto.
    1. Haga clic en **Comprobar nombres** y luego en **Aceptar**.
1. Repita los pasos anteriores para la otra carpeta.
1. Repita también los pasos anteriores en cada archivo DLL de ambas carpetas. Debe haber cuatro archivos DLL en total.

Para comprobar que la configuración ahora es correcta, realice el procedimiento siguiente con cada archivo DLL:

1. Seleccione **Propiedades > Seguridad > Editar**.
1. Recorra la lista de todos los **Grupos/Usuarios** y asegúrese de que cada uno tenga el derecho **Lectura y ejecución** establecido (deben tener la marca de verificación en la columna **Permitir**)

## <a name="low-video-quality"></a>Calidad de vídeo baja

La calidad de vídeo puede verse afectada por la calidad de la red o la falta del códec de vídeo H265.

* Consulte los pasos para [identificar problemas de red](#unstable-holograms).
* Consulte los [requisitos del sistema](../overview/system-requirements.md#development-pc) para instalar el controlador de gráficos más reciente.

## <a name="black-screen-after-successful-model-loading"></a>Pantalla negra tras cargar el modelo correctamente

Si está conectado al entorno de ejecución de representación y ha cargado un modelo correctamente, pero después solo ve una pantalla negra después, puede deberse a distintas causas.

Se recomienda realizar las comprobaciones siguientes antes de realizar un análisis más exhaustivo:

* ¿El códec H265 está instalado? Aunque debería haber una reserva para el códec H264, hemos detectado casos en los que esta reserva no funcionó correctamente. Consulte los [requisitos del sistema](../overview/system-requirements.md#development-pc) para instalar el controlador de gráficos más reciente.
* Al usar un proyecto de Unity, cierre Unity, elimine las carpetas temporales *library* y *obj* del directorio del proyecto, y cargue o cree el proyecto de nuevo. En algunos casos, los datos almacenados en caché provocaron el fracaso del ejemplo sin ningún motivo aparente.

Si estos dos pasos no ayudan, debe averiguar si el cliente recibe fotogramas de vídeo. La consulta se puede realizar mediante programación, como se explica en el capítulo [Consultas de rendimiento del lado del servidor](../overview/features/performance-queries.md). El objeto `FrameStatistics struct` tiene un miembro que indica el número de fotogramas de vídeo que se han recibido. Si este número es mayor que 0 y aumenta con el tiempo, el cliente recibe fotogramas de vídeo reales del servidor. Por lo tanto, debe tratarse de un problema en el lado cliente.

### <a name="common-client-side-issues"></a>Problemas comunes del lado cliente

**El modelo no está dentro del tronco de la vista:**

En muchos casos, el modelo se muestra correctamente pero se encuentra fuera del tronco de la cámara. Un motivo habitual es que el modelo se ha exportado con una dinamización muy descentrada y el plano de recorte lejano de la cámara lo ha recortado. Ayuda a consultar el cuadro de límite del modelo mediante programación y a visualizar el cuadro con Unity como un cuadro de línea o imprimir sus valores en el registro de depuración.

**La canalización de representación de Unity no incluye los enlaces de representación:**

Azure Remote Rendering se enlaza a la canalización de representación de Unity para realizar la composición de fotogramas con el vídeo y para volver a proyectarla. Para comprobar que los enlaces existen, abra el menú *Ventana > Análisis > Frame Debugger*. Habilite esta opción y asegúrese de que existen dos entradas de `HolographicRemotingCallbackPass` en la canalización:

![Unity Frame Debugger](./media/troubleshoot-unity-pipeline.png)

## <a name="unity-code-using-the-remote-rendering-api-doesnt-compile"></a>El código de Unity que usa la API de Remote Rendering no se compila

Cambie el *tipo de compilación* de la solución Unity a **Depuración**. Al probar ARR en el editor de Unity, el objeto `UNITY_EDITOR` de definición solo está disponible en las compilaciones de tipo "Depuración". Tenga en cuenta que esto no está relacionado con el tipo de compilación usado para las [aplicaciones implementadas](../quickstarts/deploy-to-hololens.md), donde debe preferirá las compilaciones de tipo "Versión".

## <a name="unstable-holograms"></a>Hologramas inestables

En caso de que parezca que los objetos representados se mueven con movimientos de cabeza, es posible que experimente problemas de *reproyección de fases con demora* (LSR). Consulte la sección sobre la [reproyección de fases con demora](../overview/features/late-stage-reprojection.md) para obtener instrucciones sobre cómo enfocar esta situación.

Otra causa de los hologramas inestables (hologramas con tambaleo, deformación, vibración o saltos) puede ser una conectividad de red deficiente, en particular, un ancho de banda de red insuficiente o una latencia demasiado alta. Un buen indicador de la calidad de la conexión de red es el valor de [estadísticas de rendimiento](../overview/features/performance-queries.md)`ARRServiceStats.VideoFramesReused`. Los fotogramas reutilizados indican situaciones en las que un fotograma de vídeo antiguo necesitaba reutilizarse en el lado cliente porque no había ningún nuevo fotograma de vídeo disponible (por ejemplo, debido a la pérdida de paquetes o a variaciones en la latencia de red). Si `ARRServiceStats.VideoFramesReused` suele ser mayor que cero, significa que hay un problema de red.

Otro valor que se debe examinar es `ARRServiceStats.LatencyPoseToReceiveAvg`. Debe estar siempre por debajo de 100 ms. Si ve valores más altos, significa que está conectado a un centro de datos que está demasiado lejos.

Para obtener una lista de posibles mitigaciones, consulte las [instrucciones sobre la conectividad de red](../reference/network-requirements.md#guidelines-for-network-connectivity).

## <a name="next-steps"></a>Pasos siguientes

* [Requisitos del sistema](../overview/system-requirements.md)
* [Requisitos de red](../reference/network-requirements.md)
