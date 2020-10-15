---
title: Solución de problemas del error 401003 de Azure IoT Hub IoTHubUnauthorized
description: Corrección del error 401003 IoTHubUnauthorized
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: f46d41c8287d03cbe9582ed560244cbd85cdeeaa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "81759597"
---
# <a name="401003-iothubunauthorized"></a>401003 IoTHubUnauthorized

En este artículo se describen las causas y las soluciones de los errores **401003 IoTHubUnauthorized**.

## <a name="symptoms"></a>Síntomas

### <a name="symptom-1"></a>Síntoma 1

En los registros de diagnóstico verá un patrón de dispositivos que se están desconectando con **401003 IoTHubUnauthorized**, seguido de **404104 DeviceConnectionClosedRemotely** y que se conectan correctamente poco después.

### <a name="symptom-2"></a>Síntoma 2

Las solicitudes a IoT Hub producen un error con uno de los siguientes mensajes de error:

* Authorization header missing (Falta el encabezado de autorización)
* IotHub '\*' does not contain the specified device '\*' ("*" en IotHub no contiene el dispositivo "*" especificado)
* Authorization rule '\*' does not allow access for '\*' (La regla de autorización "*" no permite el acceso a "*")
* Authentication failed for this device, renew token or certificate and reconnect (Error de autenticación para este dispositivo, renueve el token o el certificado y vuelva a realizar la conexión)
* Thumbprint does not match configuration: Thumbprint: SHA1Hash=\*, SHA2Hash=\*; Configuration: PrimaryThumbprint=\*, SecondaryThumbprint=\* (La huella digital no coincide con la configuración: Thumbprint: SHA1Hash=<1>, SHA2Hash=<2>; Configuration: PrimaryThumbprint=<3>, SecondaryThumbprint=<4>)

## <a name="cause"></a>Causa

### <a name="cause-1"></a>Causa 1

Con el protocolo MQTT, algunos SDK se basan en IoT Hub para emitir la desconexión cuando el token de SAS expira para saber cuándo actualizarlo. Por lo tanto, 

1. el token de SAS expira,
1. IoT Hub lo reconoce y desconecta el dispositivo con **401003 IoTHubUnauthorized**,
1. el dispositivo completa la desconexión con **404104 DeviceConnectionClosedRemotely**,
1. el SDK de IoT genera un nuevo token de SAS y
1. el dispositivo se vuelve a conectar con IoT Hub correctamente.

### <a name="cause-2"></a>Causa 2

IoT Hub no ha podido autenticar el encabezado de autenticación, la regla o la clave.

## <a name="solution"></a>Solución

### <a name="solution-1"></a>Solución 1

Si se usa el SDK de IoT para la conexión mediante la cadena de conexión del dispositivo, no es necesario realizar ninguna acción. El SDK de IoT regenera el token nuevo para volver a realizar la conexión tras la expiración del token de SAS. 

Si el volumen de errores es un problema, cambie al SDK de C, que renueva el token de SAS antes de la expiración. Además, con el protocolo AMQP, el token de SAS puede actualizarse sin desconexión.

### <a name="solution-2"></a>Solución 2

En general, el mensaje de error presentado debe explicar cómo corregir el error. Si por alguna razón no tiene acceso a los detalles del mensaje de error, asegúrese de lo siguiente:

- Que la firma de acceso compartido u otro token de seguridad que use no haya expirado. 
- Que la credencial de autorización tiene el formato correcto para el protocolo utilizado. Para más información, consulte [Control de acceso a IoT Hub](iot-hub-devguide-security.md).
- La regla de autorización empleada tiene permiso para la operación solicitada.

## <a name="next-steps"></a>Pasos siguientes

Para facilitar la autenticación en IoT Hub, se recomiendan los [SDK de IoT de Azure](iot-hub-devguide-sdks.md).