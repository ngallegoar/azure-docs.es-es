---
title: Creación de un dispositivo de puerta de enlace transparente con Azure IoT Edge | Microsoft Docs
description: Uso de un dispositivo Azure IoT Edge como una puerta de enlace transparente que puede procesar información para dispositivos de bajada
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/03/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 3b3aeff595671c5f924d01599b572b6b938ef09d
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2020
ms.locfileid: "80666660"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente

Este artículo proporciona instrucciones detalladas para configurar un dispositivo IoT Edge de modo que funcione como una puerta de enlace transparente para que otros dispositivos se comuniquen con IoT Hub. En este artículo, el término *puerta de enlace IoT Edge* hace referencia a un dispositivo IoT Edge configurado como una puerta de enlace transparente. Para más información, consulte [Uso de un dispositivo IoT Edge como puerta de enlace](./iot-edge-as-gateway.md).

>[!NOTE]
>Actualmente:
>
> * Los dispositivos habilitados para Edge no pueden conectarse a puertas de enlace de IoT Edge.
> * Los dispositivos de bajada no pueden usar la carga de archivos.

Hay tres pasos generales para configurar una conexión de puerta de enlace transparente correcta. En este artículo se describe el primer paso:

1. **El dispositivo de puerta de enlace debe ser capaz de conectarse a dispositivos de bajada de forma segura, recibir comunicaciones de dispositivos de bajada y enrutar los mensajes al destino adecuado.**
2. El dispositivo de bajada debe tener una identidad de dispositivo para poder autenticarse con IoT Hub y saber comunicarse a través de su dispositivo de puerta de enlace. Para más información, consulte [Autenticación de un dispositivo de bajada en Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. El dispositivo de bajada se debe poder conectar de forma segura a su dispositivo de puerta de enlace. Para más información, consulte [Conexión de un dispositivo de bajada a una puerta de enlace Azure IoT Edge](how-to-connect-downstream-device.md).

Para que un dispositivo funcione como puerta de enlace, debe poder conectarse de forma segura a sus dispositivos de bajada. Azure IoT Edge le permite usar una infraestructura de clave pública (PKI) para configurar conexiones seguras entre los dispositivos. En este caso, vamos a permitir que un dispositivo de bajada se conecte a un dispositivo IoT Edge que actúa como puerta de enlace transparente. Para mantener una seguridad razonable, el dispositivo de bajada debe confirmar la identidad del dispositivo de puerta de enlace. Esta comprobación de identidad evita que los dispositivos se conecten a puertas de enlace que pueden ser malintencionadas.

Un dispositivo de bajada en un escenario de puerta de enlace transparente puede ser cualquier aplicación o plataforma que tenga una identidad creada con el servicio en la nube [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub). En muchos casos, estas aplicaciones utilizan el [SDK de dispositivo IoT de Azure](../iot-hub/iot-hub-devguide-sdks.md). En la práctica, un dispositivo de bajada podría ser incluso una aplicación que se ejecuta en el propio dispositivo de puerta de enlace IoT Edge. Sin embargo, un dispositivo IoT Edge no puede ser inferior a una puerta de enlace de IoT Edge.

Puede crear cualquier infraestructura de certificados que permita la confianza necesaria para la topología de la puerta de enlace de dispositivo. En este artículo se da por hecho que usa la misma configuración de certificado que usaría para habilitar la [seguridad de entidad de certificación X.509](../iot-hub/iot-hub-x509ca-overview.md) en IoT Hub, lo que implica un certificado de entidad de certificación X.509 asociado a un centro de IoT específico (la entidad de certificación raíz del centro de IoT), una serie de certificados firmados con esta entidad de certificación y una entidad de certificación para el dispositivo IoT Edge.

![Configuración del certificado de puerta de enlace](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>El término "entidad de certificación raíz" utilizado en este artículo hace referencia al certificado público de entidad de nivel superior de la cadena de certificados de PKI y no necesariamente a la raíz del certificado de una entidad de certificación sindicada. En muchos casos, se trata realmente de un certificado público intermedio de la entidad de certificación.

El demonio de seguridad de IoT Edge usa el certificado de entidad de certificación de dispositivo IoT Edge para firmar un certificado de entidad de certificación de carga de trabajo, que a su vez firma un certificado de servidor para el centro de IoT Edge. La puerta de enlace presenta su certificado de servidor al dispositivo de bajada durante el inicio de la conexión. El dispositivo de bajada realiza comprobaciones para asegurarse de que el certificado de servidor forma parte de una cadena de certificados que se acumula en el certificado de entidad de certificación raíz. Este proceso permite que el dispositivo de bajada confirme que la puerta de enlace procede de un origen de confianza. Para obtener más información, consulta [Información sobre los certificados de Azure IoT Edge](iot-edge-certs.md).

Los pasos siguientes le guían por el proceso de crear los certificados e instalarlos en los lugares adecuados de la puerta de enlace. Puede usar cualquier máquina para generar los certificados y, a continuación, copiarlos en el dispositivo IoT Edge.

## <a name="prerequisites"></a>Prerrequisitos

Un dispositivo Azure IoT Edge, configurado con [certificados de producción](how-to-manage-device-certificates.md).

## <a name="deploy-edgehub-to-the-gateway"></a>Implementación de EdgeHub en la puerta de enlace

Cuando se instala IoT Edge por primera vez en un dispositivo, se inicia automáticamente un único módulo del sistema: el agente de IoT Edge. Una vez que se crea la primera implementación en un dispositivo, también se inicia el segundo módulo del sistema, el centro de IoT Edge.

El centro de IoT Edge es responsable de recibir los mensajes entrantes de los dispositivos de bajada y enrutarlos al siguiente destino. Si el módulo **edgeHub** no está en ejecución en el dispositivo, cree una implementación inicial para el dispositivo. La implementación parecerá vacía porque no agrega ningún módulo, pero garantizará que se ejecutan ambos módulos del sistema.

Puede comprobar los módulos que se ejecutan en un dispositivo mediante la comprobación de los detalles del dispositivo en Azure Portal, la visualización del estado del dispositivo en Visual Studio o Visual Studio Code o mediante la ejecución del comando `iotedge list` en el propio dispositivo.

Si el módulo **edgeAgent** se ejecuta sin el módulo **edgeHub**, siga estos pasos:

1. En Azure Portal, vaya hasta el centro de IoT.

2. Vaya a **IoT Edge** y seleccione el dispositivo IoT Edge que quiere usar como puerta de enlace.

3. Seleccione **Set modules** (Establecer módulos).

4. Seleccione **Next** (Siguiente).

5. En la página **Specify routes** (Especificar rutas), debe tener una ruta predeterminada que envíe todos los mensajes de todos los módulos a IoT Hub. Si no es así, agregue el código siguiente y seleccione **Next** (Siguiente).

   ```JSON
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

6. En la página **Review template** (Revisar plantilla), seleccione **Submit** (Enviar).

## <a name="open-ports-on-gateway-device"></a>Apertura de puertos en el dispositivo de puerta de enlace

Los dispositivos IoT Edge estándar no necesitan conectividad entrante a para funcionar, porque toda la comunicación con IoT Hub se realiza a través de conexiones salientes. Los dispositivos de puerta de enlace son diferentes porque deben recibir mensajes de sus dispositivos de bajada. Si hay un firewall entre los dispositivos de bajada y el dispositivo de puerta de enlace, la comunicación también debe ser posible a través de él.

Para que un escenario de puerta de enlace funcione, al menos uno de los protocolos admitidos en el centro de IoT Edge debe estar abierto para el tráfico entrante procedente de los dispositivos de bajada. Los protocolos admitidos son MQTT, AMQP, HTTPS, MQTT sobre WebSockets y AMQP sobre WebSockets.

| Port | Protocolo |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT + WS <br> AMQP + WS |

## <a name="route-messages-from-downstream-devices"></a>Enrutamiento de mensajes desde dispositivos de bajada

El entorno de ejecución de Azure IoT Edge puede enrutar los mensajes enviados desde dispositivos de bajada igual que los mensajes enviados por los módulos. Esta característica permite realizar análisis en un módulo que se ejecuta en la puerta de enlace antes de enviar datos a la nube.

Actualmente, la manera de enrutar los mensajes enviados por los dispositivos de bajada es diferenciándolos de los mensajes enviados por los módulos. Los mensajes enviados por los módulos contienen una propiedad del sistema denominada **connectionModuleId**, pero los mensajes enviados por los dispositivos de bajada no la tienen. Puede usar la cláusula WHERE de la ruta para excluir los mensajes que contengan dicha propiedad del sistema.

La siguiente ruta es un ejemplo que enviaría mensajes desde cualquier dispositivo de bajada a un módulo denominado `ai_insights` y, a continuación, desde `ai_insights` a IoT Hub.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")",
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream"
    }
}
```

Para obtener más información sobre el enrutamiento de mensajes, vea [Implementar módulos y establecer rutas](./module-composition.md#declare-routes).

## <a name="enable-extended-offline-operation"></a>Habilitación del funcionamiento sin conexión extendido

A partir de la [versión v1.0.4](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) del entorno de ejecución de Azure IoT Edge, el dispositivo de puerta de enlace y los dispositivos de bajada conectados a él pueden configurarse para un funcionamiento sin conexión extendido.

Con esta funcionalidad, los módulos locales o los dispositivos de bajada pueden volver a autenticarse con el dispositivo IoT Edge según sea necesario y comunicarse entre sí mediante mensajes y métodos, aunque estén desconectados de IoT Hub. Para obtener más información, consulte [Understand extended offline capabilities for IoT Edge devices, modules, and child devices](offline-capabilities.md) (Entender las capacidades sin conexión extendidas para dispositivos IoT Edge, módulos y dispositivos secundarios).

Para habilitar la funcionalidad sin conexión extendida, se debe establecer una relación principal-secundario entre el dispositivo de puerta de enlace IoT Edge y los dispositivos de bajada que se conectarán a él. Estos pasos se explican con más detalle en [Authenticate a downstream device to Azure IoT Hub](how-to-authenticate-downstream-device.md) (Autenticación de un dispositivo de bajada en Azure IoT Hub).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge que funciona como una puerta de enlace transparente, deberá configurar los dispositivos de bajada para que confíen en la puerta de enlace y envíen mensajes. Continúe con [Autenticación de un dispositivo de bajada en Azure IoT Hub](how-to-authenticate-downstream-device.md) para ver los pasos siguientes en la configuración del escenario de puerta de enlace transparente.
