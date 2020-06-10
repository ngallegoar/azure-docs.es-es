---
title: Introducción sobre las transacciones de base de datos elástica con Base de datos SQL de Azure
description: Introducción sobre las transacciones de Elastic Database con Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/12/2019
ms.openlocfilehash: c1ecd5e66986df6affc186770b9da0decf2e92c6
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84031386"
---
# <a name="distributed-transactions-across-cloud-databases"></a>Introducción sobre las transacciones de base de datos elástica con Base de datos SQL de Azure
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Las transacciones de base de datos elástica para Azure SQL Database permiten ejecutar transacciones que abarcan varias bases de datos de SQL Database. Están disponibles para aplicaciones .NET mediante ADO .NET y se integran con la conocida experiencia de programación en la que se hace uso de las clases [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx). Para obtener la biblioteca, vea [.NET Framework 4.6.1 (instalador web)](https://www.microsoft.com/download/details.aspx?id=49981).

De forma local, este escenario requiere normalmente la ejecución del Coordinador de transacciones distribuidas de Microsoft (MSDTC). Puesto que MSDTC no está disponible en Azure para aplicaciones de plataforma como servicio, la capacidad de coordinar transacciones distribuidas ahora se integra directamente en SQL Database. Las aplicaciones pueden conectarse a cualquier instancia de SQL Database para iniciar transacciones distribuidas, y una de las bases de datos coordinará de forma transparente la transacción distribuida, como se muestra en la ilustración siguiente.

  ![Transacciones distribuidas con Azure SQL Database mediante transacciones de base de datos elástica ][1]

## <a name="common-scenarios"></a>Escenarios frecuentes

Las transacciones de base de datos elástica para SQL Database permiten a las aplicaciones realizar cambios atómicos en los datos almacenados en varias instancias de SQL Database diferentes. La versión preliminar se centra en las experiencias de desarrollo del lado cliente en C# y. NET. Para más adelante está prevista una experiencia en el lado servidor mediante T-SQL.  
Las transacciones de base de datos elástica están orientadas a los siguientes escenarios:

* Aplicaciones de varias bases de datos en Azure: con este escenario, los datos se particionan verticalmente entre varias bases de datos de SQL Database, de forma que diferentes clases de datos residen en diferentes bases de datos. Algunas operaciones requieren cambios en los datos que se mantienen en dos o más bases de datos. La aplicación emplea transacciones de base de datos elástica para coordinar los cambios entre las bases de datos y garantizar la atomicidad.
* Aplicaciones de base de datos particionada en Azure: con este escenario, la capa de datos usa la [biblioteca cliente de Base de datos elástica](elastic-database-client-library.md) o el particionamiento automático para particionar horizontalmente los datos entre muchas bases de datos en SQL Database. Un caso destacado de uso es cuando existe la necesidad de realizar cambios atómicos en una aplicación particionada multiempresa cuando los cambios abarcan a los inquilinos. Piense por ejemplo en una transferencia desde un inquilino a otro, donde cada uno reside en una base de datos diferente. Un segundo caso es el particionamiento específico para satisfacer las necesidades de capacidad de un inquilino de gran tamaño, lo que a su vez supone normalmente que algunas operaciones atómicas deban extenderse entre varias bases de datos usadas para el mismo inquilino. Un tercer caso es el de las actualizaciones atómicas para hacer referencia a los datos que se replican entre bases de datos. Ahora se pueden coordinar operaciones de transacciones atómicas a lo largo de estas líneas entre varias bases de datos mediante la versión preliminar.
  Las transacciones de base de datos elástica usan una confirmación en dos fases para garantizar la atomicidad de las transacciones entre las bases de datos. Esto resulta adecuado para transacciones que suponen menos de 100 bases de datos a la vez en una única transacción. Aunque estos límites no se aplican, se supone que las tasas de rendimiento y éxito de las transacciones de base de datos elástica se verán afectadas cuando se excedan estos límites.

## <a name="installation-and-migration"></a>Instalación y migración

Las capacidades de las transacciones de base de datos elástica en SQL Database se proporcionan a través de actualizaciones de las bibliotecas .NET System.Data.dll y System.Transactions.dll. Los archivos DLL garantizan que la confirmación en dos fases se usa cuando es necesario para asegurar la atomicidad. Para empezar a desarrollar aplicaciones mediante transacciones de base de datos elástica, instale [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) o versión posterior. Cuando se ejecutan en una versión anterior de .NET Framework, las transacciones no podrán promoverse a una transacción distribuida y se producirá una excepción.

Tras la instalación, puede usar las API de transacciones distribuidas de System.Transactions con conexiones a SQL Database. Si tiene aplicaciones de MSDTC existentes que usan estas API, basta con que las regenere para .NET 4.6 después de instalar 4.6.1 Framework. Si los proyectos se destinan a .NET 4.6, usarán automáticamente los archivos DLL actualizados de la nueva versión de Framework, y las llamadas a API de transacciones distribuidas en combinación con las conexiones a SQL Database ahora se realizarán correctamente.

Recuerde que las transacciones de base de datos elástica no precisan la instalación de MSDTC. En su lugar, se administran directamente mediante SQL Database y dentro de esta. Con ello se simplifican enormemente los escenarios de nube, ya que no es necesario implementar MSDTC para usar transacciones distribuidas con SQL Database. En la sección 4 se explica con más detalle cómo implementar transacciones de base de datos elástica y  la versión de .NET Framework necesaria junto con sus aplicaciones de nube en Azure.

## <a name="development-experience"></a>Experiencia de desarrollo

### <a name="multi-database-applications"></a>Aplicaciones de varias bases de datos

En el ejemplo de código siguiente se usa la experiencia de programación conocida con System.Transactions de .NET. La clase TransactionScope establece una transacción de ambiente en. NET. (Una "transacción de ambiente" es aquella que reside en el subproceso actual). Todas las conexiones abiertas dentro de TransactionScope intervienen en la transacción. Si intervienen bases de datos diferentes, la transacción se eleva automáticamente a transacción distribuida. El resultado de la transacción se controla mediante la configuración del ámbito como completo para indicar una confirmación.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Aplicaciones de base de datos particionada

Las transacciones de base de datos elástica para SQL Database también admiten la coordinación de transacciones distribuidas en las que se usa el método OpenConnectionForKey de la biblioteca cliente de base de datos elástica a fin de abrir conexiones para una capa de datos escalada horizontalmente. Tenga en cuenta los casos en lo que deba garantizar la coherencia de las transacciones para los cambios entre varios valores diferentes de clave de particionamiento. Las conexiones a las particiones que hospedan los diferentes valores de clave de particionamiento se interrumpen mediante OpenConnectionForKey. Por lo general, las conexiones pueden ser a particiones diferentes de forma que, para garantizar las transacciones, se necesita una transacción distribuida.
En el ejemplo de código siguiente se muestra este método. Se supone que una variable llamada shardmap se usa para representar un mapa de particiones de la biblioteca de cliente de base de datos elástica:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

## <a name="net-installation-for-azure-cloud-services"></a>Instalación de .NET para Azure Cloud Services

Azure proporciona varias ofertas para hospedar aplicaciones. NET. Hay disponible una comparación de las diferentes ofertas en [Comparación de Azure App Service, Cloud Services y Virtual Machines](/azure/architecture/guide/technology-choices/compute-decision-tree). Si el SO invitado de la oferta es inferior a .NET 4.6.1, que es el que se requiere para las transacciones elásticas, debe actualizar el SO invitado a 4.6.1.

Para Azure App Service, no se admiten las actualizaciones del SO invitado en estos momentos. En el caso de Azure Virtual Machines, solo tiene que iniciar sesión en la máquina virtual y ejecutar el instalador para la última versión de .NET Framework. Para Azure Cloud Services, hay que incluir la instalación de una versión más reciente de .NET en las tareas de inicio de la implementación. Los conceptos y los pasos se documentan en [Instalación de .NET en un rol de servicio en la nube](../../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Tenga en cuenta que el instalador de .NET 4.6.1 puede requerir más espacio de almacenamiento temporal durante el proceso de arranque en los Servicios en la nube de Azure que el instalador de .NET 4.6. Para garantizar una instalación correcta, debe aumentar el almacenamiento temporal para el servicio en la nube de Azure en el archivo ServiceDefinition.csdef en la sección LocalResources y en la configuración del entorno de la tarea de inicio, como se muestra en el ejemplo siguiente:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Transacciones entre varios servidores

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos.

Se admiten transacciones de la base de datos elástica entre diferentes servidores en Azure SQL Database. Cuando las transacciones cruzan los límites del servidor, los servidores implicados en primer lugar deben involucrarse en una relación de comunicación mutua. Una vez que se ha establecido la relación de comunicación, cualquier base de datos ubicada en uno de los dos servidores puede participar en transacciones elásticas con bases de datos desde el otro servidor. En el caso de las transacciones distribuidas en más de dos servidores, debe existir una relación de comunicación para cualquier par de servidores.

Use los siguientes cmdlets de PowerShell para administrar las relaciones de comunicación entre los servidores para las transacciones de la base de datos elástica:

* **New-AzSqlServerCommunicationLink**: este cmdlet se usa para crear una relación de comunicación nueva entre dos servidores en Azure SQL Database. La relación es simétrica, lo que significa que ambos servidores pueden iniciar transacciones con el otro servidor.
* **Get-AzSqlServerCommunicationLink**: este cmdlet se usa para recuperar una relación de comunicación existente y sus propiedades.
* **Remove-AzSqlServerCommunicationLink**: este cmdlet se usa para eliminar una relación de comunicación existente.

## <a name="monitoring-transaction-status"></a>Supervisión del estado de la transacción

Use Vistas de administración dinámica (DMV) en SQL Database para supervisar el estado y el progreso de las transacciones en curso de base de datos elástica. Todas las DMV relacionadas con las transacciones son pertinentes para las transacciones distribuidas en SQL Database. Puede encontrar la lista correspondiente de DMV aquí: [Funciones y vistas de administración dinámica relacionadas con transacciones (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Estas DMV son especialmente útiles:

* **sys.dm\_tran\_active\_transactions**: enumera las transacciones actualmente activas y su estado. La columna UOW (unidad de trabajo) puede identificar las distintas transacciones secundarias que pertenecen a la misma transacción distribuida. Todas las transacciones de la misma transacción distribuida llevan el mismo valor de UOW. Consulte la [documentación de DMV](https://msdn.microsoft.com/library/ms174302.aspx) para más información.
* **sys.dm\_tran\_database\_transactions**: proporciona información adicional sobre las transacciones, como la colocación de la transacción en el registro. Consulte la [documentación de DMV](https://msdn.microsoft.com/library/ms186957.aspx) para más información.
* **sys.dm\_tran\_locks**: ofrece información sobre los bloqueos actuales de las transacciones. Consulte la [documentación de DMV](https://msdn.microsoft.com/library/ms190345.aspx) para más información.

## <a name="limitations"></a>Limitaciones

Las limitaciones siguientes se aplican actualmente a transacciones de base de datos elástica en SQL Database:

* Se admiten únicamente transacciones entre bases de datos en SQL Database. Otros proveedores de recursos y bases de datos de [X/Open XA](https://en.wikipedia.org/wiki/X/Open_XA) externos a SQL Database no podrán participar en transacciones de base de datos elástica. Esto significa que dichas transacciones no pueden extenderse entre bases de datos locales de SQL Server y Azure SQL Database. Para las transacciones distribuidas en el entorno local, siga usando MSDTC.
* Solo se admiten transacciones coordinadas por el cliente desde una aplicación .NET. Está prevista la compatibilidad en el lado servidor con T-SQL, por ejemplo, INICIAR TRANSACCIÓN DISTRIBUIDA, pero aún no se encuentra disponible.
* No se admiten las transacciones por los servicios de WCF. Por ejemplo, tiene un método de servicio de WCF que se ejecuta una transacción. Si se encierra la llamada dentro de un ámbito de transacción, se producirá un error como [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Pasos siguientes

Si tiene alguna pregunta, póngase en contacto con nosotros en la [Página de preguntas y respuestas de Microsoft sobre SQL Database](https://docs.microsoft.com/answers/topics/azure-sql-database.html). Para efectuar solicitudes de características, agréguelas en el [foro de comentarios sobre SQL Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/elastic-transactions-overview/distributed-transactions.png
