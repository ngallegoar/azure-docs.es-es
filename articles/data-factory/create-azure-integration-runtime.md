---
title: Creación de un entorno de ejecución de integración de Azure en Azure Data Factory
description: Aprenda a crear una instancia de Azure Integration Runtime en Azure Data Factory, que se usa para copiar datos y distribuir actividades de transformación.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/15/2018
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.openlocfilehash: 87633abaaae1f6034709c6e552be6647533115ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79230020"
---
# <a name="how-to-create-and-configure-azure-integration-runtime"></a>Cómo crear y configurar una instancia de Azure Integration Runtime
Integration Runtime (IR) es la infraestructura de proceso que usa Azure Data Factory para proporcionar capacidades de integración de datos en distintos entornos de red. Para obtener más información sobre IR, vea [Integration Runtime](concepts-integration-runtime.md).

Azure IR proporciona un proceso totalmente administrado para llevar a cabo movimientos de datos de forma nativa y trasladar actividades de transformación de datos para procesar servicios como HDInsight. Se hospeda en el entorno de Azure y es compatible con la conexión a recursos en el entorno de red pública con puntos de conexión accesibles públicos.

En este documento se muestra cómo crear y configurar Azure Integration Runtime. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-azure-ir"></a>Azure IR predeterminado
De forma predeterminada, cada factoría de datos tiene una instancia de Azure IR en el back-end que admite la ejecución de operaciones en almacenes de datos en la nube y servicios de proceso en la red pública. La ubicación de esa instancia de Azure IR es auto-resolve. Si la propiedad **connectVia** no se especifica en la definición del servicio vinculado, se usa la instancia de Azure IR predeterminada. Basta con crear una instancia de Azure IR de forma explícita cuando quiera definir de forma explícita la ubicación del IR o cuando quiera agrupar de forma virtual las ejecuciones de actividades en distintos IR con fines de administración. 

## <a name="create-azure-ir"></a>Crear una instancia de Azure IR
Integration Runtime se puede crear con el cmdlet de PowerShell **Set-AzDataFactoryV2IntegrationRuntime**. Para crear una instancia de Azure IR, especifique el nombre, la ubicación y el tipo en el comando. A continuación se muestra un comando de ejemplo para crear una instancia de Azure IR{1}{2}con la ubicación establecida en "Europa Occidental":

```powershell
Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName "SampleV2DataFactory1" -Name "MySampleAzureIR" -ResourceGroupName "ADFV2SampleRG" -Type Managed -Location "West Europe"
```  
Para Azure IR, el tipo se debe establecer en **Administrado**. No es necesario especificar los detalles de proceso, porque están totalmente administrados de forma elástica en la nube. Especifique detalles de proceso, como el tamaño y el recuento de los nodos, cuando quiera crear una instancia de IR de Azure SSIS. Para más información, vea [Crear y configurar un IR de Azure SSIS](create-azure-ssis-integration-runtime.md).

Puede configurar una instancia de Azure IR existente para cambiar su ubicación mediante el cmdlet de PowerShell Set-AzDataFactoryV2IntegrationRuntime. Para más información sobre la ubicación de una instancia de Azure IR, vea [Introducción a Integration Runtime](concepts-integration-runtime.md).

## <a name="use-azure-ir"></a>Usar Azure IR

Una vez creada la instancia de Azure IR, puede hacer referencia a ella en la definición del servicio vinculado. A continuación se muestra un ejemplo de cómo hacer referencia a la instancia de Azure Integration Runtime creada anteriormente a partir de un servicio vinculado de Azure Storage:  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=myaccountname;AccountKey=..."
      },
      "connectVia": {
        "referenceName": "MySampleAzureIR",
        "type": "IntegrationRuntimeReference"
      }   
    }
}

```

## <a name="next-steps"></a>Pasos siguientes
Vea los siguientes artículos sobre cómo crear otros tipos de instancias de Integration Runtime:

- [Crear Integration Runtime autohospedado](create-self-hosted-integration-runtime.md)
- [Crear una instancia de Integration Runtime de Azure SSIS](create-azure-ssis-integration-runtime.md)
 
