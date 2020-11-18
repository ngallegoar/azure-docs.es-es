---
title: Compatibilidad con TLS en Azure IoT Hub
description: Procedimientos recomendados a la hora de usar conexiones TLS seguras para dispositivos y servicios que se comunican con IoT Hub.
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 11/13/2020
ms.author: jlian
ms.openlocfilehash: c9dd66fe9d71f0a857e4b0821190bceb5d6d4680
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94628805"
---
# <a name="tls-support-in-iot-hub"></a>Compatibilidad con TLS en IoT Hub

IoT Hub usa Seguridad de la capa de transporte (TLS) para proteger las conexiones de los dispositivos y servicios de IoT. Actualmente se admiten tres versiones del protocolo TLS: 1.0, 1.1 y 1.2.

TLS 1.0 y 1.1 se consideran versiones heredadas y está previsto que dejen de usarse próximamente. Para obtener más información, vea [Desuso de TLS 1.0 y 1.1 en IoT Hub](iot-hub-tls-deprecating-1-0-and-1-1.md). Se recomienda encarecidamente usar TLS 1.2 como la versión de TLS preferida al conectarse a IoT Hub.

## <a name="tls-12-enforcement-available-in-select-regions"></a>Aplicación TLS 1.2 disponible en regiones seleccionadas

Para mayor seguridad, configure IoT Hub para que *solo* permita conexiones de clientes que usen la versión 1.2 de TLS y para que exija el uso de [conjuntos de cifrado](#cipher-suites). Esta característica solo se admite en estas regiones:

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

El recurso de IoT Hub creado con esta configuración rechazará los clientes de servicios y dispositivos que intenten conectarse mediante las versiones 1.0 y 1.1 de TLS. Del mismo modo, se rechazará el protocolo de enlace TLS si el mensaje `ClientHello` no incluye ninguno de los [cifrados recomendados](#cipher-suites).

> [!NOTE]
> La propiedad `minTlsVersion` es de solo lectura y no se puede cambiar una vez creado el recurso de IoT Hub. Por lo tanto, es esencial que compruebe y valide de antemano que *todos* los dispositivos y servicios de IoT son compatibles con TLS 1.2 y con los [cifrados recomendados](#cipher-suites).
> 
> En caso de conmutaciones por error, la propiedad `minTlsVersion` de su IoT Hub seguirá siendo efectiva en la región con emparejamiento geográfico.

## <a name="cipher-suites"></a>Conjuntos de cifrado

Los recursos de IoT Hub que están configurados para aceptar solo TLS 1.2 también exigirán el uso de estos conjuntos de cifrado recomendados:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

En el caso de los recursos de IoT Hub que no están configurados para exigir TLS 1.2, este protocolo seguirá funcionando con los siguientes conjuntos de cifrado:

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

Un cliente puede sugerir una lista de conjuntos de cifrado mayores para usar durante el mensaje `ClientHello`. Sin embargo, es posible que no se admitan algunos de los recursos de IoT Hub (por ejemplo, `ECDHE-ECDSA-AES256-GCM-SHA384`). En este caso, IoT Hub intentará seguir las preferencias del cliente, pero finalmente negociará el conjunto de cifrado con `ServerHello`.

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

## <a name="device-authentication"></a>Autenticación de dispositivos

Después de un protocolo de enlace de TLS correcto, IoT Hub puede autenticar un dispositivo mediante una clave simétrica o un certificado X.509. En el caso de la autenticación basada en certificados, puede tratarse de cualquier certificado X.509, incluido ECC. IoT Hub valida el certificado con la huella digital o la entidad de certificación (CA) que proporcione. Para obtener más información, consulte [Certificados X.509 compatibles](iot-hub-devguide-security.md#supported-x509-certificates).
