---
title: Descripción del control de versiones de las plantillas de dispositivo para las aplicaciones de Azure IoT Central | Microsoft Docs
description: Iteración sobre las plantillas de dispositivo mediante la creación de nuevas versiones y sin afectar a los dispositivos conectados en vivo
author: sarahhubbard
ms.author: sahubbar
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: bb77d1a09cb2692765c4c834ce617d13465d4d67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157575"
---
# <a name="create-a-new-device-template-version"></a>Creación de una nueva versión de plantilla de dispositivo



Azure IoT Central permite el desarrollo rápido de aplicaciones de IoT. Puede iterar rápidamente por los diseños de la plantilla de dispositivo mediante la adición, modificación o eliminación de las funcionalidades, las vistas y personalizaciones del dispositivo. Cuando haya publicado la plantilla de dispositivo, el modelo de funcionalidad del dispositivo se muestra como **publicado** con los iconos de candado junto al modelo. Para realizar cambios en el modelo de funcionalidad del dispositivo, tendrá que crear una nueva versión de la plantilla del dispositivo. Mientras tanto, las propiedades, las personalizaciones y las vistas en la nube se pueden modificar en cualquier momento sin necesidad de crear una versión de la plantilla de dispositivo. Una vez que haya guardado cualquiera de estos cambios, puede publicar la plantilla de dispositivo para que los cambios más recientes estén disponibles y el operador pueda verlos en el Explorador de dispositivos.

> [!NOTE]
> Para más información sobre cómo crear una plantilla de dispositivo, consulte [Configuración de una plantilla de dispositivo](howto-set-up-template.md).

## <a name="add-customizations-to-the-device-template-without-versioning"></a>Adición de personalizaciones a la plantilla de dispositivo sin control de versiones

Algunos elementos de las funcionalidades del dispositivo se pueden editar sin necesidad de crear una versión de la plantilla de dispositivo ni de las interfaces. Por ejemplo, algunos de estos campos incluyen el nombre para mostrar, el tipo semántico, el valor mínimo, el valor máximo, las posiciones decimales, el color, la unidad, la unidad de visualización, el comentario y la descripción. Para agregar una de estas personalizaciones:

1. Vaya a la página **Plantillas de dispositivo**.
1. Seleccione la plantilla de dispositivo que desea personalizar.
1. Elija la pestaña **Personalizar**.
1. Todas las funcionalidades definidas en el modelo de funcionalidad del dispositivo se mostrarán aquí. Todos los campos que puede editar aquí se pueden guardar y usar en la aplicación, sin necesidad de crear una versión de la plantilla de dispositivo. Si hay campos que quiere editar y son de solo lectura, tendrá que hacer una versión de la plantilla de dispositivo para cambiarlos. Seleccione el campo que desea editar y escriba los nuevos valores.
1. Haga clic en **Save**(Guardar). Ahora, estos valores reemplazarán todo lo que se haya guardado inicialmente en la plantilla de dispositivo y se usarán en la aplicación.

## <a name="versioning-a-device-template"></a>Control de versiones de una plantilla de dispositivo

Al crear una nueva versión de la plantilla de dispositivo, se creará una versión de borrador de la plantilla en la que se puede editar el modelo de funcionalidad del dispositivo. Todas las interfaces publicadas permanecerán publicadas hasta que tengan versiones individuales. Para modificar una interfaz publicada, primero debe crear una nueva versión de plantilla de dispositivo.

Solo se debe crear una versión de la plantilla de dispositivo cuando se intenta editar una parte del modelo de funcionalidad del dispositivo que no se puede editar en la sección de personalizaciones de la plantilla de dispositivo. 

Para controlar la versión de una plantilla de dispositivo:

1. Vaya a la página **Plantillas de dispositivo**.
1. Seleccione la plantilla de dispositivo cuya versión intenta controlar.
1. Haga clic en el botón **Versión** en la parte superior de la página y asigne un nuevo nombre a la plantilla. Hemos sugerido un nuevo nombre que se puede editar.
1. Haga clic en **Crear**.
1. Ahora la plantilla de dispositivo está en modo borrador. Verá que las interfaces todavía están bloqueadas y deben tener una versión independiente para poder editarlas. 

### <a name="versioning-an-interface"></a>Control de versiones de una interfaz

El control de versiones de una interfaz permite agregar, actualizar y quitar las funcionalidades dentro de la interfaz que ya se ha creado. 

Para controlar la versión de una interfaz:

1. Vaya a la página **Plantillas de dispositivo**.
1. Seleccione la plantilla de dispositivo que tiene en modo borrador.
1. Seleccione la interfaz que está en modo publicado y que desea modificar y controlar la versión.
1. Haga clic en el botón **Versión** en la parte superior de la página de la interfaz. 
1. Haga clic en **Crear**.
1. Ahora la interfaz está en modo borrador. Podrá agregar funcionalidades a la interfaz, o editarlas, sin interrumpir las personalizaciones y vistas existentes. 

> [!NOTE]
> No se pueden modificar las versiones de las interfaces estándar publicadas por Azure IoT. Estas interfaces estándar se utilizan para la certificación de dispositivos.

> [!NOTE]
> Una vez publicada la interfaz, no puede eliminar ninguna de sus funcionalidades, ni siquiera en el modo borrador. Las funcionalidades solo se pueden editar o agregar a la interfaz en el modo borrador.


## <a name="migrate-a-device-across-device-template-versions"></a>Migración de un dispositivo a través de versiones de plantillas de dispositivo

Puede crear varias versiones de la plantilla de dispositivo. Con el tiempo, tendrá varios dispositivos conectados mediante estas plantillas de dispositivo. Puede migrar los dispositivos de una versión de la plantilla de dispositivo a otra. Los pasos siguientes describen cómo migrar un dispositivo:

1. Vaya a la página del **Explorador de dispositivos**.
1. Seleccione el dispositivo que necesita migrar a otra versión.
1. Elija **Migrate** (Migrar).
1. Seleccione la plantilla de dispositivo con el número de versión a la que desea migrar el dispositivo y elija **Migrar**.

![Migración de un dispositivo](media/howto-version-device-template/pick-version.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar las versiones de plantilla de un dispositivo en la aplicación de Azure IoT Central, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Creación de reglas de telemetría](tutorial-create-telemetry-rules.md)
