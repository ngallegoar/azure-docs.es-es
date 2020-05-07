---
title: 'TLS en Azure Database for PostgreSQL: servidor único'
description: 'Instrucciones e información sobre cómo configurar la conectividad de TSL para Azure Database for PostgreSQL: un solo servidor.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: d0482e5205b97b5c57c41e0ba98fb9ca819e5d5f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82141740"
---
# <a name="configure-tls-connectivity-in-azure-database-for-postgresql---single-server"></a>Configuración de la conectividad de TSL en Azure Database for PostgreSQL: un solo servidor

Azure Database for PostgreSQL prefiere conectar las aplicaciones cliente al servicio PostgreSQL mediante Seguridad de la capa de transporte (TLS), que anteriormente se denominaba Capa de sockets seguros (SSL). Aplicar conexiones TLS entre el servidor de bases de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "Man in the middle" mediante el cifrado del flujo de datos entre el servidor y la aplicación.

De forma predeterminada, el servicio de base de datos de PostgreSQL está configurado para requerir una conexión TLS. Puede deshabilitar el requisito de TLS si la aplicación cliente no admite la conectividad de TLS.

## <a name="enforcing-tls-connections"></a>Aplicación de conexiones TLS

En el caso de todos los servidores de Azure Database for PostgreSQL aprovisionados a través de Azure Portal y la interfaz de la línea de comandos, el establecimiento de conexiones TLS está habilitado de forma predeterminada. 

Del mismo modo, las cadenas de conexión que están predefinidas en la configuración de "Cadenas de conexión", en el servidor en Azure Portal, incluyen los parámetros necesarios de lenguajes comunes para conectarse a su servidor de bases de datos mediante TLS. El parámetro TLS varía según el conector, por ejemplo, "ssl = true" o "sslmode = require" o "sslmode = required" y otras variaciones.

## <a name="configure-enforcement-of-tls"></a>Configuración de la aplicación de TLS

Si lo desea, puede deshabilitar el establecimiento de la conectividad TLS. Microsoft Azure recomienda habilitar siempre la opción de configuración **Enforce SSL connection** (Establecer conexión SSL) para mayor seguridad.

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Visite el servidor de Azure Database for PostgreSQL y haga clic en **Seguridad de conexión**. Use el botón de alternancia para habilitar o deshabilitar la opción **Aplicar conexión SSL**. A continuación, haga clic en **Guardar**.

![Seguridad de conexión - Deshabilitar el establecimiento de TLS/SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Para confirmar la configuración, consulte la página **Información general** para ver el indicador **Estado de aplicación de SSL**.

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

Puede habilitar o deshabilitar el parámetro **ssl-enforcement** con los valores `Enabled` o `Disabled` respectivamente en la CLI de Azure.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Comprobación de que la aplicación o el marco de trabajo admiten conexiones TLS

Algunos marcos de trabajo de aplicaciones que usan PostgreSQL en los servicios de bases de datos no habilitan TLS de manera predeterminada durante la instalación. Si el servidor PostgreSQL establece conexiones TLS, pero la aplicación no está configurada para dicho protocolo, esta puede presentar un error al conectarse al servidor de bases de datos. Consulte la documentación de la aplicación para aprender a habilitar las conexiones TLS.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>Aplicaciones que requieren la verificación del certificado para la conectividad TLS

En algunos casos, las aplicaciones requieren un archivo de certificado local generado a partir de un archivo de certificado (.cer) de una entidad de certificación (CA) de confianza para conectarse de forma segura. El certificado para conectarse a un servidor de Azure Database for PostgreSQL se encuentra en https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem. Descargue el archivo de certificado y guárdelo en la ubicación que prefiera.

### <a name="connect-using-psql"></a>Conexión mediante psql

En el ejemplo siguiente se muestra cómo conectarse a su servidor de PostgreSQL mediante la utilidad de línea de comandos psql. Use la configuración de la cadena de conexión `sslmode=verify-full` para aplicar la comprobación del certificado TLS/SSL. Pase la ruta de acceso al archivo del certificado local al parámetro `sslrootcert`.

El comando siguiente es un ejemplo de la cadena de conexión de psql:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Confirme que el valor pasado a `sslrootcert` coincide con la ruta de acceso al archivo del certificado que guardó.

## <a name="next-steps"></a>Pasos siguientes

Revise las distintas opciones de conectividad de aplicaciones en [Bibliotecas de conexiones de Azure Database for PostgreSQL](concepts-connection-libraries.md).
