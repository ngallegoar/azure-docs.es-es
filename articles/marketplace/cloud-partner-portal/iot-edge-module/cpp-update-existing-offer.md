---
title: Actualizar una oferta existente de módulo Azure IoT Edge | Azure Marketplace
description: Cómo actualizar una oferta existente de módulo IoT Edge en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 019711f35d249e6684013feddb6b453d509f66d4
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985361"
---
# <a name="update-an-existing-iot-edge-module-offer"></a>Actualizar una oferta existente de módulo IoT Edge

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de módulos de IoT Edge al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de módulo de IoT Edge](https://aka.ms/AzureCreateIoT) para administrar las ofertas migradas.

En este artículo se repasan los diferentes aspectos de la actualización de una oferta de módulo IoT Edge en [Cloud Partner Portal](https://cloudpartner.azure.com/) y de la nueva publicación de la oferta.

Hay varios motivos por los que puede que quiera actualizar la oferta, como:

-  Agregar una nueva versión de imagen de módulo IoT Edge a las SKU existentes.
-  Agregar nuevas SKU.
-  Actualizar los metadatos de Marketplace de la oferta o SKU individuales.

Para ayudarle con estas modificaciones, el portal ofrece las características **Comparar** e **Historial**.  


## <a name="unpermitted-changes-to-iot-edge-module-offer-or-sku"></a>Cambios no permitidos de la oferta de módulo IoT Edge o la SKU

Hay atributos de una oferta de módulo IoT Edge o SKU que no se pueden cambiar después de la publicación de la oferta en Azure Marketplace. No se pueden cambiar los valores siguientes:

-  **Id. de oferta** e **Id. de publicador** de la oferta.
-  **Id. de SKU** de las SKU existentes.
-  Etiquetas de versión, por ejemplo: `1.0.1`
-  Cambios en el modelo de facturación o licencia en las SKU existentes.

## <a name="common-update-operations"></a>Operaciones de actualización comunes

Las operaciones de actualización siguientes son comunes.

### <a name="update-the-iot-edge-module-image-version-for-a-sku"></a>Actualizar la versión de imagen de módulo IoT Edge de una SKU

Es habitual que una imagen de módulo IoT Edge se actualice periódicamente con revisiones de seguridad, características adicionales, etc. En este escenario, quiere actualizar la imagen de módulo IoT Edge a la que hace referencia la SKU mediante los pasos siguientes:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que quiere actualizar.

3.  En la pestaña **SKU**, seleccione la SKU asociada a la imagen de módulo IoT Edge que se va a actualizar.

4.  En **Imagen de módulo Edge**, seleccione **+ Nueva versión de la imagen** para agregar una nueva imagen de módulo IoT Edge.

5.  Proporcione las nuevas **versiones de imagen** del módulo IoT Edge. La versión de imagen debe seguir las mismas directrices sobre etiquetas que las versiones anteriores. Las etiquetas de versión deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros. Compruebe que la nueva versión que proporciona es mayor que todas las versiones anteriores.

6.  Seleccione **Publicar** para iniciar el flujo de trabajo para publicar la nueva versión del módulo IoT Edge en Azure Marketplace.

### <a name="add-a-new-sku"></a>Agregar una nueva SKU

Use los siguientes pasos para poner una nueva SKU a disposición de la oferta: 

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que quiere actualizar.

3.  En la pestaña **SKU**, seleccione **Agregar nueva SKU** y proporcione un **Id. de SKU** en la ventana emergente.

4.  Vuelva a publicar el módulo IoT Edge mediante los pasos indicados en [Publicar una oferta de módulo IoT Edge en Azure Marketplace](./cpp-publish-offer.md).

5.  Seleccione **Publicar** para iniciar el flujo de trabajo para publicar la nueva SKU.


### <a name="update-offer-marketplace-metadata"></a>Actualizar los metadatos de una oferta de Marketplace

Use los pasos siguientes para actualizar los metadatos de Marketplace asociados con la oferta. (Por ejemplo: nombre de empresa, logotipos, etc.).

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que quiere actualizar.

3.  Vaya a la pestaña **Marketplace**. Use las instrucciones del artículo [Publicar una oferta de módulo IoT Edge en Azure Marketplace](./cpp-publish-offer.md) para realizar cambios de metadatos.

4.  Seleccione **Publicar** para iniciar el flujo de trabajo para publicar los cambios.

## <a name="compare-feature"></a>Característica Comparar

Al realizar cambios en una oferta publicada, puede usar la característica **Comparar** para auditar los cambios realizados. 

**Para usar la característica Comparar:**

1.  En cualquier punto del proceso de edición, seleccione **Comparar** para la oferta.

    ![Botón de la característica Comparar](./media/iot-edge-module-compare.png)


2.  Vea las versiones en paralelo de recursos de marketing y metadatos.


## <a name="history-of-publishing-actions"></a>Historial de acciones de publicación

Para ver la actividad de publicación histórica, seleccione la pestaña **Historial** de la barra de menús de navegación izquierda de Cloud Partner Portal. Puede ver las acciones con marca de tiempo que se han realizado durante la vigencia de las ofertas de Azure Marketplace.  <!-- Need to find correct link here:  legal time windowsFor more information, see [History page](cpp-history-page.md) -->
