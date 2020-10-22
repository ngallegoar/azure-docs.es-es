---
title: Administración de IoT Central mediante programación | Microsoft Docs
description: En este artículo se describe cómo crear y administrar IoT Central mediante programación. Puede ver, modificar y quitar la aplicación con SDK de varios lenguajes como JavaScript, Python, C#, Ruby y Go.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 05/19/2020
ms.topic: how-to
ms.openlocfilehash: 773793c7681890098fea1a37cc5b9912c0ecb75c
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92123001"
---
# <a name="manage-iot-central-programmatically"></a>Administración de IoT Central mediante programación

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

En lugar de crear y administrar aplicaciones de IoT Central desde el sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral), puede administrarlas mediante programación con los SDK de Azure. Los lenguajes admitidos son JavaScript, Python, C#, Ruby y Go.

## <a name="install-the-sdk"></a>Instalación del SDK

En la tabla siguiente se enumeran los repositorios de SDK y los comandos de instalación de paquetes:

| Repositorio de SDK | Instalación del paquete |
| -------------- | ------------ |
| [SDK de Azure IotCentralClient para JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/iotcentral/arm-iotcentral) | `npm install @azure/arm-iotcentral` |
| [SDK de Microsoft Azure para Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/iothub/azure-mgmt-iotcentral/azure/mgmt/iotcentral) | `pip install azure-mgmt-iotcentral` |
| [SDK de Azure para .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/iotcentral/Microsoft.Azure.Management.IotCentral) | `dotnet add package Microsoft.Azure.Management.IotCentral` |
| [SDK de Microsoft Azure para Ruby: administración de recursos (versión preliminar)](https://github.com/Azure/azure-sdk-for-ruby/tree/master/management/azure_mgmt_iot_central/lib/2018-09-01/generated/azure_mgmt_iot_central) | `gem install azure_mgmt_iot_central` |
| [SDK de Azure para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/iotcentral) | [Paquete Maven](https://search.maven.org/search?q=a:azure-mgmt-iotcentral) |
| [Azure SDK para Go](https://github.com/Azure/azure-sdk-for-go/tree/master/services/iotcentral/mgmt/2018-09-01/iotcentral) | [Versiones de paquetes](https://github.com/Azure/azure-sdk-for-go/releases) |

## <a name="samples"></a>Ejemplos

En el [repositorio de ejemplos del SDK ARM de Azure IoT Central](/samples/azure-samples/azure-iot-central-arm-sdk-samples/azure-iot-central-arm-sdk-samples/) se incluyen ejemplos de código para varios lenguajes de programación en los que se muestra cómo crear, actualizar, enumerar y eliminar aplicaciones de Azure IoT Central.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar aplicaciones de Azure IoT Central mediante programación, como paso siguiente se sugiere obtener más información sobre el servicio [Azure Resource Manager](../../azure-resource-manager/management/overview.md).