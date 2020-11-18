---
title: Conceptos sobre servidores de Azure Database for MySQL
description: En este tema se incluyen consideraciones e instrucciones para trabajar con servidores de Azure Database for MySQL.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 5477aa0c4eb2a66af816b287a8b6d7da8a53952f
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94534898"
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Conceptos sobre servidores de Azure Database for MySQL

En este artículo se incluyen consideraciones e instrucciones para trabajar con servidores de Azure Database for MySQL.

## <a name="what-is-an-azure-database-for-mysql-server"></a>¿Qué es un servidor de Azure Database for MySQL?

Un servidor de Azure Database for MySQL es un punto central de administración de varias bases de datos. Se trata de la misma construcción de servidores MySQL con la que puede estar familiarizado en entornos locales. En concreto, el servicio Azure Database for MySQL se administra, ofrece garantías de rendimiento y expone el acceso y las características a nivel de servidor.

Un servidor de Azure Database for MySQL:

- Se crea dentro de una suscripción de Azure.
- Es el recurso principal de las bases de datos.
- Proporciona un espacio de nombres para las bases de datos.
- Es un contenedor con semántica de duración segura. Si se elimina un servidor, se eliminan las bases de datos que contiene.
- Coloca recursos en una región.
- Proporciona un punto de conexión para el acceso a la base de datos y al servidor.
- Proporciona el ámbito de las directivas de administración que se aplican a sus bases de datos: inicio de sesión, firewall, usuarios, roles, configuración, etc.
- Está disponible en varias versiones. Para más información, vea [Supported Azure Database for MySQL database versions](./concepts-supported-versions.md) (Versiones de base de datos admitidas de Azure Database for MySQL).

En un servidor de Azure Database for MySQL, puede crear una o varias bases de datos. Puede optar por crear una sola base de datos por servidor para que se usen todos los recursos, o bien crear varias bases de datos para compartir los recursos. El precio está estructurado por servidor y se basa en la configuración del plan de tarifa, los núcleos virtuales y el almacenamiento (GB). Para más información, consulte [Planes de tarifa](./concepts-pricing-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>¿Cómo conectarse a un servidor de Azure Database for MySQL y autenticarse en él?

Los elementos siguientes ayudan a garantizar el acceso seguro a la base de datos.

|     |     |
| :-- | :-- |
| **Autenticación y autorización** | El servidor de Azure Database for MySQL admite la autenticación nativa de MySQL. Puede conectarse a un servidor y autenticarse en él con el inicio de sesión de administrador del servidor. |
| **Protocolo** | El servicio admite un protocolo basado en mensajes utilizado por MySQL. |
| **TCP/IP** | Se admite el protocolo a través de TCP/IP y a través de sockets de dominio de Unix. |
| **Firewall** | Para ayudar a mantener los datos protegidos, una regla de firewall impide todo acceso al servidor de bases de datos, hasta que se especifique qué equipos cuentan con permiso. Vea [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md) (Reglas de firewall del servidor de Azure Database for MySQL). |
| **SSL** | El servicio permite establecer conexiones SSL entre las aplicaciones y el servidor de bases de datos.  Consulte [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (Configuración de la conectividad SSL en la aplicación para conectarse de forma segura a Azure Database for MySQL). |

## <a name="stopstart-an-azure-database-for-mysql-preview"></a>Inicio o detención de una instancia de Azure Database for MySQL (versión preliminar)

Azure Database for MySQL permite **detener** el servidor cuando no está en uso e **iniciar** el servidor cuando se reanuda la actividad. Esto se hace básicamente para ahorrar costos en los servidores de bases de datos, ya que el recurso solo se paga cuando se usa. Esto cobra aún más importancia en las cargas de trabajo de desarrollo y pruebas, y cuando el servidor solo se usa durante una parte del día. Al detener el servidor, se anularán todas las conexiones activas. Más adelante, cuando desee volver a poner el servidor en línea, puede usar [Azure Portal](how-to-stop-start-server.md) o la [CLI](how-to-stop-start-server.md).

Si el servidor está en estado **detenido**, el proceso del mismo no se factura. Sin embargo, el almacenamiento se sigue facturando mientras se mantiene el almacenamiento del servidor, con el fin de asegurarse de que los archivos de datos están disponibles cuando se vuelve a iniciar el servidor.

> [!IMPORTANT]
> Cuando se **detiene** el servidor permanece en ese estado durante los siete días siguientes. Si no lo **inicia** de forma manual durante ese tiempo, se iniciará automáticamente al final de los siete días. Puede optar por volver a **detenerlo** si no va a usar el servidor.

Durante el tiempo en que el servidor está detenido, no se pueden realizar operaciones de administración en él. Para cambiar cualquier valor de la configuración del servidor, deberá [iniciar el servidor](how-to-stop-start-server.md).

### <a name="limitations-of-stopstart-operation"></a>Limitaciones de las operaciones de detención e inicio
- No es compatible con las configuraciones de réplica de lectura (tanto de origen como de réplicas).

## <a name="how-do-i-manage-a-server"></a>¿Cómo se administra un servidor?

Puede administrar servidores de Azure Database for MySQL en Azure Portal o con la CLI de Azure.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre el servicio, vea [Introducción a Azure Database for MySQL](./overview.md).
- Para obtener información sobre las cuotas y las limitaciones de recursos específicos en función del **plan de tarifa**, vea [Planes de tarifa](./concepts-pricing-tiers.md)
- Para obtener información sobre cómo conectarse al servicio, vea [Bibliotecas de conexiones de Azure Database for MySQL](./concepts-connection-libraries.md).
