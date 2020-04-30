---
title: 'TLS: Hiperescala (Citus) en Azure Database for PostgreSQL'
description: 'Instrucciones e información para configurar Azure Database for PostgreSQL: Hiperescala (Citus) y las aplicaciones asociadas, a fin de usar correctamente las conexiones TLS.'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: 791eed9419375c7245488b8ec61a1c5481be382e
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82580567"
---
# <a name="configure-tls-in-azure-database-for-postgresql---hyperscale-citus"></a>Configuración de TLS in Azure Database for PostgreSQL: Hiperescala (Citus)
Las conexiones de la aplicación cliente al nodo de coordinación de Hiperescala (Citus) requieren la Seguridad de la capa de transporte (TLS), antes conocida como Capa de sockets seguros (SSL). Aplicar conexiones TLS entre el servidor de bases de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "Man in the middle" mediante el cifrado del flujo de datos entre el servidor y la aplicación.

## <a name="enforcing-tls-connections"></a>Aplicación de conexiones TLS
Para todos los servidores Azure Database for PostgreSQL aprovisionados en Azure Portal, la obligatoriedad de las conexiones TLS está habilitada de forma predeterminada. 

Del mismo modo, las cadenas de conexión que están predefinidas en la configuración de "Cadenas de conexión", en el servidor en Azure Portal, incluyen los parámetros necesarios de lenguajes comunes para conectarse a su servidor de bases de datos mediante TLS. El parámetro TLS varía según el conector, por ejemplo, "ssl = true" o "sslmode = require" o "sslmode = required" y otras variaciones.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Comprobación de que la aplicación o el marco de trabajo admiten conexiones TLS
Algunos marcos de trabajo de aplicaciones que usan PostgreSQL en los servicios de bases de datos no habilitan TLS de manera predeterminada durante la instalación. Si el servidor PostgreSQL establece conexiones TLS, pero la aplicación no está configurada para dicho protocolo, esta puede presentar un error al conectarse al servidor de bases de datos. Consulte la documentación de la aplicación para aprender a habilitar las conexiones TLS.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>Aplicaciones que requieren la verificación del certificado para la conectividad TLS
En algunos casos, las aplicaciones requieren un archivo de certificado local generado a partir de un archivo de certificado (.cer) de una entidad de certificación (CA) de confianza para conectarse de forma segura. El certificado para conectarse a un servidor Azure Database for PostgreSQL: Hiperescala (Citus) se encuentra en https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem. Descargue el archivo de certificado y guárdelo en la ubicación que prefiera.

### <a name="connect-using-psql"></a>Conexión mediante psql
En el ejemplo siguiente se muestra cómo conectarse a su nodo de coordinación de Hiperescala (Citus) mediante la utilidad de línea de comandos psql. Use la configuración de la cadena de conexión `sslmode=verify-full` para aplicar la comprobación del certificado TLS. Pase la ruta de acceso al archivo del certificado local al parámetro `sslrootcert`.

Luego aparece un ejemplo de la cadena de conexión psql:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Confirme que el valor pasado a `sslrootcert` coincide con la ruta de acceso al archivo del certificado que guardó.

## <a name="next-steps"></a>Pasos siguientes
Aumente aún más la seguridad con las [reglas de firewall de Azure Database for PostgreSQL: Hiperescala (Citus)](concepts-hyperscale-firewall-rules.md).
