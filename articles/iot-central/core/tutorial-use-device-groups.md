---
title: 'Tutorial: Uso de grupos de dispositivos en una aplicación de Azure IoT Central | Microsoft Docs'
description: 'Tutorial: Como operador, aprenda a usar grupos de dispositivos para analizar la telemetría de los dispositivos de la aplicación de Azure IoT Central.'
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 3192a9f121d4380a3e681747596fc91997662bf0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90967936"
---
# <a name="tutorial-use-device-groups-to-analyze-device-telemetry"></a>Tutorial: Uso de grupos de dispositivos para analizar la telemetría de dispositivo

En este artículo se describe cómo usar grupos de dispositivos para analizar la telemetría de dispositivo en la aplicación de Azure IoT Central como operador.

Un grupo de dispositivos es una lista de dispositivos que se agrupan entre sí porque cumplen algunos criterios especificados. Los grupos de dispositivos ayudan a administrar, visualizar y analizar los dispositivos a escala, agrupando dichos dispositivos en grupos lógicos más pequeños. Por ejemplo, puede crear un grupo de dispositivos para enumerar todos los dispositivos de aire acondicionado de Seattle para que, así, un técnico de Seattle pueda encontrar los dispositivos de los que es responsable.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de un grupo de dispositivos
> * Uso de un grupo de dispositivos para analizar la telemetría del dispositivo

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, debe completar los inicios rápidos [Creación de una aplicación de Azure IoT Central](./quick-deploy-iot-central.md) e [Incorporación de un dispositivo simulado a la aplicación de IoT Central](./quick-create-simulated-device.md) para crear la plantilla de dispositivo **MXChip IoT DevKit** con la que va a trabajar.

## <a name="create-simulated-devices"></a>Creación de dispositivos simulados

Antes de crear un grupo de dispositivos, agregue al menos cinco dispositivos simulados desde la plantilla de dispositivo **MXChip IoT DevKit** que se va a usar en este tutorial:

![Cinco dispositivos de sensor simulados](./media/tutorial-use-device-groups/simulated-devices.png)

En cuatro de los dispositivos de sensor simulados, use la vista **Administrar dispositivo** para establecer el nombre del cliente en *Contoso*:

![Establecimiento del nombre de cliente en Contoso](./media/tutorial-use-device-groups/customer-name.png)

## <a name="create-a-device-group"></a>Creación de un grupo de dispositivos

Para crear un grupo de dispositivos:

1. Elija **Grupos de dispositivos** en el panel izquierdo.

1. Seleccione **+** :

    ![Grupo de dispositivos nuevo](media/tutorial-use-device-groups/image1.png)

1. Asigne al grupo de dispositivos el nombre *Dispositivos de Contoso*. También puede agregar una descripción. Un grupo de dispositivos solo puede contener los dispositivos de una sola plantilla de dispositivo. Elija la plantilla de dispositivo **MXChip IoT DevKit** para este grupo.

1. Para personalizar el grupo de dispositivos para incluir solo los dispositivos que pertenecen a **Contoso**, seleccione **+ Filtrar**. Seleccione la propiedad **Nombre del cliente**, el operador de comparación **Es igual a** y **Contoso** como valor. Puede agregar varios filtros y dispositivos que cumplan **todos** los criterios de filtro establecidos en el grupo de dispositivos. El grupo de dispositivos que cree será accesible para todas aquellas personas que tengan acceso a la aplicación, por lo que cualquier usuario podrá verlo, modificarlo o eliminarlo:

    ![Consulta del grupo de dispositivos](media/tutorial-use-device-groups/image2.png)

    > [!TIP]
    > El grupo de dispositivos es una consulta dinámica. Cada vez que vea la lista de dispositivos, puede haber diferentes dispositivos en ella. La lista depende de qué dispositivos cumplen actualmente los criterios de la consulta.

1. Elija **Guardar**.

> [!NOTE]
> En los dispositivos de Azure IoT Edge, seleccione las plantillas de Azure IoT Edge para crear un grupo de dispositivos.

## <a name="analytics"></a>Análisis

Puede usar **Analytics** con un grupo de dispositivos para analizar la telemetría de los dispositivos del grupo. Por ejemplo, puede trazar la temperatura media notificada por todos los dispositivos Environmental Sensor de Contoso.

Para analizar la telemetría de un grupo de dispositivos:

1. Elija **Analytics** en el panel izquierdo.

1. Seleccione el grupo de dispositivos **Dispositivos de Contoso** que creó. A continuación, agregue los tipos de telemetría **Temperatura** y **Humedad**:

    ![Creación de análisis](./media/tutorial-use-device-groups/create-analysis.png)

    Use los iconos de engranaje situados junto a los tipos de telemetría para seleccionar un tipo de agregación. El valor predeterminado es **Promedio**. Use **Dividir por** para cambiar el modo en que se muestran los datos de agregado. Por ejemplo, si divide por id. del dispositivo, verá un trazado para cada dispositivo al seleccionar **Analizar**.

1. Seleccione **Analizar** para ver los valores de telemetría promedio:

    ![Visualización del análisis](./media/tutorial-use-device-groups/view-analysis.png)

    Puede personalizar la vista, cambiar el período de tiempo mostrado y exportar los datos.

## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió a usar grupos de dispositivos en la aplicación de Azure IoT Central, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Creación de reglas de telemetría](tutorial-create-telemetry-rules.md)
