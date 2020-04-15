---
title: Cómo configurar una instancia administrada
description: Aprenda a configurar y administrar una instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
ms.date: 04/16/2019
ms.openlocfilehash: b56375388f6293d27bcd2f2548d8b20205a92b15
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80638029"
---
# <a name="how-to-use-a-managed-instance-in-azure-sql-database"></a>Cómo usar una instancia administrada en Azure SQL Database

En este artículo encontrará varias guías, scripts y explicaciones que pueden ayudarle a administrar y configurar su instancia administrada.

## <a name="migration"></a>Migración

- [Migrar a una instancia administrada](sql-database-managed-instance-migrate.md): obtenga información sobre el proceso de migración y las herramientas que se recomiendan para realizar la migración a una instancia administrada.

- [Migrar el certificado de TDE a una instancia administrada](sql-database-managed-instance-migrate-tde-certificate.md): si su base de datos de SQL Server está protegida con un cifrado de datos transparente (TDE), deberá migrar el certificado que una instancia administrada puede usar para descifrar la copia de seguridad que quiera restaurar en Azure.

## <a name="network-configuration"></a>Network configuration (Configuración de red)

- [Determinar el tamaño de la subred de una instancia administrada](sql-database-managed-instance-determine-size-vnet-subnet.md): la instancia administrada se coloca en una subred dedicada que no se puede cambiar de tamaño una vez se le agregan recursos. Por lo tanto, debería calcular el intervalo IP de direcciones necesario para la subred según el número y los tipos de instancias que quiera implementar en la subred.
- [Crear una nueva red virtual y una subred para una instancia administrada](sql-database-managed-instance-create-vnet-subnet.md): la red virtual y la subred de Azure donde quiere implementar las instancias administradas deben configurarse de acuerdo a los [requisitos de red que se describen aquí](sql-database-managed-instance-connectivity-architecture.md#network-requirements). En esta guía puede encontrar la manera más fácil de crear una red virtual y una subred que estén configuradas correctamente para las instancias administradas.
- [Configurar la red virtual y la subred existentes para una instancia administrada](sql-database-managed-instance-configure-vnet-subnet.md): si quiere configurar la red virtual y la subred existentes para implementar instancias administradas en ellas, aquí puede encontrar el script que comprueba los [requisitos de red](sql-database-managed-instance-connectivity-architecture.md#network-requirements) y que configura la subred según los requisitos.
- [Configurar el DNS personalizado](sql-database-managed-instance-custom-dns.md): debe configurar el DNS personalizado si quiere obtener acceso a los recursos externos de los dominios personalizados desde la instancia administrada y a través de un servidor vinculado de perfiles de correo de base de datos.
- [Sincronizar la configuración de red](sql-database-managed-instance-sync-network-configuration.md): puede suceder que, aunque haya [integrado la aplicación con una red de Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md), no pueda conectarse a una instancia administrada. Puede intentar actualizar la configuración de red para el plan de servicio que tenga.
- [Buscar la dirección IP del punto de conexión de administración](sql-database-managed-instance-find-management-endpoint-ip-address.md): la instancia administrada usa el punto de conexión público con fines administrativos. Puede determinar la dirección IP del punto de conexión de administración mediante el script que se describe aquí.
- [Comprobar la protección de firewall integrado](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md): la instancia administrada está protegida con un firewall integrado que permite el tráfico solo en los puertos necesarios. Puede comprobar las reglas de firewall integradas mediante el script que se describe en esta guía.
- [Conectar aplicaciones](sql-database-managed-instance-connect-app.md): la instancia administrada se coloca en su propia red virtual privada de Azure con la dirección IP privada. Obtenga información sobre los distintos patrones para conectar las aplicaciones a su instancia administrada.

## <a name="feature-configuration"></a>Configuración de características

- La [replicación transaccional](replication-with-sql-database-managed-instance.md) le permite replicar los datos entre instancias administradas, o desde un entorno local de SQL Server a una instancia administrada y viceversa. Puede encontrar más información sobre cómo usar y configurar la replicación de transacciones en esta guía.
- [Configurar la detección de amenazas](sql-database-managed-instance-threat-detection.md):la [detección de amenazas](sql-database-threat-detection-overview.md) es una característica integrada de Azure SQL Database que detecta diversos ataques potenciales, como la inyección de código SQL o el acceso desde ubicaciones sospechosas. En esta guía obtendrá información sobre cómo habilitar y configurar la [detección de amenazas](sql-database-threat-detection-overview.md) de una instancia administrada.
- La [creación de alertas](sql-database-managed-instance-alerts.md) permite configurar las alertas de las métricas supervisadas, como el uso de CPU, el consumo de espacio de almacenamiento, el número de IOPS por segundo y demás para la instancia administrada. En esta guía aprenderá a habilitar y configurar alertas para la instancia administrada.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [guías de procedimientos para bases de datos únicas](sql-database-howto-single-database.md).
