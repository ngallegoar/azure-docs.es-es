---
title: Compatibilidad de TLS en Azure IoT Hub Device Provisioning Service (DPS)
description: Procedimientos recomendados para usar conexiones TLS seguras para dispositivos y servicios que se comunican con IoT Hub Device Provisioning Service (DPS).
services: iot-dps
author: wesmc7777
ms.service: iot-dps
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: wesmc
ms.openlocfilehash: 285832d80d37c8553ffc8e37c6f6eab5d7f6d943
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82984259"
---
# <a name="tls-support-in-azure-iot-hub-device-provisioning-service-dps"></a>Compatibilidad de TLS en Azure IoT Hub Device Provisioning Service (DPS)

DPS utiliza Seguridad de la capa de transporte (TLS) para proteger las conexiones de los dispositivos IoT. Actualmente se admiten tres versiones del protocolo TLS: 1.0, 1.1 y 1.2.

TLS 1.0 y 1.1 se consideran versiones heredadas y está previsto que dejen de usarse próximamente. Para obtener más información, vea [Desuso de TLS 1.0 y 1.1 en IoT Hub](../iot-hub/iot-hub-tls-deprecating-1-0-and-1-1.md). Se recomienda encarecidamente usar TLS 1.2 como la versión de TLS preferida al conectarse a DPS.

## <a name="restrict-connections-to-tls-12"></a>Restricción de las conexiones a TLS 1.2

Para mayor seguridad, se recomienda configurar las instancias de DPS para que *solo* permitan conexiones de cliente de dispositivos que usen la versión 1.2 de TLS y exigir el uso de [cifrados recomendados](#recommended-ciphers).

Para ello, aprovisione un nuevo recurso de DPS en cualquiera de las [regiones admitidas](#supported-regions) y establezca la propiedad `minTlsVersion` en `1.2` en la especificación de recursos de DPS de la plantilla de Azure Resource Manager. En la plantilla de ejemplo siguiente, JSON especifica la propiedad `minTlsVersion` de una nueva instancia de DPS.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Devices/ProvisioningServices",
            "apiVersion": "2020-01-01",
            "name": "<provide-a-valid-DPS-resource-name>",
            "location": "<any-of-supported-regions-below>",
            "properties": {
                "minTlsVersion": "1.2"
            },
            "sku": {
                "name": "S1",
                "capacity": 1
            },
        }     
    ]
}
```

Puede implementar la plantilla con el siguiente comandos de la CLI de Azure. 

```azurecli
az deployment group create -g <your resource group name> --template-file template.json
```

Para más información sobre cómo crear recursos de DPS con plantillas de Resource Manager, consulte [Configuración de DPS con una plantilla de Azure Resource Manager](quick-setup-auto-provision-rm.md).

El recurso de DPS creado con esta configuración rechazará los dispositivos que intenten conectarse mediante las versiones 1.0 y 1.1 de TLS. Del mismo modo, se rechazará el protocolo de enlace TLS si el mensaje HELLO del cliente del dispositivo no incluye ninguno de los [cifrados recomendados](#recommended-ciphers).

> [!NOTE]
> La propiedad `minTlsVersion` es de solo lectura y no se puede cambiar una vez creado el recurso de DPS. Por lo tanto, es esencial que compruebe y valide de antemano que *todos* los dispositivos de IoT son compatibles con TLS 1.2 y con los [cifrados recomendados](#recommended-ciphers).

## <a name="supported-regions"></a>Regiones admitidas

Las instancias de IoT DPS que necesitan el uso de TLS 1.2 se pueden crear en estas regiones:

* US Gov: Arizona
* US Gov - Virginia

> [!NOTE]
> En caso de conmutaciones por error, la propiedad `minTlsVersion` de su DPS seguirá siendo efectiva en la región con emparejamiento geográfico después de la conmutación por error.

## <a name="recommended-ciphers"></a>Cifrados recomendados

Las instancias de DPS que están configuradas para aceptar solo TLS 1.2 también exigirán el uso de estos cifrados recomendados:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

## <a name="use-tls-12-in-the-iot-sdks"></a>Uso de TLS 1.2 en los SDK de IoT

Use los vínculos siguientes para configurar TLS 1.2 y los cifrados permitidos en los SDK de cliente de Azure IoT.

| Idioma | Versiones compatibles con TLS 1.2 | Documentación |
|----------|------------------------------------|---------------|
| C        | Etiqueta 2019-12-11 o más reciente            | [Vínculo](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Versión 2.0.0 o más reciente             | [Vínculo](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Versión 1.21.4 o más reciente            | [Vínculo](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Versión 1.19.0 o más reciente            | [Vínculo](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Versión 1.12.2 o más reciente            | [Vínculo](https://aka.ms/Tls_Node_SDK_IoT) |


## <a name="use-tls-12-with-iot-edge"></a>Uso de TLS 1.2 con IoT Edge

Los dispositivos de IoT Edge se pueden configurar para que usen TLS 1.2 al comunicarse con IoT Hub y DPS. Para más información sobre cómo hacerlo, vea la [página de documentación de IoT Edge](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md).