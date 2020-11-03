---
title: Próximos cambios importantes en Azure Security Center
description: Próximos cambios en Azure Security Center que debe tener en cuenta y para los que puede que necesite un plan
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2020
ms.author: memildin
ms.openlocfilehash: ba0c68589f7ea4b32970e3ad81fea5175b998dc7
ms.sourcegitcommit: 3e8058f0c075f8ce34a6da8db92ae006cc64151a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92629160"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Próximos cambios importantes en Azure Security Center

> [!IMPORTANT]
> La información de esta página está relacionada con productos o características en versión preliminar que pueden modificarse sustancialmente antes de lanzarse comercialmente, si es que lo hacen finalmente. Microsoft no ofrece ningún compromiso o garantía, ni expresa ni implícita, con respecto a la información que se proporciona aquí.

En esta página, obtendrá información sobre los cambios planeados para Security Center. Describe las modificaciones planeadas en el producto que pueden afectar a aspectos como los flujos de trabajo o la puntuación segura.

Si busca las notas de la versión más recientes, puede encontrarlas en [Novedades de Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Cambios planeados

### <a name="recommendations-related-to-azure-security-benchmark-to-be-added-preview"></a>Recomendaciones relacionadas con Azure Security Benchmark que se van a agregar (versión preliminar)

| Aspecto | Detalles |
|---------|---------|
|Fecha del anuncio | 26 de octubre de 2020  |
|Fecha de este cambio  |  Noviembre de 2020 |
|Impacto     | Posiblemente, más recomendaciones para revisar.<br>No tiene impacto inmediato en la puntuación segura (las recomendaciones de la versión preliminar no afectan a la puntuación segura).<br>No tiene impacto inmediato en el estado de mantenimiento de los recursos (las recomendaciones de la versión preliminar no representan un recurso "Incorrecto").|
|  |  |

Azure Security Benchmark es el conjunto de directrices específico de Azure creado por Microsoft para ofrecer los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. [Mas información sobre Azure Security Benchmark](../security/benchmarks/introduction.md).

Las 29 recomendaciones siguientes se agregarán a Security Center para aumentar la cobertura de la prueba comparativa.

Las recomendaciones de la versión preliminar no representan un recurso incorrecto y no se incluyen en los cálculos de una puntuación segura. Corríjalas siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación. Puede encontrar más información sobre cómo responder a estas recomendaciones en [Recomendaciones de corrección en Azure Security Center](security-center-remediate-recommendations.md).

- Azure Backup debería habilitarse en las máquinas virtuales
- La retención de la auditoría en los servidores de SQL Server debe establecerse en 90 días, como mínimo
- Los registros de diagnóstico deben habilitarse en App Service 
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL
- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos PostgreSQL
- Es necesario exigir FTPS en la aplicación de API.
- Es necesario exigir FTPS en la aplicación de funciones.
- Es necesario exigir FTPS en la aplicación web.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MariaDB.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MySQL.
- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for PostgreSQL.
- Java debe actualizarse a la versión más reciente en la aplicación de API
- Java debe actualizarse a la versión más reciente en la aplicación de funciones
- Java debe actualizarse a la versión más reciente en la aplicación web
- Se debe usar una identidad administrada en la aplicación de API
- Se debe usar una identidad administrada en la aplicación de funciones
- Se debe usar una identidad administrada en la aplicación web
- PHP debe actualizarse a la versión más reciente en la aplicación de API
- PHP debe actualizarse a la versión más reciente en la aplicación web
- El punto de conexión privado debe estar habilitado para servidores MariaDB
- El punto de conexión privado debe estar habilitado para servidores MySQL
- El punto de conexión privado debe estar habilitado para servidores PostgreSQL
- Python debe actualizarse a la versión más reciente en la aplicación de API
- Python debe actualizarse a la versión más reciente en la aplicación de funciones
- Python debe actualizarse a la versión más reciente en la aplicación web
- TLS debe actualizarse a la versión más reciente en la aplicación de API
- TLS debe actualizarse a la versión más reciente en la aplicación de funciones
- TLS debe actualizarse a la versión más reciente en la aplicación web
- Las aplicaciones web deben solicitar un certificado SSL a todas las solicitudes entrantes

Vínculos relacionados:

- [Más información sobre Azure Security Benchmark](../security/benchmarks/introduction.md)
- [Más información sobre las aplicaciones de API de Azure](../app-service/app-service-web-tutorial-rest-api.md)
- [Más información sobre las aplicaciones de funciones de Azure](../azure-functions/functions-overview.md)
- [Más información sobre las aplicaciones web de Azure](../app-service/overview.md)
- [Más información sobre Azure Database for MariaDB](../mariadb/overview.md)
- [Más información sobre Azure Database for MySQL](../mysql/overview.md)
- [Más información sobre Azure Database for PostgreSQL](../postgresql/overview.md)

## <a name="next-steps"></a>Pasos siguientes

Para ver todos los cambios recientes realizados en el producto, consulte [Novedades de Azure Security Center](release-notes.md).