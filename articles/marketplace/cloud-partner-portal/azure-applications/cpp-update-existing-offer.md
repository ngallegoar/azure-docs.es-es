---
title: Actualización de una oferta de aplicación de Azure existente | Azure Marketplace
description: Cómo actualizar una oferta de aplicación de Azure existente en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: dsindona
ms.openlocfilehash: 152fd24fbc5d2762d381ffce2a937bc448858b0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275958"
---
# <a name="update-an-existing-azure-application-offer"></a>Actualización de una oferta de aplicación de Azure existente

Hay varios tipos de actualizaciones que puede que quiera realizar en su oferta una vez que esta se ha publicado y está activa. Cualquier cambio que realice en una nueva versión de la oferta se debe guardar y volver a publicar para que quede reflejado en Marketplace. Este artículo le lleva por los diferentes aspectos de la actualización de la oferta de aplicación administrada en [Cloud Partner Portal](https://cloudpartner.azure.com/).

Hay varios motivos por los que puede que quiera actualizar la oferta, como:

- Agregar una nueva versión de la imagen a las SKU existentes.
- Agregar nuevas SKU.
- Actualizar los metadatos de Marketplace de la oferta o SKU individuales.

Para ayudarle con estas modificaciones, el portal ofrece las características **Comparar** e **Historial**.

## <a name="unpermitted-changes-to-an-azure-application-offer-or-sku"></a>Cambios no permitidos en la oferta o SKU de una aplicación de Azure

Hay atributos de una oferta de contenedor o SKU que no se pueden cambiar después de la publicación de la oferta en Azure Marketplace. No se pueden cambiar los valores siguientes:

- Identificador de oferta e identificador de anunciante de la oferta.
- Identificador de SKU de las SKU existentes.
- Etiquetas de versión, por ejemplo: `1.0.1`
- Cambios en el modelo de facturación o licencia en las SKU existentes.

## <a name="common-update-operations"></a>Operaciones de actualización comunes

Las operaciones de actualización siguientes son comunes.

### <a name="update-image-version-for-a-sku"></a>Actualización de la versión de imagen para una SKU

Es habitual que una imagen se actualice periódicamente con revisiones de seguridad, características adicionales, etc. En este escenario, quiere actualizar la imagen a la que hace referencia la SKU mediante los pasos siguientes:

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiere actualizar.
3. En la pestaña **SKU**, seleccione la SKU asociada a la imagen que se va a actualizar.
4. Seleccione **+ New Image Version** (+ Nueva versión de imagen) para agregar una nueva imagen.
5. Proporcione las nuevas versiones de la imagen. La versión de imagen debe seguir las mismas directrices sobre etiquetas que las versiones anteriores. Las etiquetas de versión deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros. Compruebe que la nueva versión que proporciona es mayor que todas las versiones anteriores.
6. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar la nueva versión de imagen de contenedor en Azure Marketplace.

### <a name="add-a-new-sku"></a>Agregar una nueva SKU

Use los siguientes pasos para poner una nueva SKU a disposición de la oferta:

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiere actualizar.
3. En la pestaña **SKU**, seleccione **Agregar nueva SKU** y proporcione un **Id. de SKU** en la ventana emergente.
4. Vuelva a publicar la oferta mediante los pasos descritos en [Publicación de una oferta de aplicación de Azure](./cpp-publish-offer.md).
5. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar la nueva SKU.

### <a name="update-offer-marketplace-metadata"></a>Actualizar los metadatos de una oferta de Marketplace

Use los pasos siguientes para actualizar los metadatos de Marketplace asociados con la oferta. (Por ejemplo: nombre de empresa, logotipos, etc.).

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiera actualizar.
3. Vaya a la pestaña **Marketplace**. Siga las instrucciones del artículo [Publicación de una oferta de aplicación de Azure](./cpp-publish-offer.md) para hacer cambios en los metadatos.
4. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar los cambios.
 
>[!Note]
>La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible.  Consulte [Proveedores de soluciones en la nube](../../cloud-solution-providers.md) para obtener más información sobre el marketing que ofrece a través de los canales de asociados de CSP de Microsoft.

## <a name="deleting-an-existing-offer"></a>Eliminación de una oferta existente

Puede decidir eliminar su oferta de Marketplace. La eliminación de una oferta no afecta a las compras actuales de dicha oferta. Esas compras de clientes seguirán funcionando como antes. Sin embargo, la oferta no estará disponible para nuevas compras una vez completada la eliminación.

La finalización de la oferta es el proceso de terminación del servicio o contrato de licencia entre usted y los clientes existentes.
Las guías y las directivas relacionadas con la eliminación y la terminación de ofertas se rigen por el contrato del anunciante de Microsoft Marketplace (consulte la sección 7) y las directivas de participación (consulte la sección 6.2).

Para más información, consulte [Eliminación de una oferta/SKU de Azure Marketplace](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-managed-app-offer-delete).

## <a name="compare-feature"></a>Característica Comparar

Al realizar cambios en una oferta publicada, puede usar la característica Comparar para auditar los cambios realizados.

Para usar la característica Comparar:

1. En cualquier punto del proceso de edición, seleccione Comparar para la oferta.
2. Vea las versiones en paralelo de recursos de marketing y metadatos.

## <a name="history-of-publishing-actions"></a>Historial de acciones de publicación

Para ver la actividad de publicación histórica, seleccione la pestaña **Historial** de la barra de menús de navegación izquierda de Cloud Partner Portal. Puede ver las acciones con marca de tiempo que se han realizado durante la vigencia de las ofertas de Azure Marketplace.

## <a name="next-steps"></a>Pasos siguientes

[Oferta de aplicación de Azure](./cpp-azure-app-offer.md)
