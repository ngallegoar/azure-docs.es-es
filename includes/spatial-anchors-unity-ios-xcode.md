---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/14/2020
ms.author: rgarcia
ms.openlocfilehash: 9c20726ce412e651aba0fdd202031d342350c922
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91971485"
---
Seleccione **Build** (Compilar). En el panel que se abre, seleccione la carpeta a la que va a exportar el proyecto Xcode.

   Cuando se completa la exportación, aparece una carpeta que contiene el proyecto Xcode exportado.

   > [!NOTE]
   > Si aparece una ventana con un mensaje en el que se pregunta si quiere reemplazar o anexar, es aconsejable que seleccione **Append** (Anexar), porque es más rápido. Solo sería necesario seleccionar **Replace** (Reemplazar) si va a cambiar los recursos de la escena. Por ejemplo, si va a agregar, quitar, o cambiar las relaciones de elementos primarios y secundarios, o si va a agregar, quitar o cambiar las propiedades. Si solo va a realizar cambios en el código fuente, **Anexar** debería ser suficiente.

## <a name="open-the-xcode-project"></a>Apertura del proyecto de Xcode

Puede abrir el proyecto `Unity-iPhone.xcodeproj` en Xcode. 

Puede iniciar Xcode y abrir el proyecto `Unity-iPhone.xcodeproj` exportado, o bien iniciar el proyecto en Xcode mediante la ejecución del siguiente comando desde la ubicación en la que exportó el proyecto:

 ```bash
open ./Unity-iPhone.xcodeproj
```

Seleccione el nodo raíz **Unity-iPhone** para ver la configuración del proyecto y, después, seleccione la pestaña **General**.

En **Firma**, asegúrese de que **Automatically manage signing** (Administrar automáticamente la firma) está habilitado. Si no lo está, habilítelo y luego restablezca la configuración de la compilación seleccionando **Enable Automatic** (Habilitar automático) en el panel que aparece.

En **Deployment Info** (Información de implementación), asegúrese de que **Deployment Target** (Destino de la implementación) esté establecido en **11.0**.

## <a name="deploy-the-app-to-your-ios-device"></a>Implementación de la aplicación en el dispositivo iOS

Conecte el dispositivo iOS al equipo Mac y establezca el **esquema activo** en su dispositivo iOS.

   ![Captura de pantalla del botón Mi iPhone para seleccionar el dispositivo.](./media/spatial-anchors-unity/select-device.png)

Seleccione **Build and then run the current scheme** (Compilar y luego ejecutar el esquema actual).

   ![Captura de pantalla del botón de flecha para la "Implementar y ejecutar".](./media/spatial-anchors-unity/deploy-run.png)
