---
title: Pruebas de simulación de Azure DDoS Protection
description: Aprender a realizar pruebas mediante simulaciones
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: eff738e24b3abce52e80291c55a3ae64c3c8c853
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92905018"
---
# <a name="test-through-simulations"></a>Pruebas mediante simulaciones

El procedimiento recomendado es realizar simulaciones periódicas para probar las suposiciones acerca de cómo responderán los servicios a un ataque. Durante las pruebas, compruebe que los servicios y las aplicaciones continúan funcionando según lo previsto y que no hay ninguna interrupción en la experiencia del usuario. Identifique las carencias desde la perspectiva de la tecnología y de los procesos, e incorpórelas en la estrategia de respuesta a DDoS. Es recomendable realizar dichas pruebas en entornos de ensayo o durante las horas de poca actividad para reducir el impacto en el entorno de producción.

Nos hemos asociado con [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud), un generador de tráfico de autoservicio, para crear una interfaz en la que los clientes de Azure puedan generar tráfico destinado a los puntos de conexión públicos que tengan habilitado el servicio DDoS Protection con fines de simulación. Puede usar la simulación para:

- Comprobar en qué medida Azure DDoS Protection protege sus recursos de Azure frente a ataques de DDoS.
- Optimizar el proceso de respuesta a incidentes durante el ataque de DDoS.
- Documentar el cumplimiento normativo de DDoS.
- Enseñar a los equipos de seguridad de red.

## <a name="prerequisites"></a>Prerrequisitos

- Para poder completar los pasos de este tutorial, primero debe crear un [plan de protección Estándar de Azure DDoS](manage-ddos-protection.md) con direcciones IP públicas protegidas.
- En primer lugar, debe crear una cuenta con [BreakingPoint Cloud](http://breakingpoint.cloud/). 

## <a name="configure-a-ddos-test-attack"></a>Configuración de un ataque de prueba de DDoS

1. Especifique o seleccione los siguientes valores y, a continuación, seleccione **Iniciar prueba** :

    |Configuración        |Value                                              |
    |---------      |---------                                          |
    |Dirección IP de destino           | Escriba una de las direcciones IP públicas que desee probar.                     |
    |Número de puerto   | Escriba _443_.                       |
    |Perfil de DDoS | Seleccione **TCP SYN Flood** (Desbordamiento de TCP SYN).|
    |Tamaño de prueba       | Seleccione **200K pps, 100 Mbps and 8 source Ips** (200 Kpps, 100 Mbps y 8 direcciones IP de origen).                                  |
    |Duración de la prueba | Seleccione **10 minutos**.|

Ahora debería aparecer como se muestra a continuación:

![Ejemplo de simulación de ataque de DDoS: BreakingPoint Cloud](./media/ddos-attack-simulation/ddos-attack-simulation-example-1.png)

## <a name="monitor-and-validate"></a>Supervisión y validación

1. Inicie sesión en https://portal.azure.com y vaya a la suscripción.
1. Seleccione la dirección IP pública en la que probó el ataque.
1. En **Supervisión** , seleccione **Métricas**.
1. Para **Métrica** , seleccione _Bajo ataque DDoS o no_.

Cuando el recurso esté siendo atacado, debería ver que el valor cambia de **0** a **1** , como en la siguiente imagen:

![Ejemplo de simulación de ataque de DDoS: Portal](./media/ddos-attack-simulation/ddos-attack-simulation-example-2.png)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [ver y configurar la telemetría de DDoS Protection](telemetry-monitoring-alerting.md).
- Aprenda a [configurar registros de flujo e informes de mitigación de ataques DDoS](reports-and-flow-logs.md).
- Aprenda a [interactuar con una respuesta rápida de DDoS](ddos-rapid-response.md).