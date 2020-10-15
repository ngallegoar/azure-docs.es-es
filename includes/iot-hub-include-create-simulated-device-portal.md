---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 02/26/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 8d8f8021925ac9c9e427dd6571ecaa1a30c85497
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186937"
---
<!-- This is the instructions for creating a simulated device you can use for testing routing.-->

A continuación, cree una identidad del dispositivo y guarde su clave para su uso posterior. Dicha identidad la usa la aplicación de simulación para enviar mensajes a la instancia de IoT Hub. Esta funcionalidad no está disponible en PowerShell o cuando se usa una plantilla de Azure Resource Manager. Los pasos siguientes indican cómo crear el dispositivo simulado mediante [Azure Portal](https://portal.azure.com).

1. Abra [Azure Portal](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

2. Seleccione **Grupos de recursos** y, después, elija su grupo de recursos. En este tutorial se usa **ContosoResources**.

3. En la lista de recursos, seleccione su centro de IoT. En este tutorial se usa **ContosoTestHub**. Seleccione **Dispositivos IoT** en el panel Cuadro.

4. Seleccione **+Agregar**. En el panel Agregar dispositivo, rellene el identificador del dispositivo. En este tutorial se usa **Contoso-Test-Device**. Deje en blanco las claves y marque **Generar claves automáticamente**. Asegúrese de que **Connect device to IoT hub** (Conectar dispositivo a IoT Hub) está habilitado. Seleccione **Guardar**.

   ![La pantalla para agregar dispositivos](./media/iot-hub-include-create-simulated-device-portal/add-device.png)

5. Ahora que se ha creado, seleccione el dispositivo para ver las claves generadas. Seleccione el icono Copiar en la clave principal y guárdelo en alguna parte, como el Bloc de notas, para la fase de pruebas de este tutorial.

   ![Los detalles del dispositivo, incluidas las claves](./media/iot-hub-include-create-simulated-device-portal/device-details.png)