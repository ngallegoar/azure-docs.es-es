---
title: Cifrado de datos transparente
description: Información general sobre el cifrado de datos transparente para SQL Database y SQL Analytics en Azure Synapse. Este documento explica sus ventajas y las opciones de configuración, que incluye cifrado de datos transparente administrado por el servicio y Bring Your Own Key.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and Azure Synapse
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 02/06/2020
ms.openlocfilehash: 5bbb537ef6545852423bf5315b7636671c598fdc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228444"
---
# <a name="transparent-data-encryption-for-sql-database-and-azure-synapse"></a>Cifrado de datos transparente para SQL Database y Azure Synapse

El cifrado de datos transparente (TDE) ayuda a proteger Azure SQL Database, Instancia administrada de Azure SQL y Azure Synapse frente a la amenaza de actividad malintencionada sin conexión, ya que cifra los datos en reposo. También realiza cifrado y descifrado de la base de datos en tiempo real, copias de seguridad asociadas y archivos de registro de transacciones en reposo sin necesidad de efectuar cambios en la aplicación. De forma predeterminada, TDE está habilitado para todas las bases de datos de Azure SQL recién implementadas. TDE no se puede usar para cifrar la base de datos **maestra** lógica en SQL Database.  La base de datos **maestra** contiene objetos que son necesarios para realizar las operaciones de TDE en las bases de datos de usuario.

TDE debe habilitarse manualmente para las bases de datos anteriores de Azure SQL Database, Instancia administrada de Azure SQL o Azure Synapse.
Las bases de datos de Instancia administrada creadas mediante restauración heredan el estado de cifrado de la base de datos de origen.

El cifrado de datos transparente se encarga de cifrar el almacenamiento de toda una base de datos mediante una clave simétrica denominada clave de cifrado de base de datos. Esta clave de cifrado de base de datos está protegida por el protector de cifrado de datos transparente. El protector es un certificado administrado por el servicio (cifrado de datos transparentes administrado por el servicio) o una clave asimétrica almacenada en Azure Key Vault (Bring Your Own Key). Establezca el protector de cifrado de datos transparente en el nivel de servidor para Azure SQL Database y Azure Synapse, y en el nivel de instancia para Instancia administrada de Azure SQL. El término *servidor* hace referencia tanto a servidor como a instancia a lo largo de este documento, a menos que se indique lo contrario.

Durante el inicio de la base de datos, la clave de cifrado de base de datos cifrada se descifra y, a continuación, se usa para descifrar y volver a cifrar los archivos de base de datos en el proceso del motor de base de datos de SQL Server. El cifrado de datos transparente realiza el cifrado de E/S en tiempo real y el descifrado de los datos en el nivel de página. Todas las páginas se descifran cuando se leen en la memoria y, a continuación, se cifran antes de escribirse en el disco. Para consultar una descripción general del cifrado de datos transparente, consulte [Cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

Las instancias de SQL Server que se ejecutan en una máquina virtual de Azure también pueden usar una clave asimétrica desde Key Vault. Los pasos de configuración son diferentes del uso de una clave asimétrica en SQL Database y en la Instancia administrada de SQL Database. Para obtener más información, consulte [Administración extensible de claves con Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Cifrado de datos transparente administrado por el servicio

En Azure, la configuración predeterminada para el cifrado de datos transparente es aquella por la que la clave de cifrado de base de datos está protegida por un certificado de servidor integrado. El certificado de servidor integrado es único para cada servidor y el algoritmo de cifrado que se usa es AES 256. Si una base de datos está en una relación de replicación geográfica, la base de datos principal y secundaria con replicación geográfica están protegidas por la clave de servidor principal de la base de datos principal. Si hay dos bases de datos conectadas al mismo servidor, también comparten el mismo certificado integrado.  Microsoft rota automáticamente estos certificados en cumplimiento de la directiva de seguridad interna y se protege la clave raíz mediante un almacén secreto interno de Microsoft.  Los clientes pueden verificar el cumplimiento de SQL Database con las directivas de seguridad internas en los informes de auditoría de terceros independientes disponibles en el [Centro de confianza de Microsoft](https://servicetrust.microsoft.com/).

Microsoft también mueve y administra con total fluidez las claves según sea necesario para la replicación geográfica y las restauraciones.

> [!IMPORTANT]
> Todas las bases de datos SQL y bases de datos de Instancia administrada recién creadas se cifran de forma predeterminada usando el cifrado de datos transparente administrado por el servicio. Las bases de datos SQL existentes creadas antes de mayo de 2017 y las bases de datos SQL creadas mediante restauración, replicación geográfica y copia de base de datos no se cifran de forma predeterminada. Las bases de datos de Instancia administrada existentes creadas antes de febrero de 2019 no se cifran de forma predeterminada. Las bases de datos de Instancia administrada creadas mediante restauración heredan el estado de cifrado del origen.

## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Cifrado de datos transparente administrado por el cliente (Bring Your Own Key)

El [cifrado de datos transparente con claves administradas por el cliente en Azure Key Vault](transparent-data-encryption-byok-azure-sql.md) permite cifrar la clave de cifrado de base de datos (DEK) con una clave asimétrica administrada por el cliente denominada protector de TDE.  Esto también se conoce como compatibilidad de Bring Your Own Key (BYOK) para Cifrado de datos transparente. En el escenario BYOK, el protector de TDE se almacena en una instancia de [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault) administrada por el cliente, y de la que este es propietario; esta instancia es el sistema de administración de claves externas en la nube de Azure. La [instancia de Key Vault puede generar el protector TDE o dicho protector puede transferirse a Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys) desde un dispositivo HSM local. El protector de TDE, que se almacena en Azure Key Vault y nunca sale del almacén de claves, cifra y descifra la clave de cifrado de TDE, que se almacena en la página de inicio de una base de datos.  Para cifrar y descifrar la clave de cifrado es preciso que SQL Database tenga los permisos necesarios en el almacén de claves que posee el cliente. Si se revocan los permisos del servidor con SQL Server lógico en el almacén de claves, no se podrá obtener acceso a la base de datos y se cifrarán todos los datos. En el caso de Azure SQL Database, el protector de TDE se establece en el nivel de servidor con SQL Server lógico y lo heredan todas las bases de datos asociadas a dicho servidor. En el caso de la [Instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-howto-managed-instance), el protector de TDE se establece en el nivel de instancia y lo heredan todas las bases de datos *cifradas* que se encuentren en dicha instancia. El término *servidor* hace referencia tanto a servidor como a instancia a lo largo de este documento, a menos que se indique lo contrario.

Gracias al cifrado de datos transparente con integración de Azure Key Vault, los usuarios pueden controlar las tareas de administración de claves, entre las que se incluyen las rotaciones de claves, los permisos del almacén de claves y la copia de seguridad de claves, así como la opción de llevar a cabo auditorías o crear informes sobre todos los protectores de TDE mediante la funcionalidad de Azure Key Vault. Key Vault ofrece una administración centralizada de claves, aprovecha los módulos de seguridad de hardware (HSM) extremadamente supervisados y permite la separación de obligaciones entre las claves y los datos de administración, lo que ayudar a cumplir las directivas de seguridad.
Para obtener más información sobre el cifrado de datos transparente con integración de Azure Key Vault (compatibilidad con Bring Your Own Key) para Azure SQL Database, Instancia administrada de SQL y Azure Synapse, vea [Cifrado de datos transparente con integración de Azure Key Vault](transparent-data-encryption-byok-azure-sql.md).

Para empezar a usar el cifrado de datos transparente con la integración de Azure Key Vault (compatibilidad con Bring Your Own Key), consulte la guía paso a paso [Turn on transparent data encryption by using your own key from Key Vault by using PowerShell](transparent-data-encryption-byok-azure-sql-configure.md) (Activación del cifrado de datos transparente con una clave propia desde Key Vault mediante PowerShell).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Traslado de una base de datos protegida por cifrado de datos transparente

No es necesario descifrar las bases de datos para las operaciones dentro de Azure. La configuración del cifrado de datos transparente en la base de datos de origen o la base de datos principal se hereda de forma transparente en el destino. Entre las operaciones incluidas están las siguientes:

- Geo-restore
- Restauración a un momento específico de autoservicio
- Restauración de una base de datos eliminada
- Replicación geográfica activa
- Creación de una copia de base de datos
- Restauración de archivo de copia de seguridad a la Instancia administrada de Azure SQL Database

> [!IMPORTANT]
> En Instancia administrada de Azure SQL Database no se permite realizar copias de seguridad manuales de una base de datos cifrada por TDE administrada por el servicio, ya que el certificado utilizado para el cifrado no es accesible. Utilice la característica de restauración de punto en el tiempo para mover este tipo de base de datos a otra Instancia administrada.

Al exportar una base de datos protegida por el cifrado de datos transparente, no se cifra el contenido exportado de la base de datos. Este contenido exportado se almacena en archivos BACPAC no cifrados. Asegúrese de proteger adecuadamente los archivos BACPAC y habilitar el cifrado de datos transparente una vez finalizada la importación de la nueva base de datos.

Por ejemplo, si el archivo BACPAC se exporta desde una instancia de SQL Server local, el contenido importado de la nueva base de datos no se cifra automáticamente. Del mismo modo, si el archivo BACPAC se exporta a una instancia de SQL Server local, la base de datos nueva tampoco se cifra automáticamente.

La única excepción es cuando se exporta a y desde una base de datos SQL. El cifrado de datos transparente está habilitado en la nueva base de datos, pero todavía no se cifra el propio archivo BACPAC.


## <a name="manage-transparent-data-encryption"></a>Administración del cifrado de datos transparente
# <a name="portal"></a>[Portal](#tab/azure-portal)
Administre el cifrado de datos transparente en Azure Portal.

Para configurar el cifrado de datos transparente mediante Azure Portal, debe estar conectado como propietario o colaborador de Azure o administrador de seguridad SQL.

Active o desactive el cifrado de datos transparente en el nivel de base de datos. Para habilitar el cifrado de datos transparente en una base de datos, vaya a [Azure Portal](https://portal.azure.com) e inicie sesión con su cuenta de administrador o colaborador de Azure. Busque la configuración de cifrado de datos transparente en la base de datos de usuario. De forma predeterminada, se usa el cifrado de datos transparente administrado por el servicio. Se genera automáticamente un certificado de cifrado de datos transparente para el servidor que contiene la base de datos. Para la Instancia administrada de Azure SQL Database, use T-SQL para activar y desactivar el cifrado de datos transparente en una base de datos.

![Cifrado de datos transparente administrado por el servicio](./media/transparent-data-encryption-azure-sql/service-managed-transparent-data-encryption.png)

Establezca la clave maestra del cifrado de datos transparente, también conocida como protector de cifrado de datos transparente, en el nivel de servidor. Para usar el cifrado de datos transparente con compatibilidad con Bring Your Own Key y proteger las bases de datos con una clave de Key Vault, abra la configuración del cifrado de datos transparente en el servidor.

![Cifrado de datos transparente con compatibilidad con Bring Your Own Key](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Administre el cifrado de datos transparente mediante PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos.

Para configurar el cifrado de datos transparente mediante PowerShell, debe estar conectado como propietario o colaborador de Azure o administrador de seguridad SQL.

### <a name="cmdlets-for-azure-sql-database-and-azure-synapse"></a>Cmdlets para Azure SQL Database y Azure Synapse

Use los siguientes cmdlets para Azure SQL Database y Azure Synapse:

| Cmdlet | Descripción |
| --- | --- |
| [Set-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) |Habilita o deshabilita el cifrado de datos transparente para una base de datos|
| [Get-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) |Obtiene el estado del cifrado de datos transparente para una base de datos |
| [Get-AzSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) |Comprueba el progreso del cifrado de una base de datos |
| [Add-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/add-azsqlserverkeyvaultkey) |Agrega una clave de Key Vault a una instancia de SQL Server |
| [Get-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverkeyvaultkey) |Obtiene las claves de Key Vault para un servidor de Azure SQL Database  |
| [Set-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) |Establece el protector de cifrado de datos transparente para una instancia de SQL Server |
| [Get-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) |Obtiene el protector de cifrado de datos transparente |
| [Remove-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) |Quita una clave de Key Vault de una instancia de SQL Server |
|  | |

> [!IMPORTANT]
> Para la Instancia administrada de SQL Database, use el comando T-SQL [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) para activar y desactivar el cifrado de datos transparente a nivel de base de datos y consulte el [script de PowerShell de ejemplo](transparent-data-encryption-byok-azure-sql-configure.md) para administrar el cifrado de datos transparente a nivel de instancia.

# <a name="transact-sql"></a>[Transact-SQL](#tab/azure-TransactSQL)
Administre el cifrado de datos transparente mediante Transact-SQL.

Conéctese a la base de datos mediante un inicio de sesión que sea un administrador o un miembro del rol **dbmanager** en la base de datos maestra.

| Get-Help | Descripción |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | SET ENCRYPTION ON/OFF cifra o descifra una base de datos |
| [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Devuelve información sobre el estado de cifrado de una base de datos y sus claves de cifrado de base de datos asociadas |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Devuelve información sobre el estado de cifrado de cada nodo de almacenamiento de datos y sus claves de cifrado de base de datos asociadas |
|  | |

No puede cambiar el protector de cifrado de datos transparente para una clave de Key Vault mediante Transact-SQL. Use PowerShell o Azure Portal.

# <a name="rest-api"></a>[REST API](#tab/azure-RESTAPI)
Administre el cifrado de datos transparente mediante la API REST.

Para configurar el cifrado de datos transparente mediante la API REST, debe estar conectado como el propietario o colaborador de Azure o administrador de seguridad SQL.
Use el siguiente conjunto de comandos para Azure SQL Database y Azure Synapse:

| Get-Help | Descripción |
| --- | --- |
|[Create Or Update Server](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Agrega una identidad de Azure Active Directory a una instancia de SQL Server (que se usa para conceder acceso a Key Vault)|
|[Create Or Update Server Key](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Agrega una clave de Key Vault a una instancia de SQL Server|
|[Delete Server Key](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Quita una clave de Key Vault de una instancia de SQL Server|
|[Get Server Keys](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Obtiene una clave específica de Key Vault de una instancia de SQL Server|
|[List Server Keys By Server](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|Obtiene las claves de Key Vault para una instancia de SQL Server |
|[Create Or Update Encryption Protector](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Establece el protector de cifrado de datos transparente para una instancia de SQL Server|
|[Get Encryption Protector](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Obtiene el protector de cifrado de datos transparente para una instancia de SQL Server|
|[List Encryption Protectors By Server](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Obtiene los protectores de cifrado de datos transparente para una instancia de SQL Server |
|[Create Or Update Transparent Data Encryption Configuration](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Habilita o deshabilita el cifrado de datos transparente para una base de datos|
|[Get Transparent Data Encryption Configuration](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Obtiene la configuración de cifrado de datos transparente para una base de datos|
|[List Transparent Data Encryption Configuration Results](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Obtiene el resultado de cifrado para una base de datos|

## <a name="next-steps"></a>Pasos siguientes

- Para consultar una descripción general del cifrado de datos transparente, consulte [Cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Para obtener más información sobre el cifrado de datos transparente con compatibilidad con Bring Your Own Key para Azure SQL Database, Instancia administrada de Azure SQL y Azure Synapse, vea [Cifrado de datos transparente con compatibilidad con Bring Your Own Key](transparent-data-encryption-byok-azure-sql.md).
- Para empezar a usar el cifrado de datos transparente con compatibilidad con Bring Your Own Key, consulte la guía de procedimientos [Turn on transparent data encryption by using your own key from Key Vault by using PowerShell](transparent-data-encryption-byok-azure-sql-configure.md) (Activación del cifrado de datos transparente con su propia clave desde Key Vault mediante PowerShell).
- Para obtener más información acerca de Key Vault, consulte la [página de documentación de Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
