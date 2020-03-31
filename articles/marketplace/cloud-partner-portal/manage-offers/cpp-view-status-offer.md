---
title: Visualización del estado de las ofertas de Marketplace | Azure Marketplace
description: Visualización del estado de las ofertas de los Marketplaces de Azure y AppSource mediante Cloud Partner Portal
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: dsindona
ms.openlocfilehash: 0cbe6a45ba205f32a764bdadb021dc0dcf5bf0cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275975"
---
# <a name="view-the-publishing-status-of-azure-marketplace-and-appsource-offers"></a>Visualización del estado de publicación de las ofertas de Azure Marketplace y AppSource

Después de crear una oferta y especialmente durante el proceso de publicación, puede ver el estado de la oferta en Cloud Partner Portal.  El estado de publicación general está disponible en las páginas [**Todas las ofertas**](../portal-tour/cpp-all-offers-page.md) y [**Aprobaciones**](../portal-tour/cpp-approvals-page.md) del portal.  Uno de los siguientes indicadores de estado se debe mostrar para cada oferta.  

|            Status              |   Descripción                                                           |
|            ------              |   -----------                                                           |
| **-**                          | Se ha creado la oferta pero no ha comenzado el proceso de publicación.            |
| **Publicación en curso**        | Se están realizando los pasos del proceso de publicación para la oferta.   |
| **Error al publicar**:             | Se ha detectado un problema crítico durante la validación o la revisión de Microsoft. |
| **Publicación cancelada**           | El anunciante ha cancelado el proceso de publicación de la oferta.  Este estado no elimina del Marketplace a una oferta existente. | 
| **Awaiting publisher sign out** (Esperando el cierre de sesión del anunciante) | Microsoft ha recibido la oferta y está en espera de la comprobación final del anunciante. |
| **Delisted** (Quitada de la lista)                   | Una oferta publicada anteriormente se ha quitado del Marketplace.      | 
|  |  |


## <a name="publishing-status-details"></a>Detalles del estado de publicación 

Puede obtener más detalles sobre el estado de una oferta mientras se realiza el proceso de publicación en la pestaña **Estado** de la página **Nueva oferta**.  Esta página enumera todos los pasos de publicación para ese tipo de oferta.  *Tenga en cuenta que el número y detalles específicos de los pasos a menudo son diferentes entre los tipos de oferta.*  En esta página también se indican los problemas pendientes generados por la comprobación y pasos de revisión de Microsoft, que a menudo necesitan que el anunciante realice alguna acción antes de continuar el proceso de publicación.  Por ejemplo, la siguiente imagen muestra la pestaña **Estado** para una nueva oferta de máquina virtual. 

![Pestaña de estado de la oferta de máquina virtual](./media/vm-offer-pub-steps1.png)

En la siguiente pestaña de **Estado** de ejemplo para un servicio de consultoría, se muestra un error notificado en la configuración de administración de clientes potenciales.  Dado que la administración de clientes potenciales es necesaria para los servicios de consultoría, este error se debe corregir antes de continuar con la publicación.

![Pestaña de estado que muestra un error del servicio de consultoría](./media/consulting-service-error.png)

En el último estado de ejemplo de una aplicación de Azure se muestra un problema crítico de revisión de Microsoft.  Contiene un vínculo activo al elemento Azure DevOps que incluye información detallada sobre este problema de revisión.  Para obtener más información, consulte [Publicación de la ofertas de una aplicación de Azure](cpp-publish-offer.md).

![Pestaña de estado de una aplicación de Azure que muestra el problema de revisión](../azure-applications/media/status-tab-ms-review.png)


## <a name="next-steps"></a>Pasos siguientes

Para corregir los problemas pendientes o actualizar la configuración de la oferta, debe [actualizar una oferta](./cpp-update-offer.md). 
