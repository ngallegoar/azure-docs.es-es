---
title: Compatibilidad con TLS en Azure IoT Hub
description: Procedimientos recomendados a la hora de usar conexiones TLS seguras para dispositivos y servicios que se comunican con IoT Hub.
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: jlian
ms.openlocfilehash: 8c52037684215d1672ed813389d0bbace9a03e42
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85080619"
---
# <a name="tls-support-in-iot-hub"></a>Compatibilidad con TLS en IoT Hub

IoT Hub usa Seguridad de la capa de transporte (TLS) para proteger las conexiones de los dispositivos y servicios de IoT. Actualmente se admiten tres versiones del protocolo TLS: 1.0, 1.1 y 1.2.

TLS 1.0 y 1.1 se consideran versiones heredadas y está previsto que dejen de usarse próximamente. Para obtener más información, vea [Desuso de TLS 1.0 y 1.1 en IoT Hub](iot-hub-tls-deprecating-1-0-and-1-1.md). Se recomienda encarecidamente usar TLS 1.2 como la versión de TLS preferida al conectarse a IoT Hub.

## <a name="tls-12-enforcement-available-in-select-regions"></a>Aplicación TLS 1.2 disponible en regiones seleccionadas

Para mayor seguridad, configure IoT Hub para que *solo* permita conexiones de clientes que usen la versión 1.2 de TLS y para que exija el uso de [cifrados recomendados](#recommended-ciphers). Esta característica solo se admite en estas regiones:

* Este de EE. UU.
* Centro-sur de EE. UU.
* Oeste de EE. UU. 2
* US Gov: Arizona
* US Gov - Virginia

Para ello, aprovisione un nuevo recurso de IoT Hub en cualquiera de las regiones admitidas y establezca la propiedad `minTlsVersion` como `1.2` en la especificación de recursos de IoT Hub de la plantilla de Azure Resource Manager:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Devices/IotHubs",
            "apiVersion": "2020-01-01",
            "name": "<provide-a-valid-resource-name>",
            "location": "<any-of-supported-regions-below>",
            "properties": {
                "minTlsVersion": "1.2"
            },
            "sku": {
                "name": "<your-hubs-SKU-name>",
                "tier": "<your-hubs-SKU-tier>",
                "capacity": 1
            }
        }
    ]
}
```

El recurso de IoT Hub creado con esta configuración rechazará los clientes de servicios y dispositivos que intenten conectarse mediante las versiones 1.0 y 1.1 de TLS. Del mismo modo, se rechazará el protocolo de enlace TLS si el mensaje HELLO del cliente no incluye ninguno de los [cifrados recomendados](#recommended-ciphers).

> [!NOTE]
> La propiedad `minTlsVersion` es de solo lectura y no se puede cambiar una vez creado el recurso de IoT Hub. Por lo tanto, es esencial que compruebe y valide de antemano que *todos* los dispositivos y servicios de IoT son compatibles con TLS 1.2 y con los [cifrados recomendados](#recommended-ciphers).
> 
> En caso de conmutaciones por error, la propiedad `minTlsVersion` de su IoT Hub seguirá siendo efectiva en la región con emparejamiento geográfico.

## <a name="recommended-ciphers"></a>Cifrados recomendados

Los recursos de IoT Hub que están configurados para aceptar solo TLS 1.2 también exigirán el uso de estos cifrados recomendados:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

En el caso de los recursos de IoT Hub que no están configurados para exigir TLS 1.2, este protocolo seguirá funcionando con los siguientes cifrados:

* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_DHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_DHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_CBC_SHA256`
* `TLS_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="use-tls-12-in-your-iot-hub-sdks"></a>Uso de TLS 1.2 en los SDK de IoT Hub

Use los vínculos enumerados abajo para configurar TLS 1.2 y los cifrados permitidos en los SDK de cliente de IoT Hub.

| Idioma | Versiones compatibles con TLS 1.2 | Documentación |
|----------|------------------------------------|---------------|
| C        | Etiqueta 2019-12-11 o más reciente            | [Vínculo](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Versión 2.0.0 o más reciente             | [Vínculo](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Versión 1.21.4 o más reciente            | [Vínculo](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Versión 1.19.0 o más reciente            | [Vínculo](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Versión 1.12.2 o más reciente            | [Vínculo](https://aka.ms/Tls_Node_SDK_IoT) |


## <a name="use-tls-12-in-your-iot-edge-setup"></a>Uso de TLS 1.2 en la configuración de IoT Edge

Los dispositivos de IoT Edge se pueden configurar para que usen TLS 1.2 al comunicarse con IoT Hub. Para más información sobre cómo hacerlo, vea la [página de documentación de IoT Edge](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md).