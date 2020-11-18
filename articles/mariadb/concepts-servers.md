---
title: 'Servidores: Azure Database for MariaDB'
description: En este tema se incluyen consideraciones e instrucciones para trabajar con servidores de Azure Database for MariaDB.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 4d8293258083ea3e8d0172f510e5b41e91328736
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94541069"
---
# <a name="server-concepts-in-azure-database-for-mariadb"></a>Conceptos sobre servidores de Azure Database for MariaDB
En este artículo se incluyen consideraciones e instrucciones para trabajar con servidores de Azure Database for MariaDB.

## <a name="what-is-an-azure-database-for-mariadb-server"></a>¿Qué es un servidor de Azure Database for MariaDB?

Un servidor de Azure Database for MariaDB es un punto central de administración de varias bases de datos. Se trata de la misma construcción de servidores MariaDB con la que puede estar familiarizado en entornos locales. En concreto, el servicio Azure Database for MariaDB se administra, ofrece garantías de rendimiento y expone el acceso y las características a nivel de servidor.

Un servidor de Azure Database for MariaDB:

- Se crea dentro de una suscripción de Azure.
- Es el recurso principal de las bases de datos.
- Proporciona un espacio de nombres para las bases de datos.
- Es un contenedor con semántica de duración segura. Si se elimina un servidor, se eliminan las bases de datos que contiene.
- Coloca recursos en una región.
- Proporciona un punto de conexión para el acceso a la base de datos y al servidor.
- Proporciona el ámbito de las directivas de administración que se aplican a sus bases de datos: inicio de sesión, firewall, usuarios, roles, configuración, etc.
- Está disponible en la versión del motor 10.2 de MariaDB. Para más información, consulte [Supported Azure Database for MariaDB database versions](./concepts-supported-versions.md) (Versiones de base de datos admitidas de Azure Database for MariaDB).

Dentro de un servidor de Azure Database for MariaDB, puede crear una o varias bases de datos. Puede optar por crear una sola base de datos por servidor para que se usen todos los recursos, o bien crear varias bases de datos para compartir los recursos. El precio está estructurado por servidor y se basa en la configuración del plan de tarifa, los núcleos virtuales y el almacenamiento (GB). Para más información, consulte [Planes de tarifa](./concepts-pricing-tiers.md).

## <a name="how-do-i-secure-an-azure-database-for-mariadb-server"></a>¿Cómo puedo proteger un servidor de Azure Database for MariaDB?

Los elementos siguientes ayudan a garantizar el acceso seguro a la base de datos.

|||
| :--| :--|
| **Autenticación y autorización** | El servidor de Azure Database for MariaDB admite la autenticación nativa de MySQL. Puede conectarse a un servidor y autenticarse en él con el inicio de sesión de administrador del servidor. |
| **Protocolo** | El servicio admite un protocolo basado en mensajes utilizado por MySQL. |
| **TCP/IP** | Se admite el protocolo a través de TCP/IP y a través de sockets de dominio de Unix. |
| **Firewall** | Para ayudar a mantener los datos protegidos, una regla de firewall impide todo acceso al servidor de bases de datos, hasta que se especifique qué equipos cuentan con permiso. Vea [Reglas de firewall del servidor de Azure Database for MariaDB](./concepts-firewall-rules.md). |
| **SSL** | El servicio permite establecer conexiones SSL entre las aplicaciones y el servidor de bases de datos. Vea [Configuración de la conectividad SSL en la aplicación para conectarse de forma segura a Azure Database for MariaDB](./howto-configure-ssl.md). |

## <a name="stopstart-an-azure-database-for-mariadb-preview"></a>Inicio y detención de una instancia de Azure Database for MariaDB (versión preliminar)
Azure Database for MariaDB permite **detener** el servidor cuando no está en uso e **iniciar** el servidor cuando se reanuda la actividad. Esto se hace básicamente para ahorrar costos en los servidores de bases de datos, ya que el recurso solo se paga cuando se usa. Esto cobra aún más importancia en las cargas de trabajo de desarrollo y pruebas, y cuando el servidor solo se usa durante una parte del día. Al detener el servidor, se anularán todas las conexiones activas. Más adelante, cuando desee volver a poner el servidor en línea, puede usar [Azure Portal](../mysql/how-to-stop-start-server.md) o la [CLI](../mysql/how-to-stop-start-server.md).

Si el servidor está en estado **detenido**, el proceso del mismo no se factura. Sin embargo, el almacenamiento se sigue facturando mientras se mantiene el almacenamiento del servidor, con el fin de asegurarse de que los archivos de datos están disponibles cuando se vuelve a iniciar el servidor.

> [!IMPORTANT]
> Cuando se **detiene** el servidor permanece en ese estado durante los siete días siguientes. Si no lo **inicia** de forma manual durante ese tiempo, se iniciará automáticamente al final de los siete días. Puede optar por volver a **detenerlo** si no va a usar el servidor.

Durante el tiempo en que el servidor está detenido, no se pueden realizar operaciones de administración en él. Para cambiar cualquier valor de la configuración del servidor, deberá [iniciar el servidor](../mysql/how-to-stop-start-server.md).

### <a name="limitations-of-stopstart-operation"></a>Limitaciones de las operaciones de detención e inicio
- No es compatible con las configuraciones de réplica de lectura (tanto de origen como de réplicas).

## <a name="how-do-i-manage-a-server"></a>¿Cómo se administra un servidor?
Puede administrar servidores de Azure Database for MariaDB en Azure Portal o con la CLI de Azure.

## <a name="next-steps"></a>Pasos siguientes
- Para obtener información general sobre el servicio, consulte [Introducción a Azure Database for MariaDB](./overview.md)
- Para obtener información sobre las cuotas y limitaciones aplicables a recursos específicos en función de su **nivel de servicio**, consulte [Niveles de servicio](./concepts-pricing-tiers.md).

<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->
