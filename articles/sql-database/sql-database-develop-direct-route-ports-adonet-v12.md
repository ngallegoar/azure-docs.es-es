---
title: Puertos más allá de 1433
description: Las conexiones de cliente de ADO.NET a Azure SQL Database pueden omitir el proxy e interactuar directamente con la base de datos a través de otros puertos que no sean el 1433.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
ms.date: 04/03/2019
ms.openlocfilehash: 34e9b40b0b6989edacd72eb23d357e2a84b6f8d8
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891077"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Puertos más allá de 1433 para ADO.NET 4.5

En este tema se describe el comportamiento de conexión de Azure SQL Database para clientes que usan ADO.NET 4.5 o una versión posterior.

> [!IMPORTANT]
> Para obtener información acerca de la arquitectura de conectividad, consulte [Arquitectura de conectividad de Azure SQL Database](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Fuera o dentro

Para las conexiones a Azure SQL Database, debemos preguntar si el programa cliente se ejecuta *fuera* o *dentro* del límite de la nube de Azure. En las subsecciones se describen dos escenarios comunes.

### <a name="outside-client-runs-on-your-desktop-computer"></a>*Fuera:* el cliente se ejecuta en un equipo de escritorio.

El puerto 1433 es el único puerto que debe estar abierto en su equipo de escritorio que hospeda su aplicación de cliente de la SQL Database.

### <a name="inside-client-runs-on-azure"></a>*Dentro:* el cliente se ejecuta en Azure.

Cuando el cliente se ejecuta dentro del límite de la nube de Azure, usa lo que podemos llamar una *ruta directa* para interactuar con el servidor de SQL Database. Cuando se ha establecido una conexión, las interacciones posteriores entre el cliente y la base de datos no implican a ninguna puerta de enlace de Azure SQL Database.

La secuencia es la siguiente:

1. ADO.NET 4.5 (o posterior) inicia una breve interacción con la nube de Azure y recibe un número de puerto identificado dinámicamente.

   * El número de puerto identificado dinámicamente se encuentra en el intervalo de 11000-11999.
2. Luego, ADO.NET se conecta al servidor de SQL Database directamente, sin ningún middleware entre ellos.
3. Las consultas se envían directamente a la base de datos y los resultados se devuelven directamente al cliente.

Asegúrese de que los intervalos de puertos de 11000 a 11999 en la máquina cliente de Azure queden disponibles para las interacciones de cliente de ADO.NET 4.5 con SQL Database.

* En concreto, los puertos del intervalo deben estar libres de cualquier otro bloqueador de salida.
* En la máquina virtual de Azure, **Firewall de Windows con seguridad avanzada** controla la configuración de puertos.
  
  * Puede usar la [interfaz de usuario del firewall](https://msdn.microsoft.com/library/cc646023.aspx) para agregar una regla para la que se especifique el protocolo **TCP** junto con un intervalo de puertos con una sintaxis similar a **11000-11999**.

## <a name="version-clarifications"></a>Aclaraciones de versiones

En esta sección se explican los monikers que hacen referencia a las versiones de producto. También se muestran algunos emparejamientos de versiones entre productos.

### <a name="adonet"></a>ADO.NET

* ADO.NET 4.0 admite el protocolo TDS 7.3, pero no 7.4.
* ADO.NET 4.5 y versiones posteriores admite el protocolo TDS 7.4.

### <a name="odbc"></a>ODBC

* Microsoft SQL Server ODBC 11 o posterior

### <a name="jdbc"></a>JDBC

* Microsoft SQL Server JDBC 4.2 o posterior (JDBC 4.0 realmente admite TDS 7.4, pero no implementa el "redireccionamiento")

## <a name="related-links"></a>Vínculos relacionados

* ADO.NET 4.6 se publicó el 20 de julio de 2015. Hay un anuncio de blog del equipo de .NET disponible [aquí](https://devblogs.microsoft.com/dotnet/announcing-net-framework-4-6/).
* ADO.NET 4.5 se publicó el 15 de julio de 2012. Hay un anuncio de blog del equipo de .NET disponible [aquí](https://devblogs.microsoft.com/dotnet/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code/).
  * Hay una entrada de blog sobre ADO.NET 4.5.1 disponible [aquí](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/).

* Microsoft® ODBC Driver 17 for SQL Server®: Windows, Linux y macOS https://www.microsoft.com/download/details.aspx?id=56567

* Conexión a Azure SQL Database V12 mediante un redireccionamiento https://techcommunity.microsoft.com/t5/DataCAT/Connect-to-Azure-SQL-Database-V12-via-Redirection/ba-p/305362

* [Lista de versiones del protocolo TDS](https://www.freetds.org/userguide/tdshistory.htm)
* [Información general de desarrollo de SQL Database](sql-database-develop-overview.md)
* [Firewall de Azure SQL Database](sql-database-firewall-configure.md)
* [Cómo: configuración del firewall en SQL Database](sql-database-configure-firewall-settings.md)


