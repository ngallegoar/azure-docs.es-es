---
title: Creación de webhooks para reglas en Azure IoT Central | Microsoft Docs
description: Cree webhooks en Azure IoT Central para notificar automáticamente a otras aplicaciones cuándo se activan las reglas.
author: viv-liu
ms.author: viviali
ms.date: 12/02/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: d97bd7a3c6de92f22a9880040f407960d5257f6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158102"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Creación de acciones de webhook para reglas en Azure IoT Central

*Este tema se aplica a los compiladores y administradores.*

Los webhooks permiten conectar la aplicación de IoT Central a otras aplicaciones y servicios para supervisión remota y notificaciones. Los webhooks notifican automáticamente a otras aplicaciones y servicios a los que se conecta si se activa una regla en la aplicación IoT Central. Su aplicación IoT Central envía una solicitud POST al otro punto de conexión HTTP de la aplicación siempre que se desencadena una regla. La carga útil contiene detalles del dispositivo y del desencadenador de la regla.

## <a name="set-up-the-webhook"></a>Configuración del webhook

En este ejemplo, se conectará a RequestBin para obtener notificaciones mediante webhooks cuando se activan las reglas.

1. Abra [RequestBin](https://requestbin.net/).

1. Cree una instancia de RequestBin y copie la **dirección URL de la papelera**.

1. Cree una [regla de telemetría](tutorial-create-telemetry-rules.md). Guarde la regla y agregue una nueva acción.

    ![Pantalla de creación de webhooks](media/howto-create-webhooks/webhookcreate.png)

1. Elija la acción de webhook y proporcione un nombre para mostrar y pegue la dirección URL de la papelera como la dirección URL de devolución de llamada.

1. Guarde la regla.

Ahora, cuando se desencadena la regla, verá que aparece una nueva solicitud en RequestBin.

## <a name="payload"></a>Carga

Cuando se desencadena una regla, se realiza una solicitud HTTP POST a la dirección URL de devolución de llamada que contiene una carga útil JSON con los detalles de telemetría, dispositivo, regla y aplicación. La carga útil podría ser similar a la siguiente:

```json
{
    "id": "<id>",
    "displayName": "Webhook 1",
    "timestamp": "2019-10-24T18:27:13.538Z",
    "rule": {
        "id": "<id>",
        "displayName": "High temp alert",
        "enabled": true
    },
    "device": {
        "id": "mx1",
        "displayName": "MXChip IoT DevKit - mx1",
        "instanceOf": "<device-template-id>",
        "simulated": true,
        "provisioned": true,
        "approved": true
    },
    "data": [{
        "@id": "<id>",
        "@type": ["Telemetry"],
        "name": "temperature",
        "displayName": "Temperature",
        "value": 66.27310467496761,
        "interfaceInstanceName": "sensors"
    }],
    "application": {
        "id": "<id>",
        "displayName": "x - Store Analytics Checkout---PnP",
        "subdomain": "<subdomain>",
        "host": "<host>"
    }
}
```

## <a name="known-limitations"></a>Restricciones conocidas

Actualmente no hay ningún mecanismo de programación para suscribir o cancelar la suscripción de estos webhooks mediante una API.

Si tiene ideas sobre cómo mejorar esta característica, envíe sus sugerencias a nuestro [foro de UserVoice](https://feedback.azure.com/forums/911455-azure-iot-central).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar y usar webhooks, el siguiente paso sugerido es explorar la [configuración de grupos de acciones de Azure Monitor](howto-use-action-groups.md).
