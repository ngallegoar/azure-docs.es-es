---
title: Planes y SKU privados | Azure Marketplace
description: Cómo usar las SKU privadas para administrar la disponibilidad de la oferta.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: dsindona
ms.openlocfilehash: ee3ab7be4d15b13a3c0bb014a3ca4d4096299b4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280395"
---
<a name="private-skus-and-plans"></a>SKU y planes privados
============

Las SKU privadas le permiten restringir la disponibilidad de las SKU a clientes específicos. Cuando una SKU se marca como privada, no está disponible en ningún catálogo público, incluidos [Azure Marketplace](https://azuremarketplace.microsoft.com) y [Azure Portal](https://portal.azure.com). En Azure Portal, solo los clientes con acceso a la SKU pueden verla. Además, también se les puede exigir que tengan acceso a las ofertas privadas.

>[!NOTE]
>Las SKU privadas deben tener identificadores de SKU o plan únicos para evitar conflictos con sus SKU públicas.

Puede usar las SKU privadas para controlar los escenarios siguientes:

1.  Publique software que quiera que esté disponible públicamente solo para clientes específicos y no para el público general.
2.  Publique las variaciones de software público a un precio personalizado para clientes específicos.
3.  Publique las variaciones de software público con condiciones y con una descripción personalizadas (a través de una oferta nueva).

Si solo desea quiere cambiar el precio, puede volver a usar los discos de otra SKU en la misma oferta. Con las SKU privadas, no tiene que volver a enviar discos en todas las SKU.

<a name="mark-a-sku-private"></a>Marcar una SKU como privada
---------------------

Para marcar una SKU como privada, active o desactive la opción que le pregunta si la SKU es privada:

![Marcar una SKU como privada](./media/cloud-partner-portal-publish-virtual-machine/markingskuprivate.png)

Puede volver a usar los discos en otra SKU y modificar los precios o la descripción. Para volver a usar los discos, seleccione **Sí** como respuesta al mensaje que pregunta si la SKU reutiliza las imágenes de una SKU pública.

Si la SKU está marcada como privada y la oferta tiene otras SKU con discos reutilizables, deberá indicar que la SKU reutiliza los discos de otra SKU. También es necesario especificar la audiencia de destino de la SKU privada.

>[!NOTE]
>Una vez publicada, una SKU pública no puede convertirse en privada.

<a name="select-an-image"></a>Seleccionar una imagen
------------------

Puede proporcionar nuevos discos para la SKU privada o volver a usar los mismos discos proporcionados en otra SKU, y modificar solo los precios o la descripción. Para volver a usar los discos, seleccione **Sí** como respuesta al mensaje que pregunta si la SKU reutiliza las imágenes de una SKU pública.

![Indicar la reutilización de imágenes](./media/cloud-partner-portal-publish-virtual-machine/selectimage1.png)

Después de confirmar que la SKU reutiliza las imágenes, seleccione la SKU de origen o de *base* para las imágenes:

![Seleccionar una imagen](./media/cloud-partner-portal-publish-virtual-machine/selectimage2.png)

Al publicar la oferta, las imágenes de la SKU seleccionada estarán disponibles en el identificador de la SKU privada con las tarifas y condiciones personalizadas. La SKU privada solo será visible para la audiencia de destino.

Para las actualizaciones de imágenes, solo tendrá que actualizar la imagen de la SKU subyacente. En segundo plano, la imagen de la SKU privada también se actualizará automáticamente. De forma similar, si elimina la imagen de la SKU subyacente, esta también se quitará de la SKU privada.

<a name="restricting-the-audience"></a>Restringir la audiencia
------------------------

Las ofertas privadas solo las pueden buscar e implementar los usuarios de destino.
Actualmente, permitimos dirigirse a los usuarios a través de los identificadores de suscripción.

Estas suscripciones se pueden introducir a través de un formulario de entrada manual con un límite **de hasta 10 suscripciones**, o bien mediante la carga de un archivo CSV, que permite **hasta 20 000 suscripciones**.

Entrada manual para un público restringido:

![Restringir la audiencia manualmente](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience1.png)

Carga de CSV para un público restringido:

![Usar un archivo CSV para restringir la audiencia](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience2.png)

Contenido del archivo CSV de ejemplo:

            Type,Id,Description
            SubscriptionId,7738d703-3135-4e8d-8b81-1e70379abd9d,Private Customer

Al cambiar de la vista de entrada manual a la de carga de CSV o de la entrada de CSV a la entrada manual, no se conserva la lista antigua de identificadores de suscripción con acceso a la SKU. Se muestra una advertencia y solo se sobrescribe la lista al guardar la oferta.

<a name="managing-private-audiences"></a>Administración de audiencias privadas
-------------------------

**Para actualizar la audiencia sin volver a publicar toda la oferta, realice los cambios de audiencia que quiera (mediante la interfaz de usuario o la API) y, a continuación, inicie la acción "Sincronizar audiencias privadas".**

Si la audiencia es de 10 o menos suscripciones, puede administrarla completamente mediante la interfaz de usuario de CPP.

Si su audiencia tiene más de 10 suscripciones, puede administrarla mediante un archivo CSV que puede cargar en la interfaz de usuario de CPP o con la API.

Si usa la API y no quiere mantener un archivo CSV, puede administrar al público directamente mediante la API según las instrucciones a continuación.

> [!NOTE]
> Use un id. de suscripción de Azure (planes y SKU) o el id. de inquilino (solo planes) para agregar la audiencia a la oferta privada.

###  <a name="managing-subscriptions-with-the-api"></a>Administración de suscripciones con la API

Puede usar la API para cargar un archivo CSV o administrar la audiencia directamente (sin usar un archivo CSV). En general, solo tiene que recuperar la oferta, actualizar el objeto `restrictedAudience` y, a continuación, enviar los cambios de vuelta a la oferta para agregar o quitar miembros de la audiencia.

A continuación se indica cómo actualizar la lista de audiencias mediante programación:

1. [Recupere los datos de la oferta](cloud-partner-portal-api-retrieve-specific-offer.md):

    ```
    GET https://cloudpartner.azure.com/api/publishers//offers/?api-version=2017-10-31&includeAllPricing=true
    ```

2. Busque objetos de audiencia restringidos en cada SKU de la oferta mediante esta consulta JPath:

    ```
    $.definition.plans[*].restrictedAudience
    ```
3. Actualice los objetos de audiencia restringidos para su oferta.

    **Si originalmente cargó la lista de suscripciones para la oferta privada desde un archivo CSV:**

    Los objetos *restrictedAudience* tendrán el siguiente aspecto.
    ```
    "restrictedAudience": {
                  "uploadedCsvUri": "{SasUrl}"
    }
    ```

    Para cada objeto de audiencia restringida:

    a. Descargue el contenido de `restrictedAudience.uploadedCsvUri`. El contenido es simplemente un archivo CSV con encabezados. Por ejemplo:

        type,id,description
        subscriptionId,541a269f-3df2-486e-8fe3-c8f9dcf28205,sub1
        subscriptionId,c0da499c-25ec-4e4b-a42a-6e75635253b9,sub2

    b. Agregue o elimine las suscripciones en el archivo CSV descargado según sea necesario.

    c. Cargue el archivo CSV actualizado en una ubicación, como [Azure Blob Storage](../../storage/blobs/storage-blobs-overview.md) o [OneDrive](https://onedrive.live.com), y cree un vínculo de solo lectura en el archivo. Este será la nueva *SasUrl*.

    d. Actualice la clave `restrictedAudience.uploadedCsvUri` con la nueva *SasUrl*.

    **Si especificó manualmente la lista original de suscripciones de la oferta privada en Cloud Partner Portal:**

    Los objetos *restrictedAudience* tendrán un aspecto similar al siguiente:

    ```
    "restrictedAudience": {
        "manualEntries": [{
            "type": "subscriptionId",
            "id": "541a269f-3df2-486e-8fe3-c8f9dcf28205",
            "description": "sub1"
            }, {
            "type": "subscriptionId",
            "id": "c0da499c-25ec-4e4b-a42a-6e75635253b9",
            "description": "sub2"
            }
        ]}
    ```

    a. Para cada objeto de audiencia restringido, agregue o elimine las entradas en la lista `restrictedAudience.manualEntries` según sea necesario.

4. Cuando termine de actualizar todos los objetos *restrictedAudience* para cada SKU de la oferta privada, [actualice la oferta](cloud-partner-portal-api-creating-offer.md):

    ```
    PUT https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31
    ```
    Gracias a ello, la lista de audiencias actualizada ahora está en vigor.

<a name="previewing-private-offers"></a>Vista previa de las ofertas privadas
-------------------------

Durante el paso de previsualización o almacenamiento provisional, solo podrán acceder a la SKU las suscripciones de versión preliminar de nivel de la oferta. En esta fase de prueba puede obtener una vista previa de la oferta tal y como aparecería para los clientes de destino.

Suscripciones de versión preliminar de nivel de la oferta para acceder a ofertas almacenadas temporalmente:

![Preview Subscription Ids (Id. de suscripción de versión preliminar)](./media/cloud-partner-portal-publish-virtual-machine/previewoffer1.png)

Cuando la oferta esté activa, solo las suscripciones de audiencia restringida (especificadas mediante entrada manual o CSV) podrán ver e implementar la SKU privada. Se recomienda **incluir siempre sus propias suscripciones en la audiencia restringida** para la SKU privada con fines de validación.

>[!NOTE]
>Con fines de depuración, los equipos de soporte técnico e ingeniería de Microsoft también tendrán acceso a estas ofertas privadas.
