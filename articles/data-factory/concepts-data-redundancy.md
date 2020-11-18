---
title: Redundancia de datos en Azure Data Factory | Microsoft Docs
description: Obtenga información sobre los mecanismos de redundancia de metadatos en Azure Data Factory.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: weehyongtok
ms.reviewer: abnarain
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: abnarain
ms.openlocfilehash: f71fdf66624d67939f915f91c2cc1dbe7553cad7
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94332018"
---
# <a name="azure-data-factory-data-redundancy"></a>**Redundancia de datos en Azure Data Factory**

Los datos de Azure Data Factory incluyen metadatos (canalizaciones, conjuntos de datos, servicios vinculados, entorno de ejecución de integración y desencadenadores) y datos de supervisión (ejecuciones de canalizaciones, desencadenadores y actividades). 

En todas las regiones (excepto Sur de Brasil y Sudeste Asiático), los datos de Azure Data Factory se almacenan y replican en la [región emparejada](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#azure-regional-pairs) para protegerse frente a la pérdida de metadatos. Durante los errores del centro de datos regional, Microsoft puede iniciar una conmutación por error regional de la instancia de Azure Data Factory. En la mayoría de los casos, no se requiere ninguna acción por su parte. Cuando se haya completado la conmutación por error administrada por Microsoft, podrá acceder a Azure Data Factory en la región de conmutación por error. 

Debido a los requisitos de residencia de datos en Sur de Brasil y Sudeste Asiático, los datos de Azure Data Factory se almacenan [únicamente en la región local](https://docs.microsoft.com/azure/storage/common/storage-redundancy#locally-redundant-storage). Para el Sudeste Asiático, todos los datos se almacenan en Singapur. Para el Sur de Brasil, todos los datos se almacenan en Brasil. Cuando la región se pierde debido a un desastre significativo, Microsoft no podrá recuperar los datos de Azure Data Factory.  

> [!NOTE]
> La conmutación por error administrada por Microsoft no se aplica al entorno de ejecución de integración autohospedado (SHIR), ya que esta infraestructura normalmente es administrada por el cliente. Si el SHIR está configurado en una máquina virtual de Azure, se recomienda aprovechar [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) para controlar la [conmutación por error de la máquina virtual de Azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture) a otra región.



## <a name="using-source-control-in-azure-data-factory"></a>**Uso del control de código fuente en Azure Data Factory**

Para asegurarse de que puede realizar el seguimiento y la auditoría de los cambios realizados en los metadatos de Azure Data Factory, considere la posibilidad de configurar el control de código fuente para la instancia de Azure Data Factory. También le permitirá acceder a los archivos JSON de metadatos para canalizaciones, conjuntos de datos, servicios vinculados y desencadenadores. Azure Data Factory le permite trabajar con diferentes repositorios de Git (Azure DevOps y GitHub). 

 Aprenda a configurar el [control de código fuente en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/source-control). 

> [!NOTE]
> En caso de desastre (pérdida de región), la nueva factoría de datos se puede aprovisionar manualmente o de forma automatizada. Una vez creada la nueva factoría de datos, puede restaurar el código JSON de las canalizaciones, los conjuntos de datos y los servicios vinculados desde el repositorio de Git existente. 



## <a name="data-stores"></a>**Almacenes de datos**

Azure Data Factory permite trasladar datos entre los almacenes de datos locales y en la nube. Para garantizar la continuidad empresarial con los almacenes de datos, debe consultar las recomendaciones de continuidad empresarial para cada uno de estos almacenes de datos. 

 

## <a name="see-also"></a>Consulte también

- [Pares regionales de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
- [Residencia de datos en Azure](https://azure.microsoft.com/global-infrastructure/data-residency/) 
