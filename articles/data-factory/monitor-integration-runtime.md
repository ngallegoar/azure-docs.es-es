---
title: Supervisión de Integration Runtime en Azure Data Factory
description: Aprenda a supervisar los distintos tipos de instancia de Integration Runtime en Azure Data Factory.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/25/2018
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 6d2ea5c0b7354867086fc0cce43732f2d73c53ab
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81398961"
---
# <a name="monitor-an-integration-runtime-in-azure-data-factory"></a>Supervisión de Integration Runtime en Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]
  
**Integration Runtime** es la infraestructura de proceso que usa Azure Data Factory para proporcionar varias funcionalidades de integración de datos en distintos entornos de red. Data Factory ofrece tres tipos de instancia de Integration Runtime:

- Tiempo de ejecución de integración de Azure
- Integration Runtime autohospedado
- Integration Runtime de SSIS de Azure

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para obtener el estado de una instancia de Integration Runtime (IR), ejecute el siguiente comando de PowerShell: 

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName MyDataFactory -ResourceGroupName MyResourceGroup -Name MyAzureIR -Status
``` 

El cmdlet devuelve información diferente para distintos tipos de instancia de Integration Runtime. En este artículo se explican las propiedades y los estados de cada tipo de instancia de Integration Runtime.  

## <a name="azure-integration-runtime"></a>Tiempo de ejecución de integración de Azure
El recurso de proceso para una instancia de Azure Integration Runtime se puede administrar completamente de manera elástica en Azure. En la tabla siguiente se proporcionan las descripciones de las propiedades que devuelve el comando **Get-AzDataFactoryV2IntegrationRuntime**:

### <a name="properties"></a>Propiedades
En la siguiente tabla se proporcionan las descripciones de las propiedades que devuelve el cmdlet para una instancia de Azure Integration Runtime:

| Propiedad | Descripción |
-------- | ------------- | 
| Nombre | Nombre de la instancia de Azure Integration Runtime. |  
| State | Estado de la instancia de Azure Integration Runtime. | 
| Location | Ubicación de la instancia de Azure Integration Runtime. Para más información sobre la ubicación de una instancia de Azure Integration Runtime, consulte la [introducción a Integration Runtime](concepts-integration-runtime.md). |
| DataFactoryName | Nombre de la instancia de Data Factory a la que pertenece la instancia de Azure Integration Runtime. | 
| ResourceGroupName | Nombre del grupo de recursos al que pertenece la instancia de Data Factory.  |
| Descripción | Descripción de la instancia de Integration Runtime.  |

### <a name="status"></a>Status
En la tabla siguiente se proporcionan los estados posibles de una instancia de Azure Integration Runtime:

| Status | Comentarios/Escenarios | 
| ------ | ------------------ |
| En línea | La instancia de Azure Integration Runtime está en línea y lista para su uso. | 
| Sin conexión | La instancia de Azure Integration Runtime está desconectada por un error interno. |

## <a name="self-hosted-integration-runtime"></a>Integration Runtime autohospedado
En la sección siguiente se proporcionan las descripciones de las propiedades que devuelve el cmdlet Get-AzDataFactoryV2IntegrationRuntime. 

> [!NOTE] 
> Las propiedades y estado devueltos contienen información sobre la instancia de Integration Runtime autohospedada y de los nodos en tiempo de ejecución.  

### <a name="properties"></a>Propiedades

En la tabla siguiente se proporcionan las descripciones de las propiedades de supervisión para **cada nodo**:

| Propiedad | Descripción | 
| -------- | ----------- | 
| Nombre | Nombre de la instancia de Integration Runtime autohospedada y sus nodos asociados. Un nodo es una máquina Windows local con la instancia de Integration Runtime autohospedada instalada. |  
| Status | Estado de la instancia general de Integration Runtime autohospedada y de los nodos. Ejemplo: En línea, Sin conexión, Limitado, etc. Para información acerca de estos estados, consulte la siguiente sección. | 
| Versión | Versión de la instancia de Integration Runtime autohospedada y de los nodos. La versión de la instancia de Integration Runtime autohospedada se determina en función de la versión de la mayoría de los nodos del grupo. Si hay nodos con versiones diferentes en la configuración de la instancia de Integration Runtime autohospedada, solo los nodos con el mismo número de versión que la instancia lógica de Integration Runtime autohospedada funcionan correctamente. Los otros están en el modo limitado y deben actualizarse manualmente (solo en caso de que se produzca un error en la actualización automática). | 
| Memoria disponible | Memoria disponible en un nodo de una instancia de Integration Runtime autohospedada. Este valor es una instantánea casi en tiempo real. | 
| Uso de CPU | Uso de CPU de un nodo de una instancia de Integration Runtime autohospedada. Este valor es una instantánea casi en tiempo real. |
| Redes (Entrada/Salida) | Uso de las redes de un nodo de una instancia de Integration Runtime autohospedada. Este valor es una instantánea casi en tiempo real. | 
| Trabajos simultáneos (En ejecución / Límite) | **En ejecución**. Número de trabajos o tareas que se ejecutan en cada nodo. Este valor es una instantánea casi en tiempo real. <br/><br/>**Límite**. Límite significa el máximo número de trabajos simultáneos para cada nodo. Este valor se define basándose en el tamaño de la máquina. Puede aumentar el límite para escalar verticalmente la ejecución de trabajos simultáneos en escenarios avanzados, cuando se supera el tiempo de espera de las actividades, aunque la CPU, la memoria o la red estén infrautilizadas. Esta funcionalidad también está disponible con una instancia de Integration Runtime autohospedada de nodo único. |
| Role | En una instancia multinodo de Integration Runtime autohospedada hay dos tipos de roles: distribuidor y trabajo. Todos los nodos son trabajos, lo que significa que pueden usarse para ejecutar los trabajos. Hay solo un nodo distribuidor, que se usa para extraer los trabajos y las tareas de Cloud Services y enviarlos a los diferentes nodos de trabajo. El nodo distribuidor también es un nodo de trabajo. |

Algunos valores de las propiedades tienen más sentido cuando hay dos o más nodos en el entorno de ejecución de integración autohospedado (es decir, en un escenario de escalado horizontal).

#### <a name="concurrent-jobs-limit"></a>Límite de trabajos simultáneos

El valor predeterminado del límite de trabajos simultáneos se establece en función del tamaño de la máquina. Los factores utilizados para calcular este valor dependen de la cantidad de RAM y del número de núcleos de CPU del equipo. Por tanto, cuantos más núcleos y más memoria, mayor será el limite predeterminado de trabajos simultáneos.

Realice un escalado horizontal aumentando el número de nodos. Al aumentar el número de nodos, el límite de trabajos simultáneos es la suma de los valores límite de trabajos simultáneos de todos los nodos disponibles.  Por ejemplo, si un nodo le permite ejecutar un máximo de doce trabajos simultáneos, la incorporación de tres nodos más similares le permite ejecutar un máximo de 48 trabajos simultáneos (es decir, 4 x 12). Se recomienda aumentar el límite de trabajos simultáneos solo cuando vea un uso escaso de los recursos con los valores predeterminados en cada nodo.

El valor predeterminado calculado se puede invalidar en Azure Portal. Seleccione Autor > Conexiones > Runtimes de integración > Editar > Nodos > Modificar valor de trabajos simultáneos por nodo. También puede usar el comando [update-Azdatafactoryv2integrationruntimenode](https://docs.microsoft.com/powershell/module/az.datafactory/update-Azdatafactoryv2integrationruntimenode#examples) de PowerShell.
  
### <a name="status-per-node"></a>Estado (por nodo)
En la tabla siguiente se proporcionan los estados posibles de los nodos de una instancia de Integration Runtime autohospedada:

| Status | Descripción |
| ------ | ------------------ | 
| En línea | Nodo conectado al servicio Data Factory. |
| Sin conexión | El nodo está sin conexión. |
| Actualizando | El nodo se está actualizando automáticamente. |
| Limitado | Debido a un problema de conectividad. Puede ser debido a un problema en el puerto HTTP 8050, a un problema de conectividad de Service Bus o a un problema de sincronización de credenciales. |
| Inactivo | El nodo tiene una configuración diferente de la configuración de la mayoría de los otros nodos. |

Un nodo puede estar inactivo cuando no se puede conectar a otros nodos.

### <a name="status-overall-self-hosted-integration-runtime"></a>Estado (instancia general de Integration Runtime autohospedada)
En la tabla siguiente se proporcionan los estados posibles de una instancia de Integration Runtime autohospedada. Este estado depende de los de todos los nodos que pertenecen a la instancia de Runtime. 

| Status | Descripción |
| ------ | ----------- | 
| Need registration (Se necesita registro) | No hay nodos registrados para esta instancia de Integration Runtime autohospedada aún. |
| En línea | Todos los nodos están en línea. |
| Sin conexión | Ningún nodo está en línea. |
| Limitado | No todos los nodos de esta instancia de Integration Runtime autohospedada tienen un estado correcto. Este estado es una advertencia de que alguno de los nodos podría estar inactivo. Puede deberse a un problema de sincronización de credenciales en el nodo distribuidor o de trabajo. |

Use el cmdlet **Get-AzDataFactoryV2IntegrationRuntimeMetric** para capturar la carga de JSON que contiene las propiedades detalladas del entorno de ejecución de integración autohospedado y sus valores de instantánea durante el tiempo de ejecución del cmdlet.

```powershell
Get-AzDataFactoryV2IntegrationRuntimeMetric -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName | ConvertTo-Json 
```

Salida de ejemplo (se supone que hay dos nodos asociados a esta instancia de Integration Runtime autohospedada):

```json
{
    "IntegrationRuntimeName":  "<Name of your integration runtime>",
    "ResourceGroupName":  "<Resource Group Name>",
    "DataFactoryName":  "<Data Factory Name>",
    "Nodes":  [
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        },
        {
            "NodeName":  "<Node Name>",
            "AvailableMemoryInMB":  <Value>,
            "CpuUtilization":  <Value>,
            "ConcurrentJobsLimit":  <Value>,
            "ConcurrentJobsRunning":  <Value>,
            "MaxConcurrentJobs":  <Value>,
            "SentBytes":  <Value>,
            "ReceivedBytes":  <Value>
        }

    ]
} 
```


## <a name="azure-ssis-integration-runtime"></a>Integration Runtime de SSIS de Azure
Integration Runtime de SSIS de Azure es un clúster totalmente administrado de máquinas virtuales (o nodos) de Azure que se dedica a ejecutar los paquetes SSIS. No ejecuta otras actividades de Azure Data Factory. Una vez aprovisionado, puede consultar sus propiedades y supervisar su estado general o el específico del nodo.

### <a name="properties"></a>Propiedades

| Propiedad/estado | Descripción |
| --------------- | ----------- |
| CreateTime | Hora UTC a la que se creó la instancia de Integration Runtime de SSIS de Azure. |
| Nodos | Nodos asignados/disponibles de la instancia de Integration Runtime de SSIS de Azure con estados específicos de nodo (Starting/Available/Recycling/Unavailable) y errores que requieren acción. |
| OtherErrors | Errores inespecíficos del nodo que no requieren acción en la instancia de Integration Runtime de SSIS de Azure. |
| LastOperation | Resultado de la última operación de inicio/detención en la instancia de Integration Runtime de SSIS de Azure con errores que requieren acción, en caso de error de esta. |
| State | Estado general (Initial/Starting/Started/Stopping/Stopped) de la instancia de Integration Runtime de SSIS de Azure. |
| Location | Ubicación de la instancia de Integration Runtime de SSIS de Azure. |
| NodeSize | Tamaño de cada nodo de la instancia de Integration Runtime de SSIS de Azure. |
| NodeCount | Número de nodos de la instancia de Integration Runtime de SSIS de Azure. |
| MaxParallelExecutionsPerNode | Número de ejecuciones en paralelo por nodo en la instancia de Integration Runtime de SSIS de Azure. |
| CatalogServerEndpoint | Punto de conexión del servidor de Azure SQL Database/de la instancia administrada existente al host SSISDB. |
| CatalogAdminUserName | Nombre de usuario administrador del servidor de Azure SQL Database/de la instancia administrada. El servicio Data Factory utiliza esta información para preparar y administrar SSISDB en su nombre. |
| CatalogAdminPassword | Contraseña del administrador del servidor de Azure SQL Database/de la instancia administrada. |
| CatalogPricingTier | Plan de tarifa de SSISDB hospedado en el servidor de Azure SQL Database existente.  No es aplicable a la Instancia administrada de Azure SQL Database que hospeda SSISDB. |
| VNetId | Identificador del recurso de red virtual para que se una la instancia de Integration Runtime de SSIS de Azure. |
| Subnet | Nombre de la subred para que se una la instancia de Integration Runtime de SSIS de Azure. |
| id | Identificador del recurso de la instancia de Integration Runtime de SSIS de Azure. |
| Tipo | Tipo (administrada/autohospedada) de instancia de Integration Runtime de SSIS de Azure. |
| ResourceGroupName | Nombre del grupo de recursos de Azure donde se crearon las instancias de Data Factory y de Integration Runtime de SSIS de Azure. |
| DataFactoryName | Nombre de la instancia de Azure Data Factory. |
| Nombre | Nombre de la instancia de Integration Runtime de SSIS de Azure. |
| Descripción | Descripción de la instancia de Integration Runtime de SSIS de Azure. |

  
### <a name="status-per-node"></a>Estado (por nodo)

| Status | Descripción |
| ------ | ----------- | 
| Iniciando | Este nodo se está preparando. |
| Disponible | Este nodo está listo para la implementación y ejecución de paquetes SSIS. |
| Recycling | Este nodo se va a reparar/reiniciar. |
| No disponible | Este nodo no está listo para implementar ni ejecutar paquetes SSIS y tiene errores que requieren acción o problemas que se pueden resolver. |

### <a name="status-overall-azure-ssis-integration-runtime"></a>Estado (instancia general de Integration Runtime de SSIS de Azure)

| Estado general | Descripción | 
| -------------- | ----------- | 
| Initial | Los nodos de la instancia de Integration Runtime de SSIS de Azure no se han asignado/preparado. | 
| Iniciando | Los nodos de la instancia de Integration Runtime de SSIS de Azure se están asignando/preparando y la facturación ha comenzado. |
| Iniciado | Los nodos de la instancia de Integration Runtime de SSIS de Azure se han asignado/preparado y están listos para la implementación/ejecución de paquetes SSIS. |
| Deteniéndose  | Se están liberando los nodos de la instancia de Integration Runtime de SSIS de Azure. |
| Detenido | Los nodos de la instancia de Integration Runtime de SSIS de Azure se han liberado y la facturación se ha detenido. |

### <a name="monitor-the-azure-ssis-integration-runtime-in-the-azure-portal"></a>Supervisión de Integration Runtime de SSIS de Azure en Azure Portal

Las capturas de pantalla siguientes muestran cómo seleccionar la instancia de IR de SSIS de Azure para supervisión y proporcionan un ejemplo de la información que se muestra.

![Selección de Integration Runtime de SSIS de Azure para supervisión](media/monitor-integration-runtime/monitor-azure-ssis-ir-image1.png)

![Vista de información sobre Integration Runtime de SSIS de Azure](media/monitor-integration-runtime/monitor-azure-ssis-ir-image2.png)

### <a name="monitor-the-azure-ssis-integration-runtime-with-powershell"></a>Supervisión de Integration Runtime de SSIS de Azure con PowerShell

Use un script como el siguiente ejemplo para comprobar el estado de Integration Runtime de SSIS de Azure.

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Status
```

### <a name="more-info-about-the-azure-ssis-integration-runtime"></a>Más información sobre Integration Runtime de SSIS de Azure

Consulte los artículos siguientes para más información sobre Integration Runtime de SSIS de Azure:

- [Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime). En este artículo se proporciona información conceptual acerca de Integration Runtime en general, lo que incluye Integration Runtime de SSIS de Azure. 
- [Tutorial: Implementación de paquetes SSIS en Azure](tutorial-create-azure-ssis-runtime-portal.md). En este artículo se proporcionan instrucciones paso a paso para crear una instancia de IR de SSIS de Azure y se usa una base de datos de Azure SQL para hospedar el catálogo de SSIS. 
- [Cómo: Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md). Este artículo amplía el tutorial y proporciona instrucciones acerca del uso de Instancia administrada de Azure SQL Database y cómo unir un entorno de ejecución de integración a una red virtual. 
- [Administración de Integration Runtime de SSIS de Azure](manage-azure-ssis-integration-runtime.md). En este artículo se muestra cómo detener, iniciar o quitar una instancia de IR de SSIS de Azure. También se muestra cómo escalar horizontalmente IR de SSIS de Azure mediante la adición de más nodos a IR. 
- [Unión de una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md). En este artículo se proporciona información conceptual sobre cómo unir una instancia de Integration Runtime de SSIS de Azure a una red virtual de Azure. También se proporcionan los pasos para configurar la red virtual mediante Azure Portal para que se una la instancia de Integration Runtime de SSIS de Azure. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes artículos para la supervisión de las canalizaciones de maneras diferentes: 

- [Guía de inicio rápido:creación de una instancia de Data Factory](quickstart-create-data-factory-dot-net.md)
- [Uso de Azure Monitor de Azure para supervisar las canalizaciones Data Factory](monitor-using-azure-monitor.md)
