---
title: Crear los recursos técnicos de un módulo Azure IoT Edge | Azure Marketplace
description: Cree los recursos técnicos de un módulo IoT Edge.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 1a523872bb15981958eeb1678caa1d911a9cbbba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142405"
---
# <a name="prepare-your-iot-edge-module-technical-assets"></a>Preparar los recursos técnicos del módulo IoT Edge

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de módulos de IoT Edge al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de módulo de IoT Edge](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-iot-edge-module-asset) para administrar las ofertas migradas.

En este artículo se describen los requisitos que deben cumplir los recursos técnicos del módulo IoT Edge para que se pueda publicar en Azure Marketplace.

## <a name="understanding-iot-edge-modules-and-getting-started"></a>Descripción de los módulos IoT Edge e introducción

Un módulo IoT Edge es un contenedor compatible con Docker diseñado para ejecutarse en un dispositivo IoT Edge.

- Para obtener más información sobre los módulos de IoT Edge, vea [Información sobre los módulos de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).
- Para empezar a trabajar en el desarrollo del módulo IoT Edge, vea [Descripción de los requisitos y las herramientas para desarrollar módulos de IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="technical-requirements"></a>Requisitos técnicos

Deben cumplirse los siguientes requisitos técnicos para que el módulo IoT Edge se certifique y se publique en Azure Marketplace.

### <a name="platform-support"></a>Compatibilidad con plataformas

El módulo IoT Edge debe admitir una de las siguientes opciones de plataforma.

#### <a name="tier-1-platforms-supported-by-iot-edge"></a>Plataformas de nivel 1 compatibles con IoT Edge

Admita todas las plataformas de nivel 1 compatibles con IoT Edge (según el registro de [Compatibilidad de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)). Se recomienda esta opción porque proporciona una mejor experiencia de cliente. Se presentarán los módulos que cumplan este criterio. Un módulo que use esta opción de plataforma debe:

- Proporcionar una etiqueta `latest` y una etiqueta de versión (por ejemplo, `1.0.1`) que sean etiquetas de manifiesto compiladas con la [herramienta de manifiestos](https://github.com/estesp/manifest-tool) de GitHub.
- Use la [pestaña Marketplace](./cpp-marketplace-tab.md) para agregar un vínculo a [IoT Edge Certified](https://catalog.azureiotsolutions.com/alldevices?filters={%2218%22:[%221%22]}), un sitio web donde los clientes pueden examinar o buscar dispositivos certificados. Este sitio web también se conoce como el catálogo de dispositivos [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/).

#### <a name="a-subset-of-tier-1-platforms-supported-by-iot-edge"></a>Un subconjunto de plataformas de nivel 1 compatibles con IoT Edge
  
Admita un subconjunto (al menos uno) de plataformas de nivel 1 compatibles con IoT Edge (según el registro de [Compatibilidad de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)). Un módulo que use esta opción de plataforma debe:

- Proporcionar una etiqueta `latest` y una etiqueta de versión (por ejemplo, `1.0.1`) que sean etiquetas de manifiesto compiladas con la [herramienta de manifiestos](https://github.com/estesp/manifest-tool) de GitHub, si hay más de una plataforma compatible. Las etiquetas de manifiesto son opcionales solo cuando hay una plataforma compatible.
- Use la [pestaña Marketplace](./cpp-marketplace-tab.md) para proporcionar un vínculo a al menos un dispositivo IoT Edge del catálogo de dispositivos [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/).

### <a name="device-dimensions"></a>Dimensiones de dispositivos

Las dimensiones de un módulo IoT Edge (CPU/RAM/almacenamiento/GPU/etc.) en dispositivos IoT Edge de destino deben cumplir los siguientes requisitos:

- El módulo debe **trabajar con al menos un dispositivo IoT Edge Certified** del catálogo de dispositivos [Azure IoT Edge Certified](https://catalog.azureiotsolutions.com/).
- Los **requisitos mínimos de hardware** deben quedar documentados como último párrafo de la descripción de la oferta (en la [pestaña Marketplace](./cpp-marketplace-tab.md)). También puede indicar los requisitos de hardware recomendados si difieren considerablemente. Por ejemplo, agregue la siguiente sección al final de la descripción de la oferta:

  ```html
    <p><u>Minimum hardware requirements:</u> Linux x64 and arm32  OS, 1GB of RAM, 500 Mb of storage</p>
  ```

### <a name="configuration"></a>Configuración

También incluye la configuración predeterminada para que la implementación en un dispositivo IoT Edge se realice de la manera más simple posible. Es posible que el contenedor también incluya el SDK del módulo IoT Edge para habilitar la comunicación con EdgeHub e IoT Hub.

#### <a name="default-configuration"></a>Configuración predeterminada

Los módulos IoT Edge se deben poder iniciar con la configuración predeterminada proporcionada en la [pestaña SKU de Cloud Partner Portal](./cpp-skus-tab.md). Están disponibles los siguientes valores predeterminados:

- **Rutas** predeterminadas
- **Propiedades deseadas gemelas** predeterminadas
- **Variables de entorno** predeterminadas
- Elemento **createOptions** predeterminado

En un escenario donde un parámetro necesario para un valor predeterminado no tiene sentido (por ejemplo, la dirección IP del servidor de un cliente), agregue un parámetro como valor predeterminado. Este valor se escribe entre corchetes y en mayúsculas. En este ejemplo, se configura la variable de entorno predeterminada siguiente:

```
    ServerIPAddress = <MY_SERVER_IP_ADDRESS>
```

#### <a name="configuration-documentation"></a>Documentación de configuración

Todos los valores de configuración de un módulo IoT Edge deben quedar documentados claramente (cómo usar sus rutas, propiedades deseadas gemelas, variables de entorno, createOptions, etc.). Proporcione un vínculo a la documentación o convierta la documentación en parte de la descripción de la oferta o SKU.

### <a name="tags-and-versioning"></a>Etiquetas y control de versiones

Los clientes deben poder implementar fácilmente un módulo y obtener automáticamente actualizaciones desde Marketplace (en un escenario de desarrollador). También deben poder usar y congelar una versión exacta que hayan probado (en un escenario de producción).

Para satisfacer las expectativas de estos clientes y publicarse en Marketplace, los módulos IoT Edge deben cumplir los siguientes requisitos:

- Incluir una etiqueta `latest` de manifiesto que apunte a la versión más reciente en todas las plataformas compatibles.
- Las etiquetas de versión deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros.
- Incluir una etiqueta de "versión", como `1.0.1`, que apunte a una versión concreta en todas las plataformas compatibles.
- No actualizar las etiquetas de "versión", como `1.0.1`, ya que deben ser inmutables.

>[!Note]
>Opcionalmente, el control de versiones puede incluir etiquetas "revertir versión", como `2.0` y `1.0`. Esto permite mantener varias versiones principales en paralelo.

### <a name="telemetry"></a>Telemetría

Los módulos que usan el SDK de módulo IoT deben establecer el identificador de módulo único en `PublisherId.OfferId.SkuId` para fines de telemetría. Un identificador único permite a Azure Marketplace identificar la cantidad de instancias de módulos que se están ejecutando.

 Use los métodos siguientes de los SDK de módulo IoT para establecer `ProductInfo` en este identificador:

- [C\#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.productinfo?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_ProductInfo) 
- [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
- [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.productinfo?view=azure-java-stable)

En cuanto a los módulos que no usan el SDK de módulo IoT, puede encontrar detalles menos precisos en Cloud Partner Portal, por ejemplo el número de descargas.

### <a name="security"></a>Seguridad

Los módulos IoT Edge deben pedir el acceso con menos privilegios como sea posible al host. Los [módulos con privilegios](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) deben evitarse.

### <a name="module-iot-sdk"></a>SDK de módulo IoT

Incluir el SDK de módulo IoT no es un requisito previo para realizar la certificación. Pero, si decide incluir el SDK de módulo IoT, puede proporcionar una experiencia de usuario mejor. Por ejemplo, admitir el enrutamiento o el envío de mensajes a la nube.

El SDK de módulo IoT es necesario para obtener datos de telemetría sobre la cantidad de instancias de módulos en ejecución.


## <a name="recertification-process"></a>Proceso de recertificación

<!-- Add legal time windows-->
Los asociados reciben una notificación siempre que hay un cambio importante que afecta a sus módulos, como:

- Matriz de compatibilidad de sistema operativo o arquitectura de nivel 1 compatible con IoT Edge
- SDK de módulo IoT
- Runtime de IoT Edge
- Directrices de certificación de módulo IoT Edge

Los asociados tienen que actualizar sus módulos y volver a certificarlos mediante la herramienta Cloud Partner Portal.

## <a name="host-your-iot-edge-module-in-an-azure-container-registry"></a>Hospedar el módulo IoT Edge en Azure Container Registry

Para cargar el módulo IoT Edge en Cloud Partner Portal, primero debe hospedarlo en [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR). El módulo debe incluir todas las etiquetas que quiere publicar, incluidas las etiquetas de imagen a las que hace referencia una etiqueta de manifiesto.


## <a name="next-steps"></a>Pasos siguientes

- [Creación de una oferta del módulo de IoT Edge](./cpp-create-offer.md)
