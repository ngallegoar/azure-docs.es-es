---
title: 'Disponibilidad de la infraestructura de Azure: seguridad de Azure'
description: En este artículo se proporciona información sobre las acciones de Microsoft para proteger la infraestructura de Azure y proporcionar la máxima disponibilidad para los datos de clientes.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2019
ms.author: terrylan
ms.openlocfilehash: c50c4faf47caf0a7519d61fdc8989ec9fd809d78
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68727227"
---
# <a name="azure-infrastructure-availability"></a>Disponibilidad de la infraestructura de Azure
En este artículo se proporciona información sobre las acciones de Microsoft para proteger la infraestructura de Azure y proporcionar la máxima disponibilidad para los datos de clientes. Azure proporciona una disponibilidad sólida basada en la amplia redundancia lograda con la tecnología de virtualización.

## <a name="temporary-outages-and-natural-disaster"></a>Interrupciones temporales y desastres naturales
El equipo de infraestructura y operaciones de Microsoft Cloud diseña, compila, opera y mejora la infraestructura en la nube. Este equipo garantiza que la infraestructura de Azure ofrece alta disponibilidad y confiabilidad, elevada eficiencia y escalabilidad inteligente. El equipo proporciona una nube más segura, privada y de confianza.

Los sistemas de alimentación ininterrumpida y los grandes bancos de baterías garantizan que la electricidad siga siendo continua si se produce un corte de energía a corto plazo. Los generadores de emergencia suministran energía de reserva para apagones prolongados y mantenimiento planeado. Si se produce un desastre natural, el centro de datos puede usar las reservas de combustible in situ.

Las redes de fibra óptica sólidas y de alta velocidad conectan los centros de datos con otros centros importantes y usuarios de Internet. Calcule las cargas de trabajo de host de los nodos más cerca de los usuarios para reducir la latencia, proporcionar redundancia geográfica y aumentar la resistencia general del servicio. Un equipo de ingenieros trabaja las 24 horas del día para garantizar que los servicios estén siempre disponibles.

Microsoft garantiza una alta disponibilidad mediante la supervisión avanzada y la respuesta a incidentes, el soporte técnico de servicios y la funcionalidad de conmutación por error de copia de seguridad. Los centros de operaciones de Microsoft distribuidos geográficamente operan ininterrumpidamente todos los días del año. La red de Azure es una de las más grandes del mundo. La red de distribución de fibra óptica y contenido conecta los centros de datos y los nodos perimetrales para garantizar un alto rendimiento y una gran confiabilidad.

## <a name="disaster-recovery"></a>Recuperación ante desastres
Azure mantiene los datos durables en dos ubicaciones. Puede elegir la ubicación del sitio de copia de seguridad. En ambas ubicaciones, Azure mantiene constantemente tres réplicas en buen estado de los datos.

## <a name="database-availability"></a>Disponibilidad de la base de datos
Azure garantiza que una base de datos sea accesible desde Internet mediante una puerta de enlace de Internet con disponibilidad sostenida de la base de datos. La supervisión evalúa el mantenimiento y el estado de las bases de datos activas en intervalos de tiempo de cinco minutos.

## <a name="storage-availability"></a>Disponibilidad de almacenamiento
Azure ofrece almacenamiento mediante un servicio de almacenamiento muy durable y escalable que proporciona puntos de conexión de conectividad. Esto significa que una aplicación puede acceder directamente al servicio de almacenamiento. El servicio de almacenamiento procesa las solicitudes de almacenamiento entrantes de forma eficiente, con integridad transaccional.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que Microsoft hace para ayudar a proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](physical-security.md)
- [Componentes y límites del sistema de información de Azure](infrastructure-components.md)
- [Arquitectura de red de Azure](infrastructure-network.md)
- [Red de producción de Azure](production-network.md)
- [Características de seguridad de Azure SQL Database](infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](infrastructure-integrity.md)
- [Protección de datos de cliente de Azure](protection-customer-data.md)
