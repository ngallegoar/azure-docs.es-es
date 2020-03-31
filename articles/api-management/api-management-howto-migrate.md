---
title: Migración de Azure API Management entre regiones
description: Aprenda a migrar una instancia de API Management de una región a otra.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/26/2019
ms.author: apimpm
ms.openlocfilehash: 3294a7b2112e9527041ef343f4452aedb7a2a272
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70073459"
---
# <a name="how-to-migrate-azure-api-management-across-regions"></a>Migración de Azure API Management entre regiones
Para migrar instancias de API Management de una región de Azure a otra, puede usar la característica de [copia de seguridad y restauración](api-management-howto-disaster-recovery-backup-restore.md). Debe elegir el mismo plan de tarifa de API Management en las regiones de origen y de destino. 

> [!NOTE]
> La copia de seguridad y restauración no funcionarán si la migración se realiza entre distintos tipos de nube. Si ese es el caso, deberá exportar el recurso [como una plantilla](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-portal#export-resource-groups-to-templates). Después, adapte la plantilla exportada a la región de Azure de destino y vuelva a crear el recurso. 

## <a name="option-1-use-a-different-api-management-instance-name"></a>Opción 1: Usar un nombre de instancia de API Management diferente

1. En la región de destino, cree una nueva instancia de API Management con el mismo plan de tarifa que la instancia de API Management de origen. La nueva instancia debe tener un nombre diferente. 
1. Realice una copia de seguridad de la instancia de API Management existente en una cuenta de almacenamiento.
1. Restaure la copia de seguridad creada en el paso 2 en la nueva instancia de API Management creada en la nueva región en el paso 1.
1. Si tiene un dominio personalizado que apunta a la instancia de API Management de la región de origen, actualice el CNAME del dominio personalizado para que apunte a la nueva instancia de API Management. 


## <a name="option-2-use-the-same-api-management-instance-name"></a>Opción 2: Usar el mismo nombre para la instancia de API Management

> [!NOTE]
> Esta opción producirá un tiempo de inactividad durante la migración.

1. Realice una copia de seguridad de la instancia de API Management de la región de origen en una cuenta de almacenamiento.
1. Elimine la instancia de API Management en la región de origen. 
1. Cree una nueva instancia de API Management en la región de destino con el mismo nombre que el de la región de origen.
1. Restaure la copia de seguridad creada en el paso 1 en la nueva instancia de API Management de la región de destino.  


## <a name="next-steps"></a><a name="next-steps"> </a>Pasos siguientes
* Para más información sobre la característica de copia de seguridad y restauración, consulte [Implementación de la recuperación ante desastres](api-management-howto-disaster-recovery-backup-restore.md).
* Para más información sobre la migración de recursos, consulte la [guía para migración entre regiones de Azure](https://github.com/Azure/Azure-Migration-Guidance).