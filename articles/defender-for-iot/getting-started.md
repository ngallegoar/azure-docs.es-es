---
title: Opciones de implementación
description: Empiece a comprender el flujo de trabajo básico de las características y el servicio de Defender for IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: 6aa525fd7f2d82194baa2e2a0c910cb71509c2d5
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96340023"
---
# <a name="getting-started-with-azure-defender-for-iot"></a>Introducción a Azure Defender para IoT

En este artículo se describen los procesos de implementación e incorporación necesarios para poner en funcionamiento Azure Defender para IoT. No es necesario efectuar pasos adicionales. Se recomienda que lea estos pasos y se familiarice con la información de la documentación complementaria.

Una vez completados todos los pasos, los sensores de Azure Defender para IoT supervisarán la red. En función de cómo se configure la solución, también se pueden transferir detecciones a la consola de administración local o al centro de IoT.

Complete los pasos siguientes para poner en funcionamiento Azure Defender para IoT.

## <a name="1-set-up-azure"></a>1. Configuración de Azure

- Configure una cuenta de Azure. Para más información, consulte [Creación de una cuenta de Azure](/learn/modules/create-an-azure-account/).

- Firewall o proxy: Si tiene un firewall o un dispositivo de red similar que esté configurado para permitir conexiones específicas, compruebe que *.azure-devices.net:443 está abierto en el firewall o el proxy. Si no se admiten comodines o desea tener más control, el nombre de dominio completo específico del centro IoT se debería abrir en el firewall o el proxy. Para obtener más información, consulte [Referencia: Puntos de conexión de IoT Hub](../iot-hub/iot-hub-devguide-endpoints.md).

## <a name="2-deploy-hardware-software-and-onboard-to-sensor"></a>2. Implementar hardware, software e incorporar el sensor

- Compre el hardware del sensor e instale el software. Siga los pasos que se describen aquí. Para obtener más información, consulte este artículo, la [guía de hardware de Defender para IoT](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) y la [guía de instalación](https://aka.ms/AzureDefenderforIoTInstallSensorISO).

  - Después de instalar el sensor, guarde de forma segura las credenciales de inicio de sesión del sensor. Necesitará las credenciales para cargar el archivo de activación en el sensor.

  - Si trabaja con sensores administrados de forma local, guarde de forma segura la dirección IP del sensor o el nombre del sensor definido en la instalación. Puede que desee usarlo al crear un nombre de sensor durante el registro del sensor en el portal de Defender para IoT. Puede usarlo más adelante para facilitar el seguimiento y usar una nomenclatura coherente para el nombre de registro en el portal de Azure Defender para IoT y la dirección IP del sensor implementado que se muestra en la consola del sensor.

- Registre el sensor con el portal de Defender para IoT y descargue un archivo de activación del sensor.

- Cargue el archivo de activación en el sensor.

## <a name="3-perform-network-setup-for-sensor-monitoring-and-management"></a>3. Configurar la red para la supervisión y administración del sensor

- Conecte el sensor a la red. Este paso se describe en la [guía de configuración de red](https://aka.ms/AzureDefenderForIoTNetworkSetup).

## <a name="4-start-discovering-your-network"></a>4. Iniciar la detección de la red

- Ajuste la configuración del sistema en la consola del sensor.

- Conecte los sensores a una consola de administración local.

Para obtener más información, consulte el [manual del usuario del sensor de Azure Defender para IoT](https://aka.ms/AzureDefenderforIoTUserGuide) y el [manual del usuario de la consola de administración de Defender para IoT](https://aka.ms/DefenderForIoTManagementConsole).

## <a name="5-populate-azure-sentinel-with-alert-information"></a>5. Rellenar Azure Sentinel con la información de alertas

- Para enviar información de alertas a Azure Sentinel, configure la solución: [Conexión de los datos de Defender para IoT con Azure Sentinel (versión preliminar)](how-to-configure-with-sentinel.md)
 

## <a name="next-steps"></a>Pasos siguientes

- Habilitar [Defender for IoT](quickstart-onboard-iot-hub.md)
- Configurar la [solución](quickstart-configure-your-solution.md)
- [Creación de módulos de seguridad](quickstart-create-security-twin.md)
- Configurar [alertas personalizadas](quickstart-create-custom-alerts.md)
- [Implementación de un agente de seguridad](how-to-deploy-agent.md)