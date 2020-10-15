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
ms.openlocfilehash: e1b70e0e3eb54253972afded1bd37363d1a868e7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84195712"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-sql-database-geo-replication-and-failover"></a>Configuración de Azure-SSIS Integration Runtime con la replicación geográfica y la conmutación por error de SQL Database

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se describe cómo configurar Azure-SSIS Integration Runtime (IR) con la replicación geográfica de Azure SQL Database para la base de datos SSISDB. Cuando se produce una conmutación por error, puede asegurarse de que Azure-SSIS IR sigue funcionando con la base de datos secundaria.

Para más información acerca de la replicación geográfica y la conmutación por error para SQL Database, consulte [Información general: Grupos de conmutación por error automática](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-ssis-ir-failover-with-a-sql-managed-instance"></a>Conmutación por error de Azure-SSIS IR con una instancia administrada de SQL

### <a name="prerequisites"></a>Requisitos previos

La instancia administrada de Azure SQL usa la *clave maestra de base de datos (DMK)* para ayudarle a proteger los datos, las credenciales y la información de conexión almacenada en la base de datos. Para habilitar el descifrado automático de DMK, se cifra una copia de la clave mediante la *clave maestra de servidor (SMK)* . 

SMK no se replica en un grupo de conmutación por error. Debe agregar una contraseña en las instancias principal y secundaria del descifrado de DMK después de realizar la conmutación por error.

1. Ejecute el comando siguiente de SSISDB en la instancia principal. Este paso agrega una nueva contraseña de cifrado.

    ```sql
    ALTER MASTER KEY ADD ENCRYPTION BY PASSWORD = 'password'
    ```

2. Cree un grupo de conmutación por error en una instancia administrada de SQL.

3. Ejecute **sp_control_dbmasterkey_password** en la instancia secundaria, con la nueva contraseña de cifrado.

    ```sql
    EXEC sp_control_dbmasterkey_password @db_name = N'SSISDB',   
        @password = N'<password>', @action = N'add';  
    GO
    ```

### <a name="scenario-1-azure-ssis-ir-is-pointing-to-a-readwrite-listener-endpoint"></a>Escenario 1: Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura.

Si quiere que Azure-SSIS IR apunte al punto de conexión del cliente de escucha de lectura y escritura, primero debe apuntar al punto de conexión del servidor principal. Después de colocar SSISDB en un grupo de conmutación por error, puede cambiar al punto de conexión del agente de escucha de lectura y escritura y reiniciar Azure-SSIS IR.

#### <a name="solution"></a>Solución

Cuando se produzca la conmutación por error, siga estos pasos:

1. Detenga la instancia de Azure-SSIS IR en la región primaria.

2. Edite la instancia de Azure-SSIS IR con la nueva información de la región, la red virtual y el URI de la firma de acceso compartido (SAS) para realizar la instalación personalizada en la instancia secundaria. Como Azure-SSIS IR está señalando al agente de escucha de lectura y escritura y el punto de conexión es transparente para Azure-SSIS IR, no es necesario editar el punto de conexión.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -VNetId "new VNet" `
                -Subnet "new subnet" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Inicie la instancia de Azure-SSIS IR.

### <a name="scenario-2-azure-ssis-ir-is-pointing-to-a-primary-server-endpoint"></a>Escenario 2: Azure-SSIS IR señala al punto de conexión del servidor principal.

Este escenario es adecuado si Azure-SSIS IR apunta al punto de conexión del servidor principal.

#### <a name="solution"></a>Solución

Cuando se produzca la conmutación por error, siga estos pasos:

1. Detenga la instancia de Azure-SSIS IR en la región primaria.

2. Edite Azure-SSIS IR con la información de la nueva región, el punto de conexión y la red virtual de la instancia secundaria.

    ```powershell
      Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database endpoint" `
                    -CatalogAdminCredential "Azure SQL Database admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
        ```

3. Restart the Azure-SSIS IR.

### Scenario 3: Azure-SSIS IR is pointing to a public endpoint of a SQL Managed Instance

This scenario is suitable if the Azure-SSIS IR is pointing to a public endpoint of a Azure SQL Managed Instance and it doesn't join to a virtual network. The only difference from scenario 2 is that you don't need to edit virtual network information for the Azure-SSIS IR after failover.

#### Solution

When failover occurs, take the following steps:

1. Stop the Azure-SSIS IR in the primary region.

2. Edit the Azure-SSIS IR with the new region and endpoint information for the secondary instance.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Inicie la instancia de Azure-SSIS IR.

### <a name="scenario-4-attach-an-existing-ssisdb-instance-ssis-catalog-to-a-new-azure-ssis-ir"></a>Escenario 4: asociación de una SSISDB existente (catálogo de SSIS) a una nueva instancia de Azure-SSIS IR

Este escenario es adecuado si quiere que SSISDB funcione con una nueva instancia de Azure-SSIS IR en una nueva región cuando se produce un desastre en las instancias de Azure Data Factory o Azure-SSIS IR en la región actual.

#### <a name="solution"></a>Solución

Cuando se produzca la conmutación por error, siga estos pasos:

> [!NOTE]
> Use PowerShell para el paso 4 (creación de IR). Si no lo hace, Azure Portal notificará un error que indicará que SSISDB ya existe.

1. Detenga la instancia de Azure-SSIS IR en la región primaria.

2. Ejecute un procedimiento almacenado para actualizar los metadatos de SSISDB para que acepten conexiones de **\<new_data_factory_name\>** y **\<new_integration_runtime_name\>** .
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Cree una factoría de datos nueva denominada **\<new_data_factory_name\>** en la nueva región.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                      -Location "new region"`
                      -Name "<new_data_factory_name>"
    ```
    
    Para obtener más información sobre este comando de PowerShell, consulte [Creación de una factoría de datos de Azure con PowerShell](quickstart-create-data-factory-powershell.md).

4. Cree una nueva instancia de Azure-SSIS IR denominada **\<new_integration_runtime_name\>** en la nueva región mediante Azure PowerShell.

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

    Para obtener más información sobre este comando de PowerShell, consulte [Creación de un entorno de ejecución para la integración de Azure-SSIS en Azure Data Factory](create-azure-ssis-integration-runtime.md).



## <a name="azure-ssis-ir-failover-with-sql-database"></a>Conmutación por error de Azure-SSIS IR con Azure SQL Database

### <a name="scenario-1-azure-ssis-ir-is-pointing-to-a-readwrite-listener-endpoint"></a>Escenario 1: Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura.

Este escenario es adecuado cuando:

- Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura del grupo de conmutación por error.
- El servidor de SQL Database *no* está configurado con la regla de punto de conexión de servicio de red virtual.

Si quiere que Azure-SSIS IR apunte al punto de conexión del cliente de escucha de lectura y escritura, primero debe apuntar al punto de conexión del servidor principal. Después de colocar SSISDB en un grupo de conmutación por error, puede cambiar al punto de conexión del agente de escucha de lectura y escritura y reiniciar Azure-SSIS IR.

#### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, es transparente para Azure-SSIS IR. Azure-SSIS IR se conecta automáticamente a la nueva instancia principal del grupo de conmutación por error. 

Si quiere actualizar la región u otra información en Azure-SSIS IR, puede detenerla, editarla y reiniciarla.


### <a name="scenario-2-azure-ssis-ir-is-pointing-to-a-primary-server-endpoint"></a>Escenario 2: Azure-SSIS IR señala al punto de conexión del servidor principal.

Este escenario es adecuado si Azure-SSIS IR apunta al punto de conexión del servidor principal.

#### <a name="solution"></a>Solución

Cuando se produzca la conmutación por error, siga estos pasos:

1. Detenga la instancia de Azure-SSIS IR en la región primaria.

2. Edite Azure-SSIS IR con la información de la nueva región, el punto de conexión y la red virtual de la instancia secundaria.

    ```powershell
      Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                        -CatalogServerEndpoint "Azure SQL Database endpoint" `
                        -CatalogAdminCredential "Azure SQL Database admin credentials" `
                        -VNetId "new VNet" `
                        -Subnet "new subnet" `
                        -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Inicie la instancia de Azure-SSIS IR.

### <a name="scenario-3-attach-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Escenario 3: asociación de una SSISDB existente (catálogo de SSIS) a una nueva instancia de Azure-SSIS IR

Este escenario es apropiado si quiere aprovisionar una nueva instancia de Azure-SSIS IR en la región secundaria. También es adecuado si quiere que SSISDB funcione con una nueva instancia de Azure-SSIS IR en una nueva región cuando se produce un desastre en las instancias de Azure Data Factory o Azure-SSIS IR en la región actual.

#### <a name="solution"></a>Solución

Cuando se produzca la conmutación por error, siga estos pasos:

> [!NOTE]
> Use PowerShell para el paso 4 (creación de IR). Si no lo hace, Azure Portal notificará un error que indicará que SSISDB ya existe.

1. Detenga la instancia de Azure-SSIS IR en la región primaria.

2. Ejecute un procedimiento almacenado para actualizar los metadatos de SSISDB para que acepten conexiones de **\<new_data_factory_name\>** y **\<new_integration_runtime_name\>** .
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Cree una factoría de datos nueva denominada **\<new_data_factory_name\>** en la nueva región.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
    ```
    
    Para obtener más información sobre este comando de PowerShell, consulte [Creación de una factoría de datos de Azure con PowerShell](quickstart-create-data-factory-powershell.md).

4. Cree una nueva instancia de Azure-SSIS IR denominada **\<new_integration_runtime_name\>** en la nueva región mediante Azure PowerShell.

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

    Para obtener más información sobre este comando de PowerShell, consulte [Creación de un entorno de ejecución para la integración de Azure-SSIS en Azure Data Factory](create-azure-ssis-integration-runtime.md).


## <a name="next-steps"></a>Pasos siguientes

Tenga en cuenta estas otras opciones de configuración para el entorno de ejecución de integración de SSIS en Azure:

- [Configuración de una instancia de Azure-SSIS Integration Runtime para conseguir un alto rendimiento](configure-azure-ssis-integration-runtime-performance.md)

- [Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Aprovisionamiento de Enterprise Edition en la instancia de Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)
