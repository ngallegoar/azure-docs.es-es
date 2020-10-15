---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/19/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 367a0b1d17f8d5ebe4f46835ace963b00e75354e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68229238"
---
Para crear una instancia de IoT Hub mediante Azure Portal:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Crear un recurso** > **Internet de las cosas** > **IoT Hub**.

    ![Seleccionar para instalar IoT Hub](media/iot-hub-tutorials-create-free-hub/selectiothub.png)

1. Para crear la instancia de IoT Hub del nivel gratis, utilice los valores de las tablas siguientes:

    | Configuración | Value |
    | ------- | ----- |
    | Suscripción | Seleccione la suscripción de Azure en el menú desplegable. |
    | Resource group | Cree uno nuevo. Este tutorial utiliza el nombre **tutorials-iot-hub-rg**. |
    | Region | En este tutorial se usa **Oeste de EE. UU.** Puede elegir la región más cercana. |
    | Nombre | En la captura de pantalla siguiente se usa el nombre **tutorials-iot-hub**. Debe elegir un nombre exclusivo de su elección al crear el concentrador. |

    ![Configuración del concentrador 1](media/iot-hub-tutorials-create-free-hub/hubdefinition-1.png)

    | Configuración | Value |
    | ------- | ----- |
    | Plan de tarifa y escalado | F1 Gratis. Solo puede tener un concentrador de nivel gratuito en una suscripción. |
    | Unidades de IoT Hub | 1 |

    ![Configuración del concentrador 2](media/iot-hub-tutorials-create-free-hub/hubdefinition-2.png)

1. Haga clic en **Crear**. La creación del concentrador puede tardar unos minutos.

    ![Configuración del concentrador 3](media/iot-hub-tutorials-create-free-hub/hubdefinition-3.png)

1. Anote el nombre de la instancia de IoT Hub que eligió. Usará este valor más adelante en el tutorial.
