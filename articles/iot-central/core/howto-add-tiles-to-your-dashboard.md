---
title: Adición de iconos al panel | Microsoft Docs
description: Aprenda como generador a configurar el panel de la aplicación predeterminado de Azure IoT Central.
author: mavoge
ms.author: mavoge
ms.date: 10/17/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f75e5a28f7ec56750432e74ee48ba68491a5e481
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310298"
---
# <a name="configure-the-application-dashboard"></a>Configuración del panel de la aplicación

El **panel** es la página que se carga cuando los usuarios que tienen acceso a la aplicación van a la dirección URL de la aplicación. Si ha creado una aplicación a partir de una de las **plantillas de aplicación**, la aplicación tendrá un panel predefinido para iniciar. Si ha creado la aplicación a partir de la plantilla de aplicación con el nombre **Aplicación heredada**, el panel estará en blanco para iniciarse.

> [!NOTE]
> Los usuarios pueden [crear varios paneles](howto-create-personal-dashboards.md) además del panel de aplicación predeterminado. Estos paneles pueden ser personales solo para el usuario o se pueden compartir entre todos los usuarios de la aplicación. 

## <a name="add-tiles"></a>Agregar iconos

En la captura de pantalla siguiente se muestra el panel en una aplicación creada a partir de la plantilla **Aplicación personalizada**. Para personalizar el panel predeterminado para la aplicación, seleccione **Editar** en la parte superior izquierda de la página.

> [!div class="mx-imgBorder"]
> ![Panel para aplicaciones basadas en la plantilla "Contoso de ejemplo"](media/howto-add-tiles-to-your-dashboard/dashboard-sample-contoso.png)

Si selecciona **Editar**, se abrirá el panel de la biblioteca de paneles. La biblioteca contiene iconos y paneles primitivos que puede usar para personalizar el panel.

> [!div class="mx-imgBorder"]
> ![Biblioteca de paneles](media/howto-add-tiles-to-your-dashboard/dashboard-library.png)

Por ejemplo, puede agregar un icono **Telemetría** para la temperatura actual del dispositivo. Para ello:
1. Seleccione una **Plantilla de dispositivo**.
1. Seleccione una **Instancia del dispositivo** para el dispositivo que quiere ver en un icono del panel. Verá una lista de las propiedades del dispositivo que se usan en el icono.
1. Para crear el icono en el panel, haga clic en **Temperatura** y arrástrelo al área del panel. También puede hacer clic en la casilla situada junto a **Temperatura** y hacer clic en **Combinar**. En la captura de pantalla siguiente se muestra cómo seleccionar una plantilla de dispositivo y una instancia de dispositivo y luego crear un icono Telemetría de temperatura en el panel.
1. Seleccione **Guardar** en la parte superior izquierda para guardar el icono en el panel.

> [!div class="mx-imgBorder"]
> ![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](media/howto-add-tiles-to-your-dashboard/device-details.png)

Ahora, cuando un operador visualice el panel de la aplicación predeterminado, verá el nuevo icono con la **Temperatura** para el dispositivo. Cada icono tiene un grafo o un gráfico (entre otras opciones) preseleccionado que se mostrará al crear el icono. Sin embargo, los usuarios pueden optar por editar y cambiar esta visualización. 

> [!div class="mx-imgBorder"]
> ![Pestaña "Panel" en la que se muestran la configuración y las propiedades del icono](media/howto-add-tiles-to-your-dashboard/settings-and-properties.png)

## <a name="edit-tiles"></a>Edición de iconos

Para editar un icono en el panel, haga clic primero en **Editar** en la parte superior izquierda de la página, que abrirá el modo de edición para el panel y todos sus iconos. 

> [!div class="mx-imgBorder"]
> ![Pantalla del panel con el modo de edición activado para un icono seleccionado](media/howto-add-tiles-to-your-dashboard/edit-mode.png)

A continuación, haga clic en el icono de **engranaje** en la esquina superior derecha del icono que desea editar. Aquí puede editar aspectos del icono, incluido su título, su visualización, agregación, etc.

> [!div class="mx-imgBorder"]
> ![Lista desplegable para la configuración de la agregación de iconos](media/howto-add-tiles-to-your-dashboard/aggregation-settings.png)

También puede cambiar la visualización del gráfico haciendo clic en el icono de **regla** en el icono.

> [!div class="mx-imgBorder"]
> ![Lista desplegable para la configuración de la visualización de iconos](media/howto-add-tiles-to-your-dashboard/visualization-settings.png)

## <a name="tile-types"></a>Tipos de iconos

En la tabla siguiente se resume el uso de iconos en Azure IoT Central:
 
| Icono | Panel | Descripción
| ----------- | ------- | ------- |
| Contenido | Paneles de conjunto de dispositivos y paneles de aplicación |Los iconos admitidos de Markdown son iconos clicables que muestran texto para el encabezado y la descripción. También puede usar este icono como icono de vínculo para que los usuarios puedan navegar a una dirección URL relacionada con la aplicación.|
| Imagen | Paneles de conjunto de dispositivos y paneles de aplicación |Los iconos de imagen muestran una imagen personalizada y se puede hacer clic en ellos. Use un icono de imagen para agregar gráficos a un panel y, opcionalmente, permitir a los usuarios navegar a una dirección URL que es relevante para la aplicación.|
| Etiqueta | Paneles de aplicación |Los iconos de etiqueta muestran texto personalizado en un panel. Puede elegir el tamaño del texto. Use un icono de etiqueta para agregar información relevante al panel como, por ejemplo, descripciones, detalles de contacto o ayuda.|
| Map | Paneles de aplicación y paneles de dispositivo |Los iconos de mapa muestran la ubicación de un dispositivo en un mapa. También puede mostrar hasta cien puntos del historial de ubicación de un dispositivo. Por ejemplo, puede mostrar una ruta muestreada en la que un dispositivo ha estado en la última semana.|
| Gráfico de líneas | Paneles de aplicación y paneles de dispositivo |Los iconos de gráfico de líneas muestran un gráfico de medidas agregadas para un dispositivo durante un período de tiempo. Por ejemplo, puede mostrar un gráfico de líneas que muestre la temperatura y presión medias en un dispositivo durante la última hora.|
| Gráfico de barras | Paneles de aplicación y paneles de dispositivo |Los iconos de gráfico de barras muestran un gráfico de mediciones agregadas para un dispositivo durante un período de tiempo. Por ejemplo, puede mostrar un gráfico de barras que muestre la temperatura y presión medias en un dispositivo durante la última hora.|
| Gráfico circular | Paneles de conjunto de dispositivos y paneles de aplicación |Los iconos de gráfico circular muestran un gráfico de mediciones agregadas para un dispositivo durante un período de tiempo.|
| Mapa térmico | Paneles de conjunto de dispositivos y paneles de aplicación |Los iconos del mapa térmico muestran información sobre el conjunto de dispositivos, representado como colores.|
| Historial de eventos | Paneles de aplicación y paneles de dispositivo |Los iconos del Historial de eventos muestran los eventos ocurridos en un dispositivo durante un período de tiempo. Por ejemplo, puede usarlo para mostrar todos los cambios de temperatura que se han producido en un dispositivo durante la última hora.|
| Historial de los estados | Paneles de aplicación y paneles de dispositivo |Los iconos del historial de estado muestran los valores de las medidas para un período de tiempo. Por ejemplo, puede usarlo para mostrar los valores de temperatura de un dispositivo durante la última hora.|
| KPI | Paneles de aplicación y paneles de dispositivo | Los iconos de KPI muestran una telemetría o medida de evento agregadas para un período de tiempo. Por ejemplo, puede usarlo para mostrar la temperatura máxima alcanzada para un dispositivo durante la última hora.|
| Último valor conocido | Paneles de aplicación y paneles de dispositivo |Los iconos de último valor conocido muestran el último valor de una telemetría o medida de estado. Por ejemplo, puede usar este icono para mostrar las medidas más recientes de temperatura, presión y humedad de un dispositivo.|

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar el panel de aplicaciones predeterminado de Azure IoT Central, puede [aprender a crear un panel personal](howto-create-personal-dashboards.md).
