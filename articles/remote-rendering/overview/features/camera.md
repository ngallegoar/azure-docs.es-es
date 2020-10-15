---
title: Cámara
description: Describe la configuración de la cámara y los casos de uso.
author: christophermanthei
ms.author: chmant
ms.date: 03/07/2020
ms.topic: article
ms.openlocfilehash: 5ad39ad9f5e585029cff5d573a026702f259607e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90564748"
---
# <a name="camera"></a>Cámara

Para asegurarse de que el contenido representado de forma local y remota se puede componer sin problemas, es necesario que las distintas propiedades de la cámara permanezcan sincronizadas entre el servidor y el cliente. En particular, la transformación y la proyección de la cámara. Por ejemplo, el contenido representado localmente debe usar la misma transformación y proyección de la cámara con las que se representó el último fotograma remoto.

![Cámara local y remota](./media/camera.png)

En la imagen anterior, la cámara local se ha movido en comparación con el fotograma remoto enviado por el servidor. Como resultado, el contenido local debe representarse desde la misma perspectiva que el fotograma remoto; finalmente, la imagen resultante se vuelve a proyectar en el espacio de la cámara local. Esto se explica de forma detallada en [Reproyección en fase tardía](late-stage-reprojection.md).

El retraso entre la representación local y remota significa que cualquier cambio de esta configuración se aplica con un retraso. Por lo tanto, los nuevos valores no se pueden usar inmediatamente en el mismo fotograma.

Las distancias de plano cercano y plano alejado se pueden definir en la configuración de la cámara. En HoloLens 2, el resto de datos de transformación y proyección se definen mediante el hardware. Al usar la [simulación de escritorio](../../concepts/graphics-bindings.md), se establecen a través de la entrada de su función `Update`.

Los valores modificados se pueden usar localmente en cuanto llega el primer fotograma remoto que se representó con ellos. En HoloLens 2, *HolographicFrame* proporciona los datos que se van a usar para la representación, igual que en cualquier otra aplicación holográfica. En la [simulación de escritorio](../../concepts/graphics-bindings.md), se recuperan a través de la salida de la función `Update`.

## <a name="camera-settings"></a>Configuración de la cámara

Las siguientes propiedades se pueden cambiar en la configuración de la cámara:

**Plano cercano y alejado:**

Para asegurarse de que no se pueden establecer intervalos no válidos, las propiedades **NearPlane** y **FarPlane** son de solo lectura y hay una función independiente, **SetNearAndFarPlane** para cambiar el intervalo. Estos datos se enviarán al servidor al final del fotograma.

> [!IMPORTANT]
> En Unity, esto se controla automáticamente cuando se cambian los planos cercano y alejado de la cámara.

**EnableDepth**:

En ocasiones, resulta útil deshabilitar la escritura del búfer de profundidad de la imagen remota con fines de depuración. Al deshabilitar la profundidad también se impide que el servidor envíe datos de profundidad, lo que reduce el ancho de banda.

> [!TIP]
> En Unity, se proporciona un componente de depuración llamado **EnableDepthComponent** que se puede usar para activar o desactivar esta característica en la interfaz de usuario del editor.

Para cambiar la configuración de la cámara, se puede hacer lo siguiente:

```cs
void ChangeCameraSetting(AzureSession session)
{
    CameraSettings settings = session.Actions.CameraSettings;

    settings.SetNearAndFarPlane(0.1f, 20.0f);
    settings.EnableDepth = false;
}
```

```cpp
void ChangeStageSpace(ApiHandle<AzureSession> session)
{
    ApiHandle<CameraSettings> settings = session->Actions()->GetCameraSettings();

    settings->SetNearAndFarPlane(0.1f, 20.0f);
    settings->SetEnableDepth(false);
}
```

## <a name="api-documentation"></a>Documentación de la API

* [CameraSettings de C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.camerasettings)
* [CameraSettings de C++](https://docs.microsoft.com/cpp/api/remote-rendering/camerasettings)
* [Función GraphicsBindingSimD3d11.Update de C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbindingsimd3d11.update)
* [Función GraphicsBindingSimD3d11::Update de C++](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbindingsimd3d11#update)

## <a name="next-steps"></a>Pasos siguientes

* [Enlace de gráficos](../../concepts/graphics-bindings.md)
* [Reproyección de fase tardía](late-stage-reprojection.md)
