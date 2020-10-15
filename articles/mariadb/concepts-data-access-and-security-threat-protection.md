---
title: 'Advanced Threat Protection: Azure Database for MariaDB'
description: Advanced Threat Protection detecta actividades anómalas en la base de datos que indican posibles amenazas de seguridad.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 8485907eba75f80c8f0ed4fd0cc7368c6147b9fd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "79532185"
---
# <a name="azure-database-for-mariadb-advanced-threat-protection"></a>Advanced Threat Protection en Azure Database for MariaDB

Advanced Threat Protection para Azure Database for MariaDB detecta actividades anómalas que indican intentos inusuales y potencialmente dañinos de acceso o ataque a las bases de datos.

> [!IMPORTANT]
> Advanced Threat Protection está en versión preliminar pública.

Advanced Threat Protection forma parte de la oferta Seguridad de datos avanzada, que es un paquete unificado para capacidades avanzadas de seguridad de SQL. Se puede acceder a Advanced Threat Protection y administrarlo por medio de [Azure Portal](https://portal.azure.com). La característica está disponible para los servidores de uso general y para los optimizados para memoria.

> [!NOTE]
> La característica Advanced Threat Protection **no** está disponible en las siguientes regiones de nube soberana y Azure Government: US Gov Texas, US Gov Arizona, US Gov Iowa, US Gov Virginia, US DoD (este), US DoD (centro), Centro de Alemania, Norte de Alemania, Este de China y Este de China 2. Visite [productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/) para obtener información de disponibilidad general del producto.


## <a name="what-is-advanced-threat-protection"></a>¿Qué es Advanced Threat Protection?

Advanced Threat Protection para Azure Database for MariaDB proporciona un nivel adicional de seguridad, ya que emite alertas de seguridad sobre actividades anómalas, lo que permite a los clientes detectar posibles amenazas y responder a ellas cuando se producen. Los usuarios reciben una alerta sobre actividades sospechosas en las bases de datos, posibles puntos vulnerables, así como sobre patrones anómalos de consulta y acceso a las bases de datos. Advanced Threat Protection para Azure Database for MariaDB integra las alertas con [Azure Security Center](https://azure.microsoft.com/services/security-center/), que contiene detalles de las actividades sospechosas y recomienda acciones sobre cómo investigar y mitigar la amenaza. Con Advanced Threat Protection para Azure Database for MariaDB, no necesita ser un experto en seguridad ni tendrá que administrar sistemas de supervisión de seguridad avanzados para solucionar posibles amenazas de la base de datos. 

![Concepto de Advanced Threat Protection](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Alertas de Advanced Threat Protection 
Advanced Threat Protection para Azure Database for MariaDB detecta actividades anómalas, que pueden ser indicativas de intentos inusuales y potencialmente peligrosos de acceder a las bases de datos o de vulnerar su seguridad, y pueden desencadenar las siguientes alertas:
- **Acceso desde una ubicación inusual**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso al servidor de Azure Database for MariaDB, donde alguien ha iniciado sesión en el servidor de Azure Database for MariaDB desde una ubicación geográfica inusual. En algunos casos, la alerta detecta una acción legítima (una nueva aplicación o el mantenimiento de un desarrollador). En otros casos, la alerta detecta una acción malintencionada (por ejemplo, un antiguo empleado o un atacante externo).
- **Acceso desde un centro de datos de Azure inusual**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso al servidor de Azure Database for MariaDB, donde alguien ha iniciado sesión en el servidor desde un centro de datos de Azure inusual que se vio en este servidor recientemente. En algunos casos, la alerta detecta una acción legítima (una aplicación nueva en Azure, Power BI). En otros casos, la alerta detecta una acción malintencionada procedente de un recurso o servicio de Azure (por ejemplo, un antiguo empleado o un atacante externo).
- **Acceso desde una entidad de seguridad desconocida**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso al servidor de Azure Database for MariaDB, donde alguien ha iniciado sesión en el servidor utilizando una entidad de seguridad inusual (un usuario de Azure Database for MariaDB). En algunos casos, la alerta detecta una acción legítima (una nueva aplicación o el mantenimiento de un desarrollador). En otros casos, la alerta detecta una acción malintencionada (por ejemplo, un antiguo empleado o un atacante externo).
- **Acceso desde una aplicación potencialmente dañina**: esta alerta se desencadena cuando una aplicación potencialmente dañina se utiliza para tener acceso a la base de datos. En algunos casos, la alerta detecta la realización de pruebas de seguridad. En otros casos, la alerta detecta un ataque que se realiza con herramientas de ataque comunes.
- **Ataques por fuerza bruta a las credenciales de Azure Database for MariaDB**: esta alerta se desencadena cuando hay un número anormalmente elevado de inicios de sesión infructuosos con distintas credenciales. En algunos casos, la alerta detecta la realización de pruebas de seguridad. En otros casos, la alerta detecta ataques por fuerza bruta.

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Para obtener más información sobre los precios, vea la [página Precios de Azure Database for MariaDB](https://azure.microsoft.com/pricing/details/mariadb/) 
* Configuración de [Advanced Threat Protection para Azure Database for MariaDB](howto-database-threat-protection-portal.md) en Azure Portal  
