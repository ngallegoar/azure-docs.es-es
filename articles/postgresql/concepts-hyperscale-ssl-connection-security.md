---
title: SSL de Hiperescala (Citus) en Azure Database for PostgreSQL
description: 'Instrucciones e información para configurar Azure Database for PostgreSQL: Hiperescala (Citus) y las aplicaciones asociadas, a fin de usar correctamente las conexiones SSL.'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: 3e4ef5d2d6db3a3d4f8923f47079f2484639a751
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74973992"
---
# <a name="configure-ssl-in-azure-database-for-postgresql---hyperscale-citus"></a>Configuración de SSL in Azure Database for PostgreSQL: Hiperescala (Citus)
Las conexiones de la aplicación cliente al nodo de coordinación de Hiperescala (Citus) requieren la Capa de sockets seguros (SSL). Aplicar conexiones SSL entre el servidor de base de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "Man in the middle" mediante el cifrado del flujo de datos entre el servidor y la aplicación.

## <a name="enforcing-ssl-connections"></a>Aplicación de conexiones SSL
Para todos los servidores Azure Database for PostgreSQL aprovisionados en Azure Portal, la obligatoriedad de las conexiones SSL está habilitada de forma predeterminada. 

Del mismo modo, las cadenas de conexión que están predefinidas en la configuración de "Cadenas de conexión", en el servidor en Azure Portal, incluyen los parámetros necesarios para los lenguajes más comunes para conectarse a su servidor de base de datos mediante SSL. El parámetro SSL varía según el conector, por ejemplo, "ssl = true" o "sslmode = require" o "sslmode = required" y otras variaciones.

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Comprobación de que la aplicación o el entorno admiten conexiones SSL
Algunos marcos de trabajo de aplicaciones comunes que usan PostgreSQL en los servicios de bases de datos no habilitan SSL de manera predeterminada durante la instalación. Si el servidor PostgreSQL establece conexiones SSL, pero la aplicación no está configurada para SSL, esta puede presentar un error al conectarse al servidor de bases de datos. Consulte la documentación de la aplicación para obtener información sobre cómo habilitar las conexiones SSL.

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Aplicaciones que requieren la verificación de certificados para la conectividad SSL
En algunos casos, las aplicaciones requieren un archivo de certificado local generado a partir de un archivo de certificado (.cer) de una entidad de certificación (CA) de confianza para conectarse de forma segura. El certificado para conectarse a un servidor Azure Database for PostgreSQL: Hiperescala (Citus) se encuentra en https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem. Descargue el archivo de certificado y guárdelo en la ubicación que prefiera.

### <a name="connect-using-psql"></a>Conexión mediante psql
En el ejemplo siguiente se muestra cómo conectarse a su nodo de coordinación de Hiperescala (Citus) mediante la utilidad de línea de comandos psql. Use la configuración de la cadena de conexión `sslmode=verify-full` para aplicar la comprobación del certificado SSL. Pase la ruta de acceso al archivo del certificado local al parámetro `sslrootcert`.

Luego aparece un ejemplo de la cadena de conexión psql:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Confirme que el valor pasado a `sslrootcert` coincide con la ruta de acceso al archivo del certificado que guardó.

## <a name="next-steps"></a>Pasos siguientes
Aumente aún más la seguridad con las [reglas de firewall de Azure Database for PostgreSQL: Hiperescala (Citus)](concepts-hyperscale-firewall-rules.md).
