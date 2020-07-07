---
title: 'Límites en Azure Database for PostgreSQL: servidor único'
description: En este artículo, se describen los límites de Azure Database for PostgreSQL con un único servidor, como el número de opciones del motor de almacenamiento y de conexión.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/28/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 047e722a0e0ade60d1eb93a48e37333fffafd674
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "76836463"
---
# <a name="limits-in-azure-database-for-postgresql---single-server"></a>Límites de Azure Database for PostgreSQL con un único servidor
En las secciones siguientes se describen los límites de capacidad y funcionales en el servicio de base de datos. Para más información sobre los niveles de recursos (proceso, memoria, almacenamiento), consulte el artículo acerca de los [planes de tarifa](concepts-pricing-tiers.md).


## <a name="maximum-connections"></a>Número máximo de conexiones
A continuación se muestran el número máximo de conexiones por plan de tarifa y los núcleos virtuales. El sistema de Azure requiere cinco conexiones para supervisar el servidor de Azure Database for PostgreSQL. 

|**Plan de tarifa**| **Núcleos virtuales**| **Conexiones máximas** | **Número máximo de conexiones de usuario** |
|---|---|---|---|
|Básico| 1| 55 | 50|
|Básico| 2| 105 | 100|
|De uso general| 2| 150| 145|
|De uso general| 4| 250| 245|
|De uso general| 8| 480| 475|
|De uso general| 16| 950| 945|
|De uso general| 32| 1\.500| 1495|
|De uso general| 64| 1900| 1895|
|Memoria optimizada| 2| 300| 295|
|Memoria optimizada| 4| 500| 495|
|Memoria optimizada| 8| 960| 955|
|Memoria optimizada| 16| 1900| 1895|
|Memoria optimizada| 32| 1987| 1982|

Si las conexiones superan el límite, puede que reciba el error siguiente:
> FATAL:  sorry, too many clients already

> [!IMPORTANT]
> Para obtener la mejor experiencia posible, se recomienda usar un agrupador de conexiones, como pgBouncer, para administrar las conexiones de forma eficaz.

Una conexión de PostgreSQL, aunque no esté activa, puede ocupar aproximadamente 10 MB de memoria. Además, la creación de conexiones lleva su tiempo. La mayoría de las aplicaciones solicitan muchas conexiones de corta duración, y esto es lo que conforma esta situación. El resultado es que hay menos recursos disponibles para la carga de trabajo real, lo que baja el rendimiento. Esto se puede evitar con un agrupador de conexiones, ya que reduce las conexiones inactivas y reutiliza las conexiones existentes. Para más información, visite nuestra [entrada de blog](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/not-all-postgres-connection-pooling-is-equal/ba-p/825717).

## <a name="functional-limitations"></a>Limitaciones funcionales
### <a name="scale-operations"></a>Operaciones de escalado
- El escalado dinámico hacia y desde planes de tarifa Básico no se admite en este momento.
- La reducción del tamaño de almacenamiento del servidor no se admite actualmente.

### <a name="server-version-upgrades"></a>Actualizaciones de la versión de servidor
- La migración automatizada entre versiones principales del motor de base de datos no se admite en este momento. Si quiere actualizar a la siguiente versión principal, realice un [volcado y restáurelo](./howto-migrate-using-dump-and-restore.md) a un servidor que se haya creado con la nueva versión del motor.

> Tenga en cuenta que antes de la versión 10 de PostgreSQL, la [directiva de versiones de PostgreSQL](https://www.postgresql.org/support/versioning/) consideraba que una actualización de la _versión principal_ suponía un aumento en el primer _o_ segundo número (por ejemplo, de 9.5 a 9.6 se consideró una actualización de la _principal_).
> A partir de la versión 10, solo un cambio en el primer número se considera una actualización de la versión principal (por ejemplo, de 10.0 a 10.1 es una actualización _menor_ de la versión, y 10 a 11 es una actualización de la versión _principal_).

### <a name="vnet-service-endpoints"></a>Puntos de conexión de servicio de red virtual
- La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

### <a name="restoring-a-server"></a>Restauración de un servidor
- Al usar la característica PITR, el nuevo servidor se crea con la misma configuración de plan de tarifa que el servidor en el que se basa.
- El servidor creado durante una restauración no tiene las reglas de firewall que existían en el servidor original. Las reglas de firewall deben configurarse por separado para este nuevo servidor.
- La restauración a un servidor que se ha eliminado no se admite en este momento.

### <a name="utf-8-characters-on-windows"></a>Caracteres UTF-8 en Windows
- En algunos escenarios, los caracteres UTF-8 no se admiten completamente en PostgreSQL de código abierto en Windows, lo que afecta a Azure Database for PostgreSQL. Consulte el subproceso en [Bug #15476 en postgresql-archive](https://www.postgresql-archive.org/BUG-15476-Problem-on-show-trgm-with-4-byte-UTF-8-characters-td6056677.html) para más información.

## <a name="next-steps"></a>Pasos siguientes
- Comprenda lo que [hay disponible en cada plan de tarifa](concepts-pricing-tiers.md).
- Conozca las [versiones de base de datos de PostgreSQL admitidas](concepts-supported-versions.md).
- Revise [cómo hacer una copia de seguridad de un servidor y restaurarlo en Azure Database for PostgreSQL mediante Azure Portal](howto-restore-server-portal.md).
