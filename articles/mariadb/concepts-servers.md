---
title: 'Servidores: Azure Database for MariaDB'
description: En este tema se incluyen consideraciones e instrucciones para trabajar con servidores de Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 444d7f1574cf1517b01250bcb9d810731030182d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "79527799"
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

## <a name="how-do-i-manage-a-server"></a>¿Cómo se administra un servidor?
Puede administrar servidores de Azure Database for MariaDB en Azure Portal o con la CLI de Azure.

## <a name="next-steps"></a>Pasos siguientes
- Para obtener información general sobre el servicio, consulte [Introducción a Azure Database for MariaDB](./overview.md)
- Para obtener información sobre las cuotas y limitaciones aplicables a recursos específicos en función de su **nivel de servicio**, consulte [Niveles de servicio](./concepts-pricing-tiers.md).

<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->
