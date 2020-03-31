---
title: Actualización de las ofertas de Marketplace | Azure Marketplace
description: Actualización de las ofertas de Marketplace de Azure y AppSource mediante Cloud Partner Portal
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: dsindona
ms.openlocfilehash: 101369a050770be3acd9534cef6229037fe1c366
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288519"
---
# <a name="update-azure-marketplace-and-appsource-offers"></a>Actualización de las ofertas de Azure Marketplace y AppSource

Hay varios tipos de actualizaciones que puede aplicar a su oferta una vez publicada.  [Cloud Partner Portal](https://cloudpartner.azure.com/) le ayuda a modificar correctamente los atributos de una oferta, por ejemplo, a:

-  Agregar una nueva imagen de máquina virtual (VM) o versión de paquete a una SKU existente.
-  Cambiar las regiones donde existe una SKU disponible.
-  Agregar nuevas SKU.
-  Actualizar los metadatos del Marketplace para las SKU o las ofertas. 
-  Actualizar precios e ofertas de pago por uso.

El portal también incluye características, como la posibilidad de comparar características y de ver un historial de aquellas que corresponden a una oferta, que le ayudan a administrar los cambios.  Después de modificar una oferta o una SKU, se deben volver a publicar antes de que se activen los cambios.  Este artículo le guía por los distintos aspectos de la actualización de la oferta de Marketplace.

## <a name="unpermitted-changes-to-an-offersku"></a>Cambios no permitidos en una oferta o una SKU

Hay algunos atributos de una oferta o una SKU que no pueden modificarse una vez que se han publicado en Marketplace.  Los campos correspondientes están deshabilitados en la pestaña **Editor** del portal, por ejemplo:  

- Offer ID (Id. de oferta) e Publisher ID (Id. de publicador)
- Identificador de SKU 
- El recuento de discos de datos de las SKU existentes.
- Cambios en el modelo de facturación o licencia en las SKU existentes
- Etiquetas de versión, por ejemplo: `1.0.1`


## <a name="common-update-operations"></a>Operaciones de actualización comunes

En las siguientes secciones se explica cómo realizar algunas de las operaciones de actualización.  Estas operaciones no están disponibles para todos los tipos de oferta.  Para iniciar cualquiera de estas operaciones, debe iniciar sesión en Cloud Partner Portal.


### <a name="update-offer-contacts"></a>Actualización de los contactos de la oferta

Siga los pasos a continuación para actualizar los contactos de soporte técnico de la oferta.
1. En la página **All Offers** (Todas las ofertas), seleccione la oferta.
2. Seleccione la pestaña **Contacts** (Contactos). Actualice los contactos.
3. Seleccione el botón **Guardar**.
4. Seleccione **Publicar** para iniciar el proceso de publicación.


### <a name="change-regions-an-offer-or-sku-is-available-in"></a>Cambio de las regiones en las que están disponible las ofertas o las SKU

Con el tiempo, puede querer que su oferta/SKU esté disponible en más regiones.
O bien, puede que desee dejar de ofrecer la oferta/SKU en una región determinada.
Para implementar estos cambios, realice los pasos siguientes.

1. En la página **All offers** (Todas las ofertas), busque aquella que quiere actualizar.

Para las ofertas de Azure Marketplace:

1. Seleccione la pestaña **SKU**.  Seleccione la SKU que quiere modificar.
1. Haga clic en el botón **Select Countries** (Seleccionar países) bajo el campo **Country/Region availability** (Disponibilidad de país o región).
1. En el cuadro de diálogo de disponibilidad de regiones, agregue o elimine las regiones para esta SKU.

Para las ofertas de AppSource:

1. Seleccione la pestaña **Storefront Details** (Detalles del escaparate).
1. Junto a la etiqueta **Supported countries/regions** (Regiones o países admitidos), haga clic en **Supported countries/regions** (Regiones o países admitidos). 
1. En el cuadro de diálogo de regiones o países admitidos, agregue o quite las regiones correspondientes a esta oferta.

En cualquiera de los Marketplace:

1. Seleccione **Publish** (Publicar) para iniciar el proceso de publicación. 

Si una SKU se vuelve disponible en una región nueva, tiene la posibilidad de especificar el precio para esa región determinada a través de la funcionalidad **Export Pricing Data** (Exportar datos de precios). Si agrega de nuevo una región que ya estaba disponible anteriormente, no podrá actualizar su precio ya que no se permiten cambios en los precios.


### <a name="add-a-new-sku"></a>Agregar una nueva SKU 

Para que una nueva SKU esté disponible para una oferta existente, use los pasos siguientes:

1. En la página **All offers** (Todas las ofertas), busque la oferta.
3. En el formulario **SKUs** (SKU), haga clic en **Add new SKU** (Agregar nueva SKU) y proporcione un valor para **SKU ID** (Id. de SKU) en la ventana emergente.
4. Siga el resto de los pasos que se detallan en [Publicación de una oferta de máquina virtual](../virtual-machine/cpp-publish-offer.md).
5. Seleccione **Publish** (Publicar) para iniciar el proceso de publicación.


### <a name="update-offer-marketplace-assets"></a>Actualización de los recursos de Marketplace de ofertas

Puede que se encuentre con escenarios en los que deba actualizar los recursos de texto e imágenes de Marketplace, como los logotipos de empresa, la descripción de la oferta, etc. Para actualizar estos recursos, siga estos pasos:

1. En la página **All offers** (Todas las ofertas), busque su oferta. 
2. Seleccione la pestaña **Marketplace** y siga las instrucciones del tema *Marketplace tab* (Pestaña Marketplace) de la oferta.
3. Seleccione **Publish** (Publicar) para iniciar el proceso de publicación.


### <a name="update-pricing-on-published-offers"></a>Actualización de precios en las ofertas publicadas

Una vez publicada la oferta de pago por uso, no puede aumentar el precio de una SKU existente.  En su lugar, cree una SKU en la misma oferta, elimine la SKU antigua y, luego, vuelva a publicar su oferta. Puede reducir el precio en las ofertas publicadas anteriormente. Para bajar el precio de oferta:

1. Seleccione la SKU cuyo precio quiere bajar.
2. Debe establecer el precio inferior mediante el mismo mecanismo que usó originalmente: bien directamente en la interfaz de usuario del portal o con la hoja de cálculo de importación y exportación.
3. Haga clic en **Save**(Guardar).
4. Seleccione **Publish** (Publicar) para iniciar el proceso de publicación.

Los precios serán visibles para los clientes nuevos una vez que se activen en Marketplace, y todos ellos pagarán el nuevo precio reducido.  Para los clientes existentes, la reducción de precio se reflejará con carácter retroactivo al principio del ciclo de facturación en el que entró en vigor la reducción de precio.  Si ya se les ha facturado el ciclo durante el que se produjo la reducción de precio, recibirán un reembolso durante el ciclo de facturación siguiente para cubrir el precio reducido.


## <a name="compare-feature"></a>Característica Comparar

Al realizar cambios en una oferta publicada, puede usar la característica *Compare* (Comparar) para auditar los cambios. Para usar esta característica:

1. En cualquier punto del proceso de edición, puede hacer clic en el botón **Compare** (Comparar) en la pestaña **Editor** de la oferta.
2. Una ventana de comparación muestra las versiones en paralelo de los cambios guardados en esta oferta en comparación con la oferta de Marketplace. 

![Botón de comparación de ofertas en la pestaña Editor](./media/offer-compare-button.png)


## <a name="history-of-publishing-actions"></a>Historial de acciones de publicación

Para ver la actividad histórica de publicación, seleccione la pestaña **History** (Historial) en la barra de menús vertical izquierda de Cloud Partner Portal.  La página del historial proporciona el filtrado flexible por varias características y admite la ordenación de columnas.  Cada evento de publicación lleva la marca de tiempo.  Para más información, consulte la [página del historial de auditoría](../portal-tour/cpp-history-page.md).


## <a name="next-steps"></a>Pasos siguientes

También puede usar Cloud Partner Portal para [eliminar una SKU o una oferta publicadas](./cpp-delete-offer.md).
