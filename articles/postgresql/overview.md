---
title: Introducción al servicio de base de datos relacional Azure Database for PostgreSQL
description: Proporciona información general sobre el servicio de base de datos relacional de Azure Database for MySQL.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 11/25/2019
ms.openlocfilehash: deb720638e0921762eb9c5800d218c3cfccb7f55
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213608"
---
# <a name="what-is-azure-database-for-postgresql"></a>¿Qué es Azure Database for PostgreSQL?
Azure Database for PostgreSQL es un servicio de base de datos relacional en la nube de Microsoft diseñado para desarrolladores. Se basa en la versión de comunidad del motor de base de datos [PostgreSQL](https://www.postgresql.org/) de código abierto y está disponible en dos opciones de implementación: Un solo servidor e Hiperescala (Citus)

## <a name="azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL: Un solo servidor
La opción de implementación Un solo servidor ofrece:

- [Alta disponibilidad](concepts-high-availability.md) integrada sin coste adicional (99,99 % en contrato de nivel de servicio)
- Rendimiento predecible, con precios de pago por uso inclusivos.
- [Escalado inmediato, cuando sea necesario](concepts-pricing-tiers.md), en cuestión de segundos
- [Supervisión y alertas](concepts-monitoring.md) para evaluar el servidor
- Seguridad y cumplimiento de nivel empresarial.
- [Protección](concepts-security.md) de información confidencial en reposo y en movimiento.
- [Copias de seguridad automáticas y restauración a un momento dado](concepts-business-continuity.md) durante un máximo de 35 días


Todas estas funcionalidades apenas requieren tareas de administración y todas se proporcionan sin costo adicional. Le permiten centrarse en el desarrollo rápido de aplicaciones y en reducir el plazo de acceso al mercado, en lugar de tener que dedicar tiempo y recursos a la administración tanto de las máquinas virtuales como de la infraestructura. Puede seguir desarrollando la aplicación con las herramientas de código abierto y en la plataforma de que prefiera sin necesidad de adquirir nuevos conocimientos.

La opción de implementación Un solo servidor ofrece tres planes de tarifa: Básico, De uso general y Optimizado para memoria. Cada plan ofrece capacidades de recursos diferente para admitir sus cargas de trabajo de la base de datos. Puede compilar su primera aplicación en una base de datos pequeña por poco dinero al mes y, después, ajustar la escala para satisfacer las necesidades de la solución. La escalabilidad dinámica permite a la base de datos responder de manera transparente a los cambiantes requisitos de recursos. Solo paga por los recursos que necesite y cuando los necesite. Para obtener más información, consulte los  [planes de tarifa](concepts-pricing-tiers.md).

## <a name="azure-database-for-postgresql---hyperscale-citus"></a>Hiperescala (Citus) de Azure Database for PostgreSQL
La opción Citus escala horizontalmente las consultas entre varias máquinas mediante particionamiento. Su motor de consultas paraleliza las consultas SQL entrantes en estos servidores para agilizar las respuestas en conjuntos de datos grandes. Sirve aplicaciones que requieren mayor escala y mejor rendimiento, por lo general cargas de trabajo que se aproximan a los 100 GB de datos (o que ya los superan).

La opción de implementación Citus ofrece:

- Escalado horizontal entre varias máquinas mediante particionamiento
- Paralelización de consultas entre estos servidores, lo que agiliza las respuestas en conjuntos de datos grandes
- Una excelente compatibilidad con aplicaciones de varios inquilinos, análisis operativos en tiempo real y cargas de trabajo transaccionales de alto rendimiento

Las aplicaciones compiladas para PostgreSQL puede ejecutar consultas distribuidas en Citus con las [bibliotecas de conexiones](./concepts-connection-libraries.md) estándar y unos cambios mínimos.

## <a name="contacts"></a>Contactos
Para cualquier pregunta o sugerencia acerca del uso de Azure Database for PostgreSQL, envíe un correo electrónico al equipo de Azure Database for PostgreSQL ([@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Esta dirección es para preguntas generales, no para incidencias de soporte técnico.

Además, tenga en cuenta estos puntos de contacto según corresponda:
- Para ponerse en contacto con el servicio de soporte técnico de Azure o solucionar cualquier problema de su cuenta, [presente una incidencia desde Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Para proporcionar comentarios o solicitar nuevas características, cree una entrada mediante [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

## <a name="next-steps"></a>Pasos siguientes
- Consulte la [página de precios](https://azure.microsoft.com/pricing/details/postgresql/) para ver comparaciones de costos y calculadoras. Las opciones de implementación de un solo servidor y de Hiperescala (Citus) ofrecen también descuentos de instancia reservada en prepago. Consulte las páginas sobre los [precios de instancia reservada de un solo servidor](concept-reserved-pricing.md) y los [precios de instancia reservada de Hiperescala](concepts-hyperscale-reserved-pricing.md) para más información.
- Comience por crear su primera base de datos de [un solo servidor](./quickstart-create-server-database-portal.md) o [Hiperescala (Citus)](./quickstart-create-hyperscale-portal.md) de Azure Database for PostgreSQL.
- Compile la primera aplicación en Python, PHP, Ruby, C\#, Java, Node.js: [Bibliotecas de conexiones](./concepts-connection-libraries.md)
