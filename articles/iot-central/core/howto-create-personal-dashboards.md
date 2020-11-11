---
title: Creación de paneles personales de Azure IoT Central | Microsoft Docs
description: Como usuario, obtenga información sobre cómo crear y administrar sus paneles personales.
author: mavoge
ms.author: mavoge
ms.date: 10/17/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 68bb4e2ac525270004a698471cd44955cd25fefc
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146116"
---
# <a name="create-and-manage-multiple-dashboards"></a>Creación y administración de varios paneles personales

El **panel** es la página que se carga cuando navega por primera vez a la aplicación. Un **generador** en la aplicación define el panel predeterminado de la aplicación para todos los usuarios. Además, puede crear su propio panel de aplicación personalizado. Puede tener varios paneles que muestren datos diferentes y cambiar entre ellos.

Si es **administrador** de la aplicación, también puede crear hasta 10 paneles de nivel de aplicación para compartirlos con otros usuarios de la aplicación. Solo los **administradores** tienen capacidad para crear, editar y eliminar paneles de nivel de aplicación.  

## <a name="create-dashboard"></a>Crear panel

En la captura de pantalla siguiente se muestra el panel en una aplicación creada a partir de la plantilla **Aplicación personalizada**. Puede reemplazar el panel predeterminado de la aplicación por un panel personal, o bien, si es administrador, otro panel de nivel de aplicación. Para ello, seleccione **+ Nuevo** en la parte superior izquierda de la página.

> [!div class="mx-imgBorder"]
> ![Panel para aplicaciones basadas en la plantilla "Aplicación personalizada"](media/howto-create-personal-dashboards/dashboard-custom-app.png)

Al seleccionar **+ Nuevo** se abre el editor de paneles. En el editor, puede asignarle un nombre al panel y elegir los elementos de la biblioteca. La biblioteca contiene iconos y paneles primitivos que puede usar para personalizar el panel.

> [!div class="mx-imgBorder"]
> ![Biblioteca de paneles](media/howto-create-personal-dashboards/dashboard-library.png)

Si es **administrador** de la aplicación, se le ofrecerá la opción de crear un panel de nivel personal o de nivel de aplicación. Si crea un panel de nivel personal, solo podrá verlo usted. Si crea un panel de nivel de aplicación, todos los usuarios de la aplicación podrán verlo. Después de escribir un título y seleccionar el tipo de panel que desea crear, puede guardar y agregar mosaicos más adelante. O bien, si ya está listo y ha agregado una plantilla de dispositivo y una instancia de dispositivo, puede continuar y crear el primer mosaico.  

> [!div class="mx-imgBorder"]
> ![Formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la temperatura](media/howto-create-personal-dashboards/device-details.png)

Por ejemplo, puede agregar un icono **Telemetría** para la temperatura actual del dispositivo. Para ello:

1. Seleccione una **Plantilla de dispositivo**.
1. Seleccione el dispositivo que quiera ver en un icono del panel en **Dispositivos**. Verá una lista de las propiedades del dispositivo que se usan en el icono.
1. Para crear el icono en el panel, haga clic en **Temperatura** y arrástrelo al área del panel. También puede hacer clic en la casilla situada junto a **Temperatura** y en **Agregar icono**. En la captura de pantalla siguiente se muestra la selección de una plantilla de dispositivo y la creación de un icono de Telemetría de temperatura en el panel.
1. Seleccione **Guardar** en la parte superior izquierda para guardar los cambios en el panel.

> [!div class="mx-imgBorder"]
> ![Pestaña "Panel" con los detalles para el mosaico Temperatura](media/howto-create-personal-dashboards/temperature-tile-edit.png)

Ahora, cuando vea el panel personal, verá el nuevo mosaico con la configuración **Temperatura** para el dispositivo:

> [!div class="mx-imgBorder"]
> ![Captura de pantalla que muestra el nuevo icono con la configuración de temperaturas del dispositivo.](media/howto-create-personal-dashboards/temperature-tile-complete.png)

Puede explorar otros tipos de iconos en la biblioteca para descubrir hasta qué punto es posible personalizar los paneles personales.

Para más información sobre cómo usar los mosaicos en Azure IoT Central, consulte [Agregar mosaicos al panel](howto-add-tiles-to-your-dashboard.md).

## <a name="manage-dashboards"></a>Administración de paneles

Puede tener varios paneles personales y cambiar entre ellos o elegir uno de los paneles predeterminados de la aplicación:

> [!div class="mx-imgBorder"]
> ![Cambio entre paneles](media/howto-create-personal-dashboards/switch-dashboards.png)

Puede editar los paneles personales y eliminar los que ya no necesite. Si es **administrador** , también puede editar o eliminar paneles de nivel de aplicación.

> [!div class="mx-imgBorder"]
> ![Eliminar paneles](media/howto-create-personal-dashboards/delete-dashboards.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear y administrar paneles personales, puede [obtener información sobre cómo administrar las preferencias de la aplicación](howto-manage-preferences.md).
