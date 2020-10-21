---
title: Preguntas más frecuentes sobre Azure IoT Central | Microsoft Docs
description: Preguntas más frecuentes (P+F) y respuestas sobre Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 09/23/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: f9c7412afcc191470902cc256586f9db21f8e78c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91852063"
---
# <a name="frequently-asked-questions-for-iot-central"></a>Preguntas más frecuentes sobre IoT Central

**¿Cómo puedo comprobar si hay problemas con las credenciales si un dispositivo no se conecta a la aplicación IoT Central?**

En el artículo [Solucionar el motivo por el que los datos de los dispositivos no aparecen en Azure IoT Central](troubleshoot-connection.md) se incluyen los pasos para diagnosticar los problemas de conectividad de los dispositivos.

**¿Cómo puedo abrir una incidencia en el departamento de asistencia al cliente?**

Si necesita ayuda, puede enviar una [incidencia de soporte técnico de Azure](https://portal.azure.com/#create/Microsoft.Support).

Para obtener más información, incluidas otras opciones de soporte técnico, consulte las [Opciones de ayuda y soporte técnico de Azure IoT](../../iot-fundamentals/iot-support-help.md).

**¿Cómo puedo desbloquear un dispositivo?**

Cuando están bloqueados, no pueden enviar datos a la aplicación de IoT Central. Los dispositivos bloqueados tienen el estado **Bloqueado** en la página de **Dispositivos** de la aplicación. Un operador debe desbloquear el dispositivo para que pueda volver a enviar datos:

:::image type="content" source="media/howto-faq/blocked.png" alt-text="Captura de pantalla que muestra el dispositivo bloqueado":::

Cuando un operador desbloquea un dispositivo, el estado vuelve a su valor anterior, **Registrado** o **Aprovisionado**.

**¿Cómo puedo aprobar un dispositivo?**

Si el estado del dispositivo es **Esperando aprobación** en la página de **Dispositivos**, significa que la opción **Aprobación automática** está deshabilitada:

:::image type="content" source="media/howto-faq/auto-approve.png" alt-text="Captura de pantalla que muestra el dispositivo bloqueado":::

Un operador debe aprobar explícitamente un dispositivo antes de que empiece a enviar datos. Los dispositivos no registrados manualmente en la página **Dispositivos**, pero conectados con credenciales válidas, tendrán el estado del dispositivo **En espera de aprobación**. Los operadores pueden aprobar estos dispositivos desde la página **Dispositivos** mediante el botón **Aprobar**:

:::image type="content" source="media/howto-faq/approve-device.png" alt-text="Captura de pantalla que muestra el dispositivo bloqueado":::

**¿Cómo puedo asociar un dispositivo a una plantilla de dispositivo?**

Si el estado del dispositivo es **Unassociated** (No asociado), significa que el dispositivo que se conecta a IoT Central no tiene una plantilla de dispositivo asociada. Esta situación suele darse en los escenarios siguientes:

- Se agrega un conjunto de dispositivos mediante la opción **Import** (Importar) de la página **Devices** (Dispositivos) sin especificar la plantilla de dispositivo.
- Se registró un dispositivo manualmente en la página **Devices** (Dispositivos) sin especificar la plantilla de dispositivo. Después, el dispositivo se conecta con credenciales válidas.  

El operador puede asociar un dispositivo a una plantilla de dispositivo desde la página **Dispositivos** mediante el botón **Migrar**. Para obtener más información, consulte [Administración de dispositivos en la aplicación de Azure IoT Central > Migración de dispositivos a una plantilla](howto-manage-devices.md).

**¿Dónde puedo obtener más información sobre IoT Hub?**

Azure IoT Central usa Azure IoT Hub como una puerta de enlace en la nube que permite la conectividad del dispositivo. IoT Hub permite lo siguiente:

- Ingesta de datos a escala en la nube.
- Administración de dispositivos.
- Conectividad de dispositivos segura.

Para más información acerca de IoT Hub, consulte [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

**¿Dónde puedo obtener más información sobre Device Provisioning Service (DPS)?**

Azure IoT Central usa DPS para permitir que los dispositivos se conecten a la aplicación. Para obtener más información sobre el rol de DPS mediante el cual se conectan los dispositivos a IoT Central, consulte [Conectarse a Azure IoT Central](concepts-get-connected.md). Para obtener más información sobre DPS, consulte [Aprovisionamiento de dispositivos con Azure IoT Hub Device Provisioning Service](../../iot-dps/about-iot-dps.md).