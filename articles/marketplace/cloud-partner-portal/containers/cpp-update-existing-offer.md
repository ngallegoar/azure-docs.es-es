---
title: Actualización de una oferta de contenedor de Azure existente | Azure Marketplace
description: Cómo actualizar una oferta de contenedor existente en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: dsindona
ms.openlocfilehash: 650ef45e56184b67efba67810580d9d6e763a4d0
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81271274"
---
# <a name="update-an-existing-container-offer"></a>Actualización de una oferta de contenedor existente

> [!IMPORTANT]
> A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de contenedores de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de contenedor de Azure](https://aka.ms/CreateContainerOffer) para administrar las ofertas migradas.

Este artículo le lleva por los diferentes aspectos de la actualización de la oferta de contenedor en [Cloud Partner Portal](https://cloudpartner.azure.com/).

Hay varios motivos por los que puede que quiera actualizar la oferta, como:

-  Agregue una nueva versión de imagen de contenedor a las SKU existentes.
-  Agregar nuevas SKU.
-  Actualizar los metadatos de Marketplace de la oferta o SKU individuales.

Para ayudarle con estas modificaciones, el portal ofrece las características **Comparar** e **Historial**.  


## <a name="unpermitted-changes-to-a-container-offer-or-sku"></a>Cambios no permitidos en una oferta de contenedor o una SKU

Hay atributos de una oferta de contenedor o SKU que no se pueden cambiar después de la publicación de la oferta en Azure Marketplace. No se pueden cambiar los valores siguientes:

-  **Id. de oferta** e **Id. de publicador** de la oferta.
-  **Id. de SKU** de las SKU existentes.
-  Etiquetas de versión, por ejemplo: `1.0.1`
-  Cambios en el modelo de facturación o licencia en las SKU existentes.

## <a name="common-update-operations"></a>Operaciones de actualización comunes

Las operaciones de actualización siguientes son comunes.

### <a name="update-container-image-version-for-a-sku"></a>Actualización de la versión de la imagen de contenedor para una SKU

Es habitual que una imagen de contenedor se actualice periódicamente con revisiones de seguridad, características adicionales, etc. En este escenario, quiere actualizar la imagen de contenedor a la que hace referencia la SKU mediante los pasos siguientes:

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiere actualizar.
3. En la pestaña **SKU**, seleccione la SKU asociada a la imagen de contenedor que se va a actualizar.
4. En **Imagen de contenedor**, seleccione **+ New Image Version** (+Nueva versión de imagen) para agregar una nueva imagen de contenedor.
5. Proporcione las **versiones de imagen** del nuevo contenedor. La versión de imagen debe seguir las mismas directrices sobre etiquetas que las versiones anteriores. Las etiquetas de versión deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros. Compruebe que la nueva versión que proporciona es mayor que todas las versiones anteriores.
6. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar la nueva versión de imagen de contenedor en Azure Marketplace.

### <a name="add-a-new-sku"></a>Agregar una nueva SKU

Use los siguientes pasos para poner una nueva SKU a disposición de la oferta:

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiere actualizar.
3. En la pestaña **SKU**, seleccione **Agregar nueva SKU** y proporcione un **Id. de SKU** en la ventana emergente.
4. Vuelva a publicar el contenedor mediante los pasos descritos en [Publicación de oferta de contenedor](./cpp-publish-offer.md).
5. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar la nueva SKU.

### <a name="update-offer-marketplace-metadata"></a>Actualizar los metadatos de una oferta de Marketplace

Use los pasos siguientes para actualizar los metadatos de Marketplace asociados con la oferta. (Por ejemplo: nombre de empresa, logotipos, etc.).

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiera actualizar.
3. Vaya a la pestaña **Marketplace**. Siga las instrucciones del artículo [Publicación de oferta de contenedor](./cpp-publish-offer.md) para realizar cambios en los metadatos.
4. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar los cambios.

## <a name="compare-feature"></a>Característica Comparar

Al realizar cambios en una oferta publicada, puede usar la característica **Comparar** para auditar los cambios realizados.

### <a name="to-use-the-compare-feature"></a>Para usar la característica Comparar:

1. En cualquier punto del proceso de edición, seleccione Comparar para la oferta.
2. Vea las versiones en paralelo de recursos de marketing y metadatos.


## <a name="history-of-publishing-actions"></a>Historial de acciones de publicación

Para ver la actividad de publicación histórica, seleccione la pestaña **Historial** de la barra de menús de navegación izquierda de Cloud Partner Portal. Puede ver las acciones con marca de tiempo que se han realizado durante la vigencia de las ofertas de Azure Marketplace.
