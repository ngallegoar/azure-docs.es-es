---
title: Conceptos sobre almacenamiento
description: Obtenga información sobre las funcionalidades de almacenamiento clave en las nubes privadas de la solución de VMware en Azure (AVS) en versión preliminar.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 7f92e8f961422f8354e55192ebdddd077d61acd1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84604123"
---
# <a name="azure-vmware-solution-avs-preview-storage-concepts"></a>Conceptos de almacenamiento de la solución de VMware en Azure (AVS) en versión preliminar

Las nubes privadas de AVS proporcionan almacenamiento nativo en todo el clúster con vSAN de VMware. Todo el almacenamiento local de cada host de un clúster se usa en un almacén de datos de vSAN, y el cifrado de datos en reposo está disponible y habilitado de forma predeterminada. Puede usar recursos de Azure Storage para ampliar las funcionalidades de almacenamiento de las nubes privadas.

## <a name="vsan-clusters"></a>Clústeres de vSAN

El almacenamiento local en cada host de un clúster se usa como parte de un almacén de datos de vSAN. Todos los grupos de discos usan un nivel de caché de NVMe de 1,6 TB con la capacidad sin procesar, por host y basada en SSD de 15,4 TB. El tamaño del nivel de capacidad sin procesar de un clúster es la capacidad por host multiplicada por la cantidad de hosts. Por ejemplo, un clúster de cuatro hosts proporcionará una capacidad sin procesar de 61,6 TB en el nivel de capacidad de vSAN.

El almacenamiento local en los hosts de un clúster se usa en el almacén de datos de vSAN en todo el clúster. Todos los almacenes de datos se crean como parte de una implementación de nube privada y están disponibles para su uso inmediato. El usuario cloudadmin y todos los usuarios del grupo CloudAdmin pueden administrar almacenes de datos con estos privilegios de vSAN:
- Datastore.AllocateSpace
- Datastore.Browse
- Datastore.Config
- Datastore.DeleteFile
- Datastore.FileManagement
- Datastore.UpdateVirtualMachineMetadata

## <a name="data-at-rest-encryption"></a>Cifrado de datos en reposo

Los almacenes de datos de vSAN usan el cifrado de datos en reposo de forma predeterminada. La solución de cifrado se basa en KMS y admite operaciones de vCenter para la administración de claves. Las claves se almacenan cifradas, encapsuladas por una clave maestra de Azure Key Vault basada en HSM. Cuando se quita un host de un clúster por cualquier motivo, los datos en los SSD se invalidan inmediatamente.

## <a name="scaling"></a>Ampliación

La capacidad nativa de almacenamiento de un clúster se escala mediante la adición de hosts al clúster. En el caso de los clústeres que usan hosts HE, la capacidad sin procesar en todo el clúster aumenta 15,4 TB con cada host adicional. La capacidad sin procesar de los clústeres que se crean con hosts GP aumenta 7,7 TB con cada host adicional. En ambos tipos de clústeres, los hosts tardan unos 10 minutos en agregarse a un clúster. Para obtener instrucciones sobre el escalado de clústeres, consulte el [tutorial sobre escalado de nubes privadas][tutorial-scale-private-cloud].

## <a name="azure-storage-integration"></a>Integración del almacenamiento de Azure

Puede usar los servicios de almacenamiento de Azure en las cargas de trabajo que se ejecutan en su nube privada. Entre los servicios de almacenamiento de Azure se incluyen cuenta de almacenamiento, Table Storage y Blob Storage. La conexión de las cargas de trabajo a los servicios de almacenamiento de Azure no atraviesa Internet. Esta conectividad ofrece seguridad adicional y le permite usar los servicios de almacenamiento de Azure basados en SLA en las cargas de trabajo de su nube privada.

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso es obtener información sobre los [conceptos de identidad de nubes privadas][concepts-identity].

<!-- LINKS - external-->

<!-- LINKS - internal -->
[tutorial-scale-private-cloud]: ./tutorial-scale-private-cloud.md
[concepts-identity]: ./concepts-identity.md
