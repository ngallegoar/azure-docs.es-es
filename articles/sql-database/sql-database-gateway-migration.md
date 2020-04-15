---
title: Aviso de migración de tráfico de puertas de enlace
description: El artículo proporciona un aviso a los usuarios sobre la migración de direcciones IP de puertas de enlace de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: 9e3c33bb7493f07d9fbf19710f21d0114e7abec8
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757067"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Migración de tráfico de Azure SQL Database a puertas de enlace más recientes

A media que la infraestructura de Azure mejora, Microsoft actualizará periódicamente el hardware para asegurarse de que ofrecemos la mejor experiencia de cliente posible. En los próximos meses, tenemos previsto agregar puertas de enlace basadas en generaciones de hardware más recientes, migrar el tráfico a ellas y retirar puertas de enlace basadas en hardware más antiguo en algunas regiones.  

Se enviará un aviso a los clientes por correo electrónico y en Azure Portal con antelación de cualquier cambios en las puertas de enlace disponibles en cada región. La información más actualizada se mantendrá en la tabla [Direcciones IP de la puerta de enlace de Azure SQL Database](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses).

## <a name="impact-of-this-change"></a>Impacto de este cambio

La primera ronda de migración del tráfico a las puertas de enlace más recientes está programada para el **14 de octubre de 2019** en las siguientes regiones:
- Sur de Brasil
- Oeste de EE. UU.
- Oeste de Europa
- Este de EE. UU.
- Centro de EE. UU.
- Sudeste de Asia
- Centro-sur de EE. UU.
- Norte de Europa
- Centro-Norte de EE. UU
- Japón Occidental
- Japón Oriental
- Este de EE. UU. 2
- Este de Asia

La migración del tráfico cambiará la dirección IP pública que DNS resuelve para su instancia de SQL Database.
Se verá afectado en los siguientes casos:
- Ha codificado de forma rígida la dirección IP de una puerta de enlace determinada en el firewall local.
- Tiene subredes que usan Microsoft.SQL como punto de conexión de servicio, pero no se puede comunicar con las direcciones IP de puerta de enlace.

No se verá afectado en los siguientes casos:
- La directiva de conexión tiene redireccionamiento.
- Tiene conexiones a SQL Database desde Azure y con etiquetas de servicio.
- Las conexiones realizadas mediante versiones compatibles de JDBC Driver para SQL Server no percibirán ningún impacto. Para las versiones compatibles de JDBC, consulte [Descarga de Microsoft JDBC Driver para SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Qué puede hacer si se ve afectado

Es recomendable que permita el tráfico saliente a direcciones IP para todas las [direcciones IP de puerta de enlace de Azure SQL Database](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) de la región en el puerto TCP 1433 y el intervalo de puertos de 11000 a 11999. Esta recomendación se aplica a los clientes que se conectan desde el entorno local y también los que se conectan a través de puntos de conexión de servicio. Para obtener más información sobre los intervalos de puertos, consulte [Directiva de conexión](sql-database-connectivity-architecture.md#connection-policy).

Es posible que las conexiones realizadas desde aplicaciones que usan versiones de Microsoft JDBC Driver anteriores a 4.0 no superen la validación de certificados. Las versiones inferiores de Microsoft JDBC dependen del nombre común (CN) en el campo Asunto del certificado. La mitigación consiste en asegurarse de que la propiedad hostNameInCertificate esté establecida en *.database.windows.net. Para más información sobre cómo establecer la propiedad hostNameInCertificate, consulte [Conexión con el cifrado](/sql/connect/jdbc/connecting-with-ssl-encryption).

Si la mitigación anterior no funciona, envíe una solicitud de soporte técnico para SQL Database con la dirección URL siguiente: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Pasos siguientes

- Obtener más información sobre la [Arquitectura de conectividad de Azure SQL](sql-database-connectivity-architecture.md)
