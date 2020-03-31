---
title: 'SSL en Azure Database for PostgreSQL: servidor único'
description: 'Instrucciones e información sobre cómo configurar la conectividad SSL para Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 303da4dcb68a79e69254f6610afc0003bf0aa22c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477007"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>Configuración de la conectividad SSL en Azure Database for PostgreSQL: servidor único

Azure Database for PostgreSQL prefiere conectar las aplicaciones cliente al servicio de PostgreSQL mediante la Capa de sockets seguros (SSL). Aplicar conexiones SSL entre el servidor de base de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "Man in the middle" mediante el cifrado del flujo de datos entre el servidor y la aplicación.

De forma predeterminada, el servicio de base de datos de PostgreSQL está configurado para requerir la conexión SSL. Puede deshabilitar el requisito de SSL si la aplicación cliente no admite la conectividad SSL.

## <a name="enforcing-ssl-connections"></a>Aplicación de conexiones SSL

Para todos los servidores de Azure Database for PostgreSQL aprovisionados en Azure Portal o con la CLI de Azure, el establecimiento de conexiones SSL está habilitado de forma predeterminada. 

Del mismo modo, las cadenas de conexión que están predefinidas en la configuración de "Cadenas de conexión", en el servidor en Azure Portal, incluyen los parámetros necesarios para los lenguajes más comunes para conectarse a su servidor de base de datos mediante SSL. El parámetro SSL varía según el conector, por ejemplo, "ssl = true" o "sslmode = require" o "sslmode = required" y otras variaciones.

## <a name="configure-enforcement-of-ssl"></a>Configuración de la aplicación de SSL

Si lo desea, puede deshabilitar el establecimiento de la conectividad SSL. Microsoft Azure recomienda habilitar siempre la opción de configuración **Enforce SSL connection** (Establecer conexión SSL) para mayor seguridad.

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Visite el servidor de Azure Database for PostgreSQL y haga clic en **Seguridad de conexión**. Use el botón de alternancia para habilitar o deshabilitar la opción **Aplicar conexión SSL**. A continuación, haga clic en **Guardar**.

![Seguridad de conexión - Deshabilitar el establecimiento de SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Para confirmar la configuración, consulte la página **Información general** para ver el indicador **Estado de aplicación de SSL**.

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

Puede habilitar o deshabilitar el parámetro **ssl-enforcement** con los valores `Enabled` o `Disabled` respectivamente en la CLI de Azure.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Comprobación de que la aplicación o el entorno admiten conexiones SSL

Algunos marcos de trabajo de aplicaciones comunes que usan PostgreSQL en los servicios de bases de datos no habilitan SSL de manera predeterminada durante la instalación. Si el servidor PostgreSQL establece conexiones SSL, pero la aplicación no está configurada para SSL, esta puede presentar un error al conectarse al servidor de bases de datos. Consulte la documentación de la aplicación para obtener información sobre cómo habilitar las conexiones SSL.

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Aplicaciones que requieren la verificación de certificados para la conectividad SSL

En algunos casos, las aplicaciones requieren un archivo de certificado local generado a partir de un archivo de certificado (.cer) de una entidad de certificación (CA) de confianza para conectarse de forma segura. El certificado para conectarse a un servidor de Azure Database for PostgreSQL se encuentra en https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem. Descargue el archivo de certificado y guárdelo en la ubicación que prefiera.

### <a name="connect-using-psql"></a>Conexión mediante psql

En el ejemplo siguiente se muestra cómo conectarse a su servidor de PostgreSQL mediante la utilidad de línea de comandos psql. Use la configuración de la cadena de conexión `sslmode=verify-full` para aplicar la comprobación del certificado SSL. Pase la ruta de acceso al archivo del certificado local al parámetro `sslrootcert`.

El comando siguiente es un ejemplo de la cadena de conexión de psql:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Confirme que el valor pasado a `sslrootcert` coincide con la ruta de acceso al archivo del certificado que guardó.

## <a name="next-steps"></a>Pasos siguientes

Revise las distintas opciones de conectividad de aplicaciones en [Bibliotecas de conexiones de Azure Database for PostgreSQL](concepts-connection-libraries.md).
