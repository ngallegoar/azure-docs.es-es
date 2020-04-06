---
title: 'Continuidad empresarial: Azure Database for MariaDB'
description: Obtenga información sobre la continuidad empresarial (restauración a un momento dado, interrupción del centro de datos, restauración geográfica) al usar el servicio Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: c01e0df1f420c8489ca3445d9fa025b251a870f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532398"
---
# <a name="understand-business-continuity-in-azure-database-for-mariadb"></a>Información acerca de la continuidad empresarial en Azure Database for MariaDB

En este artículo se describen las funcionalidades de continuidad empresarial y recuperación ante desastres que proporciona Azure Database for MariaDB. Conozca más información sobre opciones para recuperarse de eventos de interrupción que podrían provocar la pérdida de información o que su base de datos y aplicación dejen de estar disponibles. Sepa qué hacer en caso de que un error del usuario o la aplicación afecte a la integridad de los datos, se produzca una interrupción en una región de Azure o su aplicación requiera mantenimiento.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>Características que puede utilizar para proporcionar continuidad empresarial

Azure Database for MariaDB proporciona características de continuidad empresarial entre las que se incluyen copias de seguridad automatizadas y la capacidad de los usuarios de iniciar la restauración geográfica. Cada una de ellas posee distintas características que abarcan los conceptos de tiempo de recuperación calculado (ERT) y pérdida de datos potencial. Cuando entienda estas opciones, puede elegir las que más relevantes considere y utilizarlas juntas en distintos escenarios. A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. A esto se le denomina "objetivo de tiempo de recuperación" (RTO). También debe conocer la cantidad máxima de actualizaciones de datos recientes (intervalo de tiempo) que la aplicación puede tolerar perder al recuperarse después de un evento de interrupción. A esto se le conoce como "objetivo de punto de recuperación" (RPO).

La tabla siguiente compara los valores de ERT y RPO para las características disponibles:

| **Funcionalidad** | **Basic** | **Uso general** | **Memoria optimizada** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Restauración a un momento dado a partir de una copia de seguridad | Cualquier punto de restauración dentro del período de retención | Cualquier punto de restauración dentro del período de retención | Cualquier punto de restauración dentro del período de retención |
| Restauración geográfica de las copias de seguridad con replicación geográfica | No compatible | ERT < 12 horas<br/>RPO < 1 hora | ERT < 12 horas<br/>RPO < 1 hora |

> [!IMPORTANT]
> Si elimina el servidor, todas las bases de datos contenidas en el servidor también se eliminan y no se pueden recuperar. No puede restaurar un servidor eliminado.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Recuperación de un servidor tras un error del usuario o la aplicación

Puede usar las copias de seguridad del servicio para recuperar un servidor a partir de diversos eventos de interrupción. Un usuario podría eliminar de forma involuntaria algunos datos, una tabla importante o, incluso, una base de datos entera. O bien, una aplicación podría sobrescribir accidentalmente datos correctos por otros no válidos debido a un defecto en esta, etc.

Puede realizar una restauración a un momento dado para crear una copia del servidor en un momento dado conocido correcto. Este momento dado debe estar incluido en el período de retención de la copia de seguridad que configuró para el servidor. Después de restaurar los datos en el nuevo servidor, puede reemplazar el servidor original por el servidor recientemente restaurado o copiar los datos necesarios del servidor restaurado en el servidor original.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Recuperación de una interrupción del centro de datos de una región de Azure

Aunque es poco habitual, en los centros de datos de Azure pueden producirse interrupciones. Cuando esto ocurre, provoca también una interrupción en el negocio que podría extenderse solo unos pocos minutos o, incluso, horas.

Una opción consiste en esperar a que el servidor vuelva a estar en línea cuando termine la interrupción del centro de datos. Esta opción es válida para aquellas aplicaciones que se pueden permitir tener el servidor sin conexión durante un período de tiempo, por ejemplo las aplicaciones de un entorno de producción. Cuando se produce una interrupción en un centro de datos, no sabe cuánto durará, por lo que esta opción solo es útil si no necesita utilizar el servidor durante un tiempo.

La otra opción es usar la característica de replicación geográfica de Azure Database for MariaDB que permite restaurar el servidor mediante copias de seguridad con redundancia geográfica. Se puede acceder a estas copias de seguridad incluso cuando la región en la que se hospeda el servidor está sin conexión. Puede realizar la restauración a partir de estas copias de seguridad en cualquier otra región y volver a poner en línea el servidor.

> [!IMPORTANT]
> La restauración geográfica solo es posible si se ha aprovisionado el servidor con almacenamiento de copia de seguridad con redundancia geográfica.

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de las copias de seguridad automatizadas, consulte [Copia de seguridad en Azure Database for MariaDB](concepts-backup.md).
- Para restaurar a un momento dado mediante Azure Portal, consulte cómo  [restaurar una base de datos a un momento dado con Azure Portal](howto-restore-server-portal.md).

<!--
- To restore to a point in time using Azure CLI, see [restore database to a point in time using CLI](howto-restore-server-cli.md). 
-->
