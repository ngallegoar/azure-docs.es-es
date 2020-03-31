---
title: Eliminación de las ofertas de Marketplace | Azure Marketplace
description: Eliminación de las ofertas de los Marketplaces de Azure y AppSource mediante Cloud Partner Portal
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: dsindona
ms.openlocfilehash: fa8ee834bd6d9261c47c540e9517c200c894b6c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286462"
---
# <a name="delete-azure-marketplace-and-appsource-offers-or-skus"></a>Eliminación de las ofertas o SKU de Azure Marketplace y AppSource

Por diversas razones, puede decidir retirar su oferta del Marketplace de Microsoft de alguna de estas dos formas:

- La *eliminación de ofertas* garantiza que los clientes nuevos ya no puedan adquirir ni implementar la oferta, pero no afecta a los clientes existentes, a quienes debe ofrecer soporte técnico según su contrato de licencia y las leyes aplicables. 
- La *finalización de la oferta* es el proceso de terminación del servicio o contrato de licencia entre usted y los clientes existentes. 

Las guías y las directivas relacionadas con la eliminación y la terminación de ofertas se rigen por el [contrato del anunciante de Microsoft Marketplace](https://go.microsoft.com/fwlink/?LinkID=699560) y las [directivas de participación](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (sección [Suspensión y eliminación de la oferta](https://docs.microsoft.com/legal/marketplace/participation-policy#offering-suspension-and-removal)). 

En este artículo se habla de los diferentes escenarios de eliminación admitidos y los pasos necesarios para llevarlos a cabo.  

> [!NOTE]
> Puede eliminar una oferta que no se ha publicado si simplemente selecciona el botón **Eliminar** de la barra de herramientas en la pestaña **Editor**.


## <a name="delete-a-published-sku-from-the-azure-marketplace"></a>Eliminación de una SKU publicada de Azure Marketplace

Puede eliminar una SKU publicada de Azure Marketplace si sigue estos pasos:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2.  En la página **Todas las ofertas**, seleccione la oferta.  La oferta debe mostrarse en la pestaña **Editor**.
3.  En la barra de herramientas izquierda, seleccione la pestaña **SKU**. 
4.  Seleccione la SKU que quiera eliminar y haga clic en el botón **Eliminar**.
5.  [Vuelva a publicar](./cpp-publish-offer.md) la oferta en Azure Marketplace.

Una vez que se haya publicado la oferta modificada en Azure Marketplace, la SKU seleccionada ya no aparecerá en Azure Portal ni en el portal de Azure Marketplace.


## <a name="roll-back-to-a-previous-sku-version"></a>Reversión a una versión anterior de una SKU

Puede eliminar la versión actual de una SKU publicada de Azure Marketplace mediante los pasos siguientes. Cuando se completa el proceso, la SKU se revierte a la versión anterior.

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En la página **Todas las ofertas**, seleccione la oferta.  La oferta debe mostrarse en la pestaña **Editor**.
3. En la barra de herramientas izquierda, seleccione la pestaña **SKU**. 
4. Elimine la versión más reciente del recurso de solución asociado desde la lista de versiones del disco.  Según el tipo de oferta, este campo podría ser **Disk Version** (Versión del disco), **Package Versions** (Versiones del paquete) o un recurso similar. 
5. [Vuelva a publicar](./cpp-publish-offer.md) la oferta en Azure Marketplace.

Cuando se publique la oferta modificada en Azure Marketplace, ya no aparecerá la versión actual de la SKU publicada ni en Azure Marketplace ni en Azure Portal.  La SKU se revertirá a la versión anterior.


## <a name="delete-a-live-offer"></a>Eliminación de una oferta en directo

Hay varios aspectos empresariales, legales y de procedimiento necesarios para eliminar una oferta publicada. Siga estos pasos si quiere recibir ayuda del equipo de soporte técnico para eliminar una oferta publicada del Azure Marketplace:

1.  Genere una incidencia de soporte técnico mediante la página [Create an incident](https://go.microsoft.com/fwlink/?linkid=844975) (Crear una incidencia) o bien haga clic en **Soporte técnico** en la esquina superior derecha de [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  Seleccione el tipo de oferta específico en la lista **Tipo de problema** y seleccione **Remove a published offer** (Quitar una oferta publicada) en la lista **Categoría**.

3.  Envíe la solicitud.

El equipo de soporte técnico le guiará en el proceso de eliminación de la oferta.

> [!NOTE]
> La eliminación de una oferta (o SKU) no afecta a las compras actuales de dicha oferta (o SKU). Estas compras de clientes seguirán funcionando como antes. Sin embargo, las ofertas o SKU eliminadas ya no estarán disponibles para futuras compras.


## <a name="next-steps"></a>Pasos siguientes

Cuando se haya familiarizado con las operaciones básicas que se usan para administrar ofertas, estará listo para crear una instancia de [oferta del Marketplace](../cpp-marketplace-offers.md) de Microsoft.
