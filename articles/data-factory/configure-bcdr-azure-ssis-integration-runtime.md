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
ms.date: 08/14/2018
ms.openlocfilehash: 92f7d25a9c19409b220b6a71fba87da91e51a415
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74928499"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Configurar Azure-SSIS Integration Runtime con la replicación geográfica y la conmutación por error de Azure SQL Database

En este artículo se describe cómo configurar Azure-SSIS Integration Runtime con la replicación geográfica de Azure SQL Database para la base de datos SSISDB. Cuando se produce una conmutación por error, puede asegurarse de que Azure-SSIS IR sigue funcionando con la base de datos secundaria.

Para obtener más información acerca de la replicación geográfica y la conmutación por error para SQL Database, consulte [Información general: grupos de conmutación por error automática y replicación geográfica activa](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Escenario 1: Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura.

### <a name="conditions"></a>Condiciones

Esta sección se aplica cuando se cumplen las condiciones siguientes:

- Azure-SSIS IR señala al punto de conexión del agente de escucha de lectura y escritura del grupo de conmutación por error.

  y

- El servidor de SQL Database *no* está configurado con la regla de punto de conexión de servicio de red virtual.

### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, es transparente para Azure-SSIS IR. Azure-SSIS IR se conecta automáticamente a la nueva instancia principal del grupo de conmutación por error.

## <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Escenario 2: Azure-SSIS IR señala al punto de conexión del servidor principal.

### <a name="conditions"></a>Condiciones

Esta sección se aplica cuando se cumple una de las condiciones siguientes:

- Azure-SSIS IR señala al punto de conexión del servidor principal del grupo de conmutación por error. Este punto de conexión cambia cuando se produce una conmutación por error.

  O BIEN

- El servidor de Azure SQL Database está configurado con la regla de punto de conexión de servicio de red virtual.

  O BIEN

- El servidor de base de datos es una Instancia administrada SQL Database configurada con una red virtual.

### <a name="solution"></a>Solución

Cuando se produce una conmutación por error, deberá hacer lo siguiente:

1. Detenga la instancia de Integration Runtime de SSIS de Azure.

2. Vuelva a configurar IR para que señale al nuevo punto de conexión principal y a una red virtual en la nueva región.

3. Reinicie IR.

En las siguientes secciones se describen estos pasos con más detalle.

### <a name="prerequisites"></a>Prerequisites

- Asegúrese de que ha habilitado la recuperación ante desastres para el servidor de Azure SQL Database en caso de que el servidor tenga una interrupción del servicio al mismo tiempo. Para más información, vea [Introducción a la continuidad empresarial con Azure SQL Database](../sql-database/sql-database-business-continuity.md).

- Si usa una red virtual en la región actual, tiene que usar otra red virtual en la nueva región para conectarse a un entorno de ejecución para la integración de SSIS en Azure. Para más información, vea [Unión de un entorno de ejecución para la integración de SSIS en Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

- Si utiliza una instalación personalizada, puede que tenga que preparar otro URI de SAS para el contenedor de blobs que almacena el script de instalación personalizada y los archivos asociados, de modo que siga siendo accesible durante una interrupción. Para más información, vea [Configure a custom setup on an Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Configuración de una instalación personalizada en un entorno de ejecución para la integración de SSIS en Azure).

### <a name="steps"></a>Pasos

Siga estos pasos para detener el entorno de ejecución de integración de SSIS en Azure, cambie el entorno de ejecución de integración a otra región y vuelva a iniciarlo.

1. Detenga la instancia del entorno de ejecución de integración en la región original.

2. Llame al comando siguiente en PowerShell para actualizar IR con la nueva configuración.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Para más información sobre este comando de PowerShell, vea [Creación de un entorno de ejecución para la integración de SSIS en Azure en Azure Data Factory](create-azure-ssis-integration-runtime.md)

3. Vuelva a iniciar el entorno de ejecución de integración.

## <a name="scenario-3---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Escenario 3: asociar una SSISDB existente (catálogo de SSIS) a una nueva integración de Azure-SSIS IR

Cuando se produce un desastre de Azure-SSIS IR o de ADF en la región actual, puede hacer que su SSISDB siga trabajando con un nuevo Azure-SSIS IR en una región nueva.

### <a name="prerequisites"></a>Prerequisites

- Si usa una red virtual en la región actual, tiene que usar otra red virtual en la nueva región para conectarse a un entorno de ejecución para la integración de SSIS en Azure. Para más información, vea [Unión de un entorno de ejecución para la integración de SSIS en Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md).

- Si utiliza una instalación personalizada, puede que tenga que preparar otro URI de SAS para el contenedor de blobs que almacena el script de instalación personalizada y los archivos asociados, de modo que siga siendo accesible durante una interrupción. Para más información, vea [Configure a custom setup on an Azure-SSIS integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md) (Configuración de una instalación personalizada en un entorno de ejecución para la integración de SSIS en Azure).

### <a name="steps"></a>Pasos

Siga estos pasos para detener el entorno de ejecución de integración de SSIS en Azure, cambie el entorno de ejecución de integración a otra región y vuelva a iniciarlo.

1. Ejecute el procedimiento almacenado para hacer que SSISDB se adjunte a **\<new_data_factory_name\>** o **\<new_integration_runtime_name\>** .
   
  ```SQL
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
   ```

2. Cree una factoría de datos nueva denominada **\<new_data_factory_name\>** en la nueva región. Para más información, consulte Creación de una factoría de datos.

     ```powershell
     Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
     ```
    Para más información sobre este comando de PowerShell, consulte [Inicio rápido: Creación de una factoría de datos de Azure con PowerShell](quickstart-create-data-factory-powershell.md)

3. Cree un nuevo Azure-SSIS IR denominado **\<new_integration_runtime_name\>** en la nueva región con Azure PowerShell.

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

4. Vuelva a iniciar el entorno de ejecución de integración.

## <a name="next-steps"></a>Pasos siguientes

Tenga en cuenta estas otras opciones de configuración para el entorno de ejecución de integración de SSIS en Azure:

- [Configuración de una instancia de Integration Runtime para la integración de SSIS en Azure para conseguir un alto rendimiento](configure-azure-ssis-integration-runtime-performance.md)

- [Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Aprovisionamiento de Enterprise Edition en la instancia de Integration Runtime para la integración de SSIS en Azure](how-to-configure-azure-ssis-ir-enterprise-edition.md)
