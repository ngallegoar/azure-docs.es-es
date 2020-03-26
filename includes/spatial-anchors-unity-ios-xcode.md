---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: b052772bbfe9d69e430d9f722d8db56b48db7610
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "72933474"
---
Seleccione **Build** (Compilar). En el cuadro de diálogo que aparece, seleccione la carpeta a la que va a exportar el proyecto Xcode.

Cuando se complete la exportación, aparecerá una carpeta que contiene el proyecto Xcode exportado.

> [!NOTE]
> Si aparece una ventana en la que se le pregunta si desea reemplazar o anexar, es aconsejable que seleccione **Append** (Anexar), porque es más rápido. Solo sería necesario seleccionar **Reemplazar** si va a cambiar los recursos de la escena. (Por ejemplo, si va a agregar, quitar, o cambiar las relaciones de elementos primarios y secundarios, o si va a agregar, quitar o cambiar las propiedades.) Si solo va a realizar cambios en el código fuente, **Anexar** debería ser suficiente.

### <a name="convert-the-xcode-project-to-xcworkspace-containing-azure-spatial-anchors-references"></a>Conversión del proyecto de Xcode a xcworkspace con referencias de Azure Spatial Anchors

En la carpeta del proyecto Xcode exportado, ejecute este comando en el Terminal para instalar las instancias de CocoaPods necesarias para el proyecto:

```bash
pod install --repo-update
```

Ya puede abrir `Unity-iPhone.xcworkspace` para abrir el proyecto en Xcode:

```bash
open ./Unity-iPhone.xcworkspace
```

> [!NOTE]
> Consulte los pasos de solución de problemas [aquí](../articles/spatial-anchors/quickstarts/get-started-unity-ios.md#cocoapods-issues-on-macos-catalina-1015) si tiene problemas con CocoaPod después de actualizar a macOS Catalina (10.15).

Seleccione el nodo raíz **Unity-iPhone** para ver la configuración del proyecto y, después, seleccione la pestaña **General**.

En **Firma**, asegúrese de que **Automatically manage signing** (Administrar automáticamente la firma) está habilitado. Si no lo está, habilítelo y seleccione **Enable Automatic** (Habilitar automático) en el cuadro de diálogo que aparece para restablecer la configuración de compilación.

En **Deployment Info** (Información de implementación), asegúrese de que **Deployment Target** (Destino de la implementación) esté establecido en `11.0`.

### <a name="deploy-the-app-to-your-ios-device"></a>Implementación de la aplicación en el dispositivo iOS

Conecte el dispositivo iOS al equipo Mac y establezca el **esquema activo** en su dispositivo iOS.

![Selección del dispositivo](./media/spatial-anchors-unity/select-device.png)

Seleccione **Build and then run the current scheme** (Compilar y luego ejecutar el esquema actual).

![Implementación y ejecución](./media/spatial-anchors-unity/deploy-run.png)

> [!NOTE]
> Si ve un error `library not found for -lPods-Unity-iPhone`, es probable que haya abierto el archivo `.xcodeproj`, en lugar de `.xcworkspace`.
