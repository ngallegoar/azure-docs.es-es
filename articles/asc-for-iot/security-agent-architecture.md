---
title: Información acerca de la arquitectura del agente de seguridad de Azure Security Center para IoT | Microsoft Docs
description: Conozca la arquitectura de agente de seguridad de los agentes que se usan en el servicio Azure Security Center for IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 998aeab197931a75579fc39b28e3a248b85fc57b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68596914"
---
# <a name="security-agent-reference-architecture"></a>Arquitectura de referencia del agente de seguridad

Azure Security Center para IoT proporciona una arquitectura de referencia a agentes de seguridad que registran, procesan, agregan y envían datos de seguridad mediante IoT Hub.

Los agentes de seguridad están diseñados para funcionar en un entorno de IoT restringido, y son tremendamente personalizables en cuanto a todo el valor que aportan en comparación con los recursos que consumen.

Los agentes de seguridad permiten realizar lo siguiente:

- Recopilar eventos de seguridad sin procesar del sistema operativo subyacente (Linux o Windows). Para más información sobre los recopiladores de datos de seguridad disponibles consulte [Configuración del agente de Azure Security Center para IoT](how-to-agent-configuration.md).

- Agregar eventos de seguridad sin procesar a los mensajes enviados mediante IoT Hub.

- Autenticarse con la identidad del dispositivo existente, o con una identidad de módulo dedicada. Vea [Métodos de autenticación del agente de seguridad](concept-security-agent-authentication-methods.md) para obtener más información.

- Establecer configuraciones de forma remota mediante el módulo gemelo **azureiotsecurity**. Para obtener más información, consulte [Configuración del agente de Azure Security Center para IoT](how-to-agent-configuration.md).

Los agentes de seguridad de Azure Security Center para IoT se desarrollan como proyectos de código abierto y están disponibles en GitHub: 

- [Agente de Azure Security Center para IoT basado en C](https://github.com/Azure/Azure-IoT-Security-Agent-C) 
- [Agente de Azure Security Center para IoT basado en C#](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Plataformas compatibles con el agente

Azure Security Center para IoT ofrece diferentes instaladores de agente de 32 y 64 bits de Windows, y lo mismo para 32 bits y 64 bits de Linux. Asegúrese de que tiene el instalador de agente correcto para cada uno de los dispositivos, según la siguiente tabla:

| Architecture | Linux | Windows |    Detalles|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32 bits  | C  | C#  ||
| 64 bits  | C# o C           | C#      | Se recomienda usar el agente de C para dispositivos con recursos de dispositivo mínimos o más restringidos.|
|

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la arquitectura de agentes de seguridad de Azure Security Center para IoT y sobre los instaladores disponibles.

Para continuar con la introducción a la implementación de Azure Security Center para IoT, lea los siguientes artículos:

- Información acerca de los [métodos de autenticación del agente de seguridad](concept-security-agent-authentication-methods.md)
- Seleccione e implemente un [agente de seguridad](how-to-deploy-agent.md)
- Revise los [requisitos previos del servicio](service-prerequisites.md) Azure Security Center para IoT
- Aprenda a [habilitar el servicio Azure Security Center para IoT en su instancia de IoT Hub](quickstart-onboard-iot-hub.md)
- Más información acerca del servicio en las [preguntas más frecuentes de Azure Security Center para IoT](resources-frequently-asked-questions.md)
