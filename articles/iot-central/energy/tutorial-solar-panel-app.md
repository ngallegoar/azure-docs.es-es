---
title: 'Tutorial: Creación de una aplicación de supervisión de paneles solares con IoT Central'
description: 'Tutorial: Aprenda a crear una aplicación de paneles solares mediante plantillas de aplicación de Azure IoT Central.'
author: op-ravi
ms.author: omravi
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: d5ea3d3420cb598693ccaede7ee10d2f8c4fd839
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77025782"
---
# <a name="tutorial-create-and-walk-through-the-solar-panel-monitoring-app-template"></a>Tutorial: Creación y tutorial de la plantilla de aplicación de supervisión de paneles solares 



Este tutorial le guía por el proceso de creación de la aplicación de supervisión de paneles solares, que incluye un modelo de dispositivo de ejemplo con datos simulados. En este tutorial, aprenderá a:


> [!div class="checklist"]
> * Instrucciones sobre cómo crear la aplicación de paneles solares gratis
> * Tutorial de la aplicación
> * Limpieza de recursos


Si no tiene ninguna suscripción, [cree una cuenta de evaluación gratuita](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Prerrequisitos
- None
- Le recomendamos que use una suscripción a Azure para la aprueba, aunque no es obligatorio.


## <a name="create-a-solar-panel-monitoring-app"></a>Creación de una aplicación de supervisión de paneles solares 

Puede crear esta aplicación en tres sencillos pasos:

1. Abra la [página principal de Azure IoT Central](https://apps.azureiotcentral.com) y haga clic en **Compilar** para crear una nueva aplicación. 

2. Seleccione la pestaña **Energía** y haga clic en **Crear aplicación** en el icono de la aplicación **Solar panel monitoring** (Supervisión de medidores inteligentes). 

    > [!div class="mx-imgBorder"]
    > ![Compilar una aplicación](media/tutorial-iot-central-solar-panel/solar-panel-build.png)
  
3. Con la opción **Crear aplicación** se abrirá el formulario **Nueva aplicación**. Rellene los detalles solicitados tal como se muestra en la ilustración siguiente:
    * **Nombre de la aplicación**: elija un nombre para su aplicación de IoT Central. 
    * **URL**: elija una dirección URL de IoT Central y la plataforma verificará su unicidad.
    * **Prueba gratuita de 7 días**: si ya tiene una suscripción a Azure, se recomienda la configuración predeterminada. Si no tiene ninguna, inicie una evaluación gratuita.
    * **Información de facturación**: la aplicación es gratuita. Los detalles del directorio, la suscripción a Azure y la región son necesarios para aprovisionar los recursos de la aplicación.
    * Haga clic en el botón **Crear** en la parte inferior de la página y la aplicación se creará en un minuto aproximadamente.
        ![Formulario de nueva aplicación](media/tutorial-iot-central-solar-panel/solar-panel-create-app.png)
        
        ![Información de facturación de un formulario de nueva aplicación](media/tutorial-iot-central-solar-panel/solar-panel-create-app-billinginfo.png)


### <a name="verify-the-application-and-simulated-data"></a>Verificación de la aplicación y los datos simulados

La aplicación de paneles solares recién creada es su aplicación y puede modificarla en cualquier momento. Vamos a asegurarnos de que la aplicación se haya implementado y funcione según lo esperado antes de modificarla.

Para verificar la creación de la aplicación y la simulación de datos, vaya al **Panel**. Si puede ver los iconos con algunos datos, significa que la implementación de la aplicación se realizó correctamente. La simulación de datos puede tardar entre 1 y 2 minutos en generar los datos, por lo que deberá esperar un poco. 

## <a name="application-walk-through"></a>Tutorial de la aplicación
Una vez que implemente la plantilla de la aplicación correctamente, esta incluirá un dispositivo de ejemplo de medidores inteligentes, un modelo de dispositivo y un panel.

Adatum es una compañía energética ficticia que supervisa y administra los paneles solares. En el panel de supervisión de paneles solares, verá propiedades, datos y comandos de ejemplo de los paneles solares. Este permite que los operadores y los equipos de soporte técnico realicen de forma proactiva las actividades siguientes antes de que se produzcan incidentes de soporte técnico:
* Revisar la información más reciente del panel y la ubicación de su instalación en el mapa
* Comprobar de manera proactiva el estado del panel y el estado de la conexión
* Revisar las tendencias de generación de energía y temperatura para detectar cualquier patrón anómalo
* Realizar un seguimiento de la generación energética total para fines de planificación y facturación
* Realizar operaciones con comandos y controles, como, por ejemplo, activar un panel y actualizar la versión del firmware. En la plantilla, los botones de comando muestran las funcionalidades posibles y no envían comandos reales.

> [!div class="mx-imgBorder"]
> ![Panel de supervisión de paneles solares](media/tutorial-iot-central-solar-panel/solar-panel-dashboard.png)

### <a name="devices"></a>Dispositivos
La aplicación incluye un dispositivo de ejemplo de paneles solares. Para ver los detalles del dispositivo, haga clic en la pestaña **Dispositivos**.

> [!div class="mx-imgBorder"]
> ![Dispositivos de paneles solares](media/tutorial-iot-central-solar-panel/solar-panel-device.png)


Haga clic en el vínculo **SP0123456789** del dispositivo de ejemplo para ver los detalles del dispositivo. En la página **Actualizar propiedades**, puede actualizar las propiedades de escritura del dispositivo y visualizar los valores actualizados en el panel. 

> [!div class="mx-imgBorder"]
> ![Propiedades del panel solar](media/tutorial-iot-central-solar-panel/solar-panel-device-properties.png)


### <a name="device-template"></a>Plantilla de dispositivo
Haga clic en la pestaña **Plantillas de dispositivo** para ver el modelo de dispositivo de paneles solares. El modelo tiene una interfaz predefinida para los datos, las propiedades, los comandos y las vistas.

> [!div class="mx-imgBorder"]
> ![Plantilla de dispositivos de paneles solares](media/tutorial-iot-central-solar-panel/solar-panel-device-templates.png)


## <a name="clean-up-resources"></a>Limpieza de recursos
Si decide no seguir usando esta aplicación, elimínela con los siguientes pasos:

1. En el panel izquierdo, abra la pestaña Administración.
2. Seleccione la configuración de la aplicación y haga clic en el botón Eliminar situado en la parte inferior de la página. 

    > [!div class="mx-imgBorder"]
    > ![Eliminar aplicación](media/tutorial-iot-central-solar-panel/solar-panel-delete-app.png)


## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información sobre la arquitectura de las aplicaciones de paneles solares, consulte [el artículo de conceptos](https://docs.microsoft.com/azure/iot-central/energy/concept-iot-central-solar-panel-app).
* Cree plantillas de aplicación de paneles solares de forma gratuita: [aplicación de paneles solares](https://apps.azureiotcentral.com/build/new/solar-panel-monitoring)
* Para obtener más información sobre IoT Central, consulte [Introducción a IoT Central](https://docs.microsoft.com/azure/iot-central/).

