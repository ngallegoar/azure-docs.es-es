---
title: Configuración de Azure-SSIS Integration Runtime para la conmutación por error de SQL Database
description: En este artículo se describe cómo configurar Azure-SSIS Integration Runtime con la replicación geográfica y la conmutación por error de Azure SQL Database para la base de datos SSISDB.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: powershell
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/09/2020
ms.openlocfilehash: b4b679b15092531ff9553d221f23d7f030fc1def
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82592101"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Configurar Azure-SSIS Integration Runtime con la replicación geográfica y la conmutación por error de Azure SQL Database

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se describe cómo configurar Azure-SSIS Integration Runtime con la replicación geográfica de Azure SQL Database para la base de datos SSISDB. Cuando se produce una conmutación por error, puede asegurarse de que Azure-SSIS IR sigue funcionando con la base de datos secundaria.

Para más información acerca de la replicación geográfica y la conmutación por error para SQL Database, consulte [Información general: Grupos de conmutación por error automática](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-ssis-ir-failover-with-azure-sql-database-managed-instance"></a>Conmutación por error de Azure-SSIS IR con Instancia administrada de Azure SQL Database

### <a name="prerequisites"></a>Prerrequisitos

La Instancia administrada de Azure SQL Database usa la **clave maestra de base de datos (DMK)** para ayudar a proteger los datos, las credenciales y la información de conexión almacenada en la base de datos. Para habilitar el descifrado automático de DMK, se cifra una copia de la clave mediante la **clave maestra de servidor (SMK)** . Pero SMK no se replica en el grupo de conmutación por error, por lo que debe agregar una contraseña adicional en las instancias principal y secundaria para el descifrado de DMK después de la conmutación por error.

1. Ejecute el comando siguiente en la SSISDB en la instancia principal. Este paso agrega una nueva contraseña de cifrado.

    ```sql
    ALTER MASTER KEY ADD ENCRYPTION BY PASSWORD = 'password'
    ```

2. Cree un grupo de conmutación por error en una instancia administrada de Azure SQL Database.

3. Ejecute **sp_control_dbmasterkey_password** en la instancia secundaria, con la nueva contraseña de cifrado.

    ```sql
    EXEC sp_control_dbmasterkey_password @db_name = N'SSISDB',   
        @password = N'<password>', @action = N'add';  
    GO
    ```

### <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Escenario 1: Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura.

Si quiere que Azure-SSIS IR apunte al extremo del cliente de escucha de lectura y escritura, primero debe apuntar al punto de conexión del servidor principal. Después de colocar SSISDB en el grupo de conmutación por error, puede cambiar al punto de conexión del agente de escucha de lectura y escritura y reiniciar Azure-SSIS IR.

#### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, deberá hacer lo siguiente:

1. Detenga Azure-SSIS IR en la región primaria.

2. Edite Azure-SSIS IR con la nueva región, red virtual e información de URI de SAS de configuración personalizada de la instancia secundaria. Como Azure-SSIS IR está señalando al agente de escucha de lectura y escritura y el punto de conexión es transparente para Azure-SSIS IR, no es necesario editar el punto de conexión.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -VNetId "new VNet" `
                -Subnet "new subnet" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Reinicie Azure-SSIS IR.

### <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Escenario 2: Azure-SSIS IR señala al punto de conexión del servidor principal.

Este escenario es adecuado si Azure-SSIS IR señala al punto de conexión del servidor principal.

#### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, deberá hacer lo siguiente:

1. Detenga Azure-SSIS IR en la región primaria.

2. Edite Azure-SSIS IR con la nueva región, el punto de conexión y la información de red virtual de la instancia secundaria.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                -VNetId "new VNet" `
                -Subnet "new subnet" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Reinicie Azure-SSIS IR.

### <a name="scenario-3---azure-ssis-ir-is-pointing-to-public-endpoint-of-azure-sql-database-managed-instance"></a>Escenario 3: Azure-SSIS IR apunta al punto de conexión público de la Instancia administrada de Azure SQL Database

El escenario es adecuado si Azure-SSIS IR apunta al punto de conexión público de la Instancia administrada de Azure SQL Database y no se une a la red virtual. La única diferencia entre el escenario 2 y estos escenarios es que no es necesario editar la información de la red virtual de Azure-SSIS IR después de la conmutación por error.

#### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, deberá hacer lo siguiente:

1. Detenga Azure-SSIS IR en la región primaria.

2. Edite Azure-SSIS IR con la nueva información de región y punto de conexión de la instancia secundaria.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Reinicie Azure-SSIS IR.

### <a name="scenario-4---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Escenario 4: asociar una SSISDB existente (catálogo de SSIS) a una nueva instancia de Azure-SSIS IR

Este escenario es adecuado si quiere aprovisionar una nueva instancia de Azure-SSIS IR en una región secundaria o quiere que SSISDB siga trabajando con una nueva instancia de Azure-SSIS IR en una nueva región si se produce un desastre de Azure-SSIS IR o ADF en la región actual.

#### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, deberá hacer lo siguiente:

> [!NOTE]
> El paso 4 (creación de IR) debe realizarse mediante PowerShell. Azure Portal notificará un error que indicará que SSISDB ya existe.

1. Detenga Azure-SSIS IR en la región primaria.

2. Ejecute el procedimiento almacenado para actualizar los metadatos en SSISDB a fin de aceptar conexiones de **\<new_data_factory_name\>** y **\<new_integration_runtime_name\>** .
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Cree una factoría de datos nueva denominada **\<new_data_factory_name\>** en la nueva región. Para más información, consulte Creación de una factoría de datos.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                      -Location "new region"`
                      -Name "<new_data_factory_name>"
    ```
    
    Para más información sobre este comando de PowerShell, consulte [Inicio rápido: Creación de una factoría de datos de Azure con PowerShell](quickstart-create-data-factory-powershell.md)

4. Cree un nuevo Azure-SSIS IR denominado **\<new_integration_runtime_name\>** en la nueva región con Azure PowerShell.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
                                           -DataFactoryName "new data factory name" `
                                           -Name "<new_integration_runtime_name>" `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId "new vnet" `
                                           -Subnet "new subnet" `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier
    ```

    Para más información sobre este comando de PowerShell, vea [Creación de un entorno de ejecución para la integración de SSIS en Azure en Azure Data Factory](create-azure-ssis-integration-runtime.md)



## <a name="azure-ssis-ir-failover-with-azure-sql-database"></a>Conmutación por error de Azure-SSIS IR con Azure SQL Database

### <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Escenario 1: Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura.

Este escenario es adecuado si Azure-SSIS IR está señalando al punto de conexión del agente de escucha de lectura y escritura del grupo de conmutación por error y el servidor de SQL Database *no* está configurado con la regla de punto de conexión de servicio de red virtual. Si quiere que Azure-SSIS IR apunte al punto de conexión del cliente de escucha de lectura y escritura, primero debe apuntar al punto de conexión del servidor principal. Después de colocar SSISDB en el grupo de conmutación por error, puede cambiar al punto de conexión del agente de escucha de lectura y escritura y reiniciar Azure-SSIS IR.

#### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, es transparente para Azure-SSIS IR. Azure-SSIS IR se conecta automáticamente a la nueva instancia principal del grupo de conmutación por error. Si quiere actualizar la región u otra información en Azure-SSIS IR, puede detenerla, editarla y reiniciarla.


### <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Escenario 2: Azure-SSIS IR señala al punto de conexión del servidor principal.

Este escenario es adecuado si Azure-SSIS IR apunta al punto de conexión del servidor principal.

#### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, deberá hacer lo siguiente:

1. Detenga Azure-SSIS IR en la región primaria.

2. Edite Azure-SSIS IR con la nueva región, el punto de conexión y la información de VNET de la instancia secundaria.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Reinicie Azure-SSIS IR.

### <a name="scenario-3---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Escenario 3: asociar una SSISDB existente (catálogo de SSIS) a una nueva integración de Azure-SSIS IR

Este escenario es adecuado si quiere aprovisionar una nueva instancia de Azure-SSIS IR en una región secundaria o quiere que SSISDB siga trabajando con una nueva instancia de Azure-SSIS IR en una nueva región si se produce un desastre de Azure-SSIS IR o ADF en la región actual.

#### <a name="solution"></a>Solución

> [!NOTE]
> El paso 4 (creación de IR) debe realizarse mediante PowerShell. Azure Portal notificará un error que indicará que SSISDB ya existe.

1. Detenga Azure-SSIS IR en la región primaria.

2. Ejecute el procedimiento almacenado para actualizar los metadatos en SSISDB a fin de aceptar conexiones de **\<new_data_factory_name\>** y **\<new_integration_runtime_name\>** .
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Cree una factoría de datos nueva denominada **\<new_data_factory_name\>** en la nueva región. Para más información, consulte Creación de una factoría de datos.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
    ```
    
    Para más información sobre este comando de PowerShell, consulte [Inicio rápido: Creación de una factoría de datos de Azure con PowerShell](quickstart-create-data-factory-powershell.md)

4. Cree un nuevo Azure-SSIS IR denominado **\<new_integration_runtime_name\>** en la nueva región con Azure PowerShell.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
                                           -DataFactoryName "new data factory name" `
                                           -Name "<new_integration_runtime_name>" `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId "new vnet" `
                                           -Subnet "new subnet" `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier
    ```

    Para más información sobre este comando de PowerShell, vea [Creación de un entorno de ejecución para la integración de SSIS en Azure en Azure Data Factory](create-azure-ssis-integration-runtime.md)


## <a name="next-steps"></a>Pasos siguientes

Tenga en cuenta estas otras opciones de configuración para el entorno de ejecución de integración de SSIS en Azure:

- [Configuración de una instancia de Integration Runtime para la integración de SSIS en Azure para conseguir un alto rendimiento](configure-azure-ssis-integration-runtime-performance.md)

- [Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Aprovisionamiento de Enterprise Edition en la instancia de Integration Runtime para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-enterprise-edition.md)
