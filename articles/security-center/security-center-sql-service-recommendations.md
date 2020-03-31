---
title: 'Recomendaciones de seguridad para el almacenamiento y los datos: Azure Security Center'
description: En este documento se explica cómo las recomendaciones de Azure Security Center ayudan a proteger el servicio SQL de Azure y los datos y a cumplir las directivas de seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2019
ms.author: memildin
ms.openlocfilehash: 74ed55e1d460495bfa8d3d4c00bd37bb7f05260e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552872"
---
# <a name="protect-azure-data-and-storage-services"></a>Protección de datos de Azure y servicios de almacenamiento
Cuando Azure Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que lo guiarán por el proceso de configuración de los controles necesarios para reforzar y proteger sus recursos.

En este artículo se explica la página **Seguridad de los datos** de la sección de seguridad de recursos de Security Center.

Para obtener una lista completa de las recomendaciones que puede ver en esta página, consulte [Recomendaciones de datos y almacenamiento](recommendations-reference.md#recs-datastorage).


## <a name="view-your-data-security-information"></a>Visualización de la información de seguridad de los datos

1. En la sección **Resource security hygiene** (Protección de seguridad de recursos), seleccione **Data and storage resources** (Recursos de datos y almacenamiento).

    ![Recursos de datos y almacenamiento](./media/security-center-monitoring/click-data.png)

    Se abre la página **Data security** (Seguridad de los datos) con recomendaciones para los recursos de datos.

    [![Recursos de datos](./media/security-center-monitoring/sql-overview.png)](./media/security-center-monitoring/sql-overview.png#lightbox)

    En esta página puede realizar las siguientes acciones:

    * Haga clic en la pestaña **Overview** (Información general) para mostrar todas las recomendaciones de recursos de datos que deben corregirse. 
    * Haga clic en cada pestaña y vea las recomendaciones por tipo de recurso.

    > [!NOTE]
    > Para obtener más información sobre cifrado de almacenamiento, vea [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md).


## <a name="remediate-a-recommendation-on-a-data-resource"></a>Corrección de una recomendación en un recurso de datos

1. En cualquiera de las pestañas de recursos, haga clic en un recurso. Se abre la página de información con las recomendaciones que deben corregirse.

    ![Información de recursos](./media/security-center-monitoring/sql-recommendations.png)

2. Haga clic en una recomendación. Se abre la página de la recomendación que muestra los **pasos de corrección** para implementar la recomendación.

   ![Pasos de corrección](./media/security-center-monitoring/remediate1.png)

3. Haga clic en **Take action** (Realizar acción). Aparece la página de configuración de recursos.

    ![Habilitación de la recomendación](./media/security-center-monitoring/remediate2.png)

4. Siga los **pasos de corrección** y haga clic en **Save** (Guardar).


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes temas:

* [Lista de referencia completa de las recomendaciones de seguridad de Azure Security Center](recommendations-reference.md)
* [Protección de las aplicaciones y las máquinas en Azure Security Center](security-center-virtual-machine-protection.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)