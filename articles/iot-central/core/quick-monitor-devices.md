---
title: 'Inicio rápido: Supervisión de dispositivos en Azure IoT Central'
description: 'Inicio rápido: Como operador, aprenda a usar una aplicación de Azure IoT Central para supervisar los dispositivos.'
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: af5683bf253e26ab928e46059f9af9d2ab8af3bd
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "90987341"
---
# <a name="quickstart-use-azure-iot-central-to-monitor-your-devices"></a>Inicio rápido: Uso de Azure IoT Central para supervisar los dispositivos

*Este artículo se aplica a los administradores, operadores y compiladores.*

En esta guía de inicio rápido se muestra cómo puede, como operador, usar la aplicación en Microsoft Azure IoT Central para supervisar los dispositivos y cambiar la configuración.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, debe completar los tres inicios rápidos anteriores: [Crear una aplicación de Azure IoT Central](./quick-deploy-iot-central.md), [Agregar un dispositivo simulado a la aplicación en IoT Central](./quick-create-simulated-device.md) y [Configurar reglas y acciones para el dispositivo](quick-configure-rules.md).

## <a name="receive-a-notification"></a>Recibir una notificación

Azure IoT Central envía notificaciones acerca de los dispositivos como mensajes de correo electrónico. El generador ha agregado una regla para enviar una notificación cuando la temperatura del sensor de cualquier dispositivo conectado supera un umbral. Compruebe los correos electrónicos enviados a la cuenta que el desarrollador ha elegido para recibir notificaciones.

Abra el mensaje de correo electrónico que recibió al final del inicio rápido [Configurar reglas y acciones para el dispositivo](quick-configure-rules.md). En el correo electrónico, seleccione el vínculo al dispositivo:

![Correo electrónico de notificación de alerta](media/quick-monitor-devices/email.png)

Se abre en el explorador la vista **Información general** del dispositivo simulado que creó en los inicios rápidos anteriores:

![Dispositivo que desencadenó el mensaje de correo electrónico de notificación](media/quick-monitor-devices/dashboard.png)

## <a name="investigate-an-issue"></a>Investigar un problema

Los operadores pueden ver información acerca del dispositivo en las vistas **Información general**, **Acerca de** y **Comandos**. El generador ha creado una vista **Administrar dispositivo** en la que se puede editar la información del dispositivo y establecer sus propiedades.

El gráfico del panel muestra un trazado de la temperatura del dispositivo. Decide que la temperatura del dispositivo es demasiado alta.

## <a name="remediate-an-issue"></a>Corregir un problema

Para realizar cualquier cambio en el dispositivo, use la página **Administrar dispositivo**.

Cambie **Velocidad del ventilador** a 500 para enfriar el dispositivo. Elija **Guardar** para actualizar el dispositivo. Cuando el dispositivo confirma el cambio en la configuración, el estado del valor cambia a **sincronizado**:

![Actualización de la configuración](media/quick-monitor-devices/change-settings.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a hacer lo siguiente:

* Recibir una notificación
* Investigar un problema
* Corregir un problema

Ahora que sabe cómo supervisar un dispositivo, el siguiente paso que se recomienda es:

> [!div class="nextstepaction"]
> [Cree y administre una plantilla de dispositivo](howto-set-up-template.md).
