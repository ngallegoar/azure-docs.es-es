---
title: Componentes y requisitos previos
description: Detalles de todo lo necesario para empezar a conocer los requisitos previos del servicio Azure Defender para IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2020
ms.author: rkarlin
ms.custom: references_regions
ms.openlocfilehash: b0913dc48f807c26396a38e31d293877b4561b7d
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92089186"
---
# <a name="azure-defender-for-iot-prerequisites"></a>Requisitos previos de Azure Defender para IoT

En este artículo se proporciona una explicación sobre los diferentes componentes del servicio Defender para IoT, lo que se necesita para comenzar, y explica igualmente los conceptos básicos para ayudar a conocer el servicio.

## <a name="minimum-requirements"></a>Requisitos mínimos

- Supervisión sin agente para dispositivos IoT y OT (basado en la tecnología CyberX)
    - Conmutadores de red compatibles con la supervisión del tráfico a través del puerto SPAN
    - Dispositivos de hardware para el sensor NTA. Para obtener más información, consulte el documento sobre[hardware certificado](https://aka.ms/AzureDefenderforIoTBareMetalAppliance)
    - Rol de **Colaborador** en una suscripción a Azure (solo se requiere durante la incorporación para definir los dispositivos confirmados)
    - Rol de **Colaborador** (nivel Gratis o Estándar) en IoT Hub (para la administración conectada en la nube)
- Seguridad para dispositivos IoT administrados que se administran mediante Azure IoT Hub
    - Rol de **Colaborador** (nivel Estándar) de IoT Hub
    - IoT Hub: Se debe habilitar la activación/desactivación de funcionalidad de **Azure Defender para IoT**
    - Para lograr compatibilidad con el módulo de seguridad de nivel de dispositivo  
        - Los agentes de Defender para IoT admiten una lista cada vez mayor de dispositivos y plataformas, consulte la [lista de plataformas compatibles](how-to-deploy-agent.md).


## <a name="supported-service-regions"></a>Regiones de servicio admitidas

Para más información, consulte [Regiones compatibles con IoT Hub](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub). 

Defender para IoT enruta todo el tráfico desde todas las regiones europeas hasta el centro de datos de Oeste de Europa, y desde todas las regiones restantes hasta el centro de datos regional de Centro de EE. UU.

## <a name="next-steps"></a>Pasos siguientes

- Lea la [introducción](overview.md) a la seguridad de Azure IoT
- Aprenda a [habilitar el servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes de Defender para IoT](resources-frequently-asked-questions.md).
- Explore la [información sobre las alertas de Defender para IoT](concept-security-alerts.md).
