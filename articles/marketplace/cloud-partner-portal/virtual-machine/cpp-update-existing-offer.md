---
title: Actualización de una oferta de VM existente en Azure Marketplace
description: Se explica cómo actualizar una oferta de máquina virtual existente en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: dsindona
ms.openlocfilehash: fe1397d7ad00a2eb6e9fe04d8bfa50e49839fe34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288791"
---
# <a name="update-an-existing-vm-offer-on-azure-marketplace"></a>Actualización de una oferta de máquina virtual existente en Azure Marketplace

Este artículo le guía por los diferentes aspectos de la actualización de una oferta de máquina virtual (VM) en [Cloud Partner Portal](https://cloudpartner.azure.com/) y de la nueva publicación de la oferta. 

Hay una serie de motivos comunes para actualizar la oferta, incluidos los siguientes:

-  Agregar una nueva versión de la imagen de máquina virtual a las SKU existentes
-  Cambiar las regiones donde hay una SKU disponible
-  Agregar SKU nuevas
-  Actualizar los metadatos de Marketplace de la oferta o las SKU individuales
-  Actualizar los precios de las ofertas de pago por uso

Para ayudarle con estas modificaciones, el portal ofrece las características **Comparar** e **Historial**.  

>[!Note]
>La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible.  Consulte [Proveedores de soluciones en la nube](../../cloud-solution-providers.md) para obtener más información sobre el marketing que ofrece a través de los canales de asociados de CSP de Microsoft.

## <a name="unpermitted-changes-to-vm-offer-or-sku"></a>Cambios no permitidos en la oferta de máquina virtual o la SKU

Hay algunos atributos de una oferta o SKU de máquina virtual que no se pueden modificar una vez que la oferta está publicada en Azure Marketplace, principalmente los siguientes:

-  **Id. de oferta** e **Id. de publicador** de la oferta.
-  **Id. de SKU** de las SKU existentes.
-  El recuento de discos de datos de las SKU existentes.
-  Cambios en el modelo de facturación o licencia en las SKU existentes.
-  Aumentos de precio en una SKU publicada.


## <a name="common-update-operations"></a>Operaciones de actualización comunes

Aunque hay una amplia gama de características que se pueden cambiar en una oferta de máquina virtual, las operaciones siguientes son comunes.

### <a name="update-the-vm-image-version-for-a-sku"></a>Actualización de la versión de imagen de la máquina virtual para una SKU

Es habitual que una imagen de máquina virtual se actualice periódicamente con revisiones de seguridad, características adicionales, etc.  En estos escenarios, le interesa actualizar la imagen de máquina virtual a la que hace referencia la SKU mediante los pasos siguientes:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que se va a actualizar.

3.  En la pestaña **SKU**, haga clic en la SKU asociada a la imagen de máquina virtual que se va a actualizar.

4.  En **Versión de disco**, haga clic en **+Nueva versión de disco** para agregar una nueva imagen de la VM.

5.  Proporcione la **versión de disco** de las imágenes nuevas de la VM. La versión del disco debe seguir el formato de [versión semántica](https://semver.org/). Las versiones deben ser del tipo X.Y.Z, donde X, Y y Z son números enteros. Compruebe que la versión nueva que proporciona es mayor que todas las versiones anteriores; en caso contrario, después de volver a publicar la versión nueva no se mostrará en el portal o Azure Marketplace.

6.  En **Dirección URL del VHD del OS**, escriba el [URI de firma de acceso compartido (SAS)](./cpp-get-sas-uri.md) creado para el disco duro virtual del sistema operativo. 

    > [!WARNING] 
    > No se puede cambiar el recuento de discos de datos entre diferentes versiones de la SKU. Si las versiones anteriores tenían discos de datos configurados, esta versión nueva también debe tener el mismo número de discos de datos.

7.  Haga clic en **Publicar** para iniciar el flujo de trabajo para publicar la nueva versión de la máquina virtual en Azure Marketplace.


### <a name="change-region-availability-of-a-sku"></a>Cambio de la disponibilidad de región de una SKU

Con el tiempo, puede querer que su oferta/SKU esté disponible en más regiones.  O bien, puede que desee dejar de ofrecer la oferta/SKU en una región determinada.
Para modificar la disponibilidad, siga estos pasos:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que quiera actualizar.

3.  En la pestaña **SKU**, haga clic en la SKU de la que quiera modificar la disponibilidad.

4.  Haga clic en el botón **Seleccionar países** situado bajo el campo **Disponibilidad en país/región**.

5.  En el menú emergente de disponibilidad por región, agregue o elimine las regiones para esta SKU.

6.  Haga clic en **Publicar** para iniciar el flujo de trabajo de publicación para actualizar la disponibilidad por región de las SKU.

Si una SKU se pone a disposición en una región nueva, tendrá la posibilidad de especificar el precio para esa región determinada a través de la funcionalidad **Exportar datos de precios**. Si va a agregar de nuevo una región que ya estaba disponible anteriormente, no podrá actualizar su precio ya que no se permiten cambios en los precios.


### <a name="add-a-new-sku"></a>Agregar una nueva SKU

Siga los pasos siguientes para poner una nueva SKU a disposición de la oferta existente: 

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que quiera actualizar.

3.  En la pestaña **SKU**, haga clic en **Agregar nueva SKU** y proporcione un **ID de SKU** en el menú emergente.

4.  Vuelva a publicar la máquina virtual como se describe en el artículo [Publicación de una máquina virtual en Azure Marketplace](./cpp-publish-offer.md).

5.  Haga clic en **Publicar** para iniciar el flujo de trabajo para publicar la nueva SKU.


### <a name="update-offer-marketplace-metadata"></a>Actualizar los metadatos de una oferta de Marketplace

Siga los pasos siguientes para actualizar los metadatos de Marketplace (nombre de la empresa, logotipo, etc.) asociados con la oferta: 

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que quiera actualizar.

3.  Vaya a la pestaña **Marketplace** y, después, siga las instrucciones del artículo [Publicación de una máquina virtual en Azure Marketplace](./cpp-publish-offer.md) para realizar cambios en los metadatos.

4.  Haga clic en **Publicar** para iniciar el flujo de trabajo para publicar los cambios.


### <a name="update-pricing-on-published-offers"></a>Actualización de precios en las ofertas publicadas

Una vez publicada la oferta de pago por uso, no puede aumentar directamente los precios de la SKU.  (Pero puede crear una SKU en la misma oferta, eliminar la SKU antigua y, después, volver a publicar la oferta para nuevos clientes).  Por el contrario, puede reducir el precio de una oferta publicada mediante los pasos siguientes:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  En **Todas las ofertas**, busque la oferta que se va a actualizar.

3.  Haga clic en la SKU cuyo precio quiere bajar.

4.  Si ha establecido el precio en la GUI 1x1, puede cambiarlo directamente en la interfaz de usuario. Si establece los precios a través de la hoja de cálculo de importación o exportación, solo puede bajarlos mediante la característica de importación o exportación.

3.  Haga clic en **Save**(Guardar).

4.  Haga clic en **Publicar** para iniciar el flujo de trabajo para publicar los cambios.

El nuevo precio reducido será visible para los clientes nuevos una vez que se publique en el sitio web.  Este precio nuevo afectará a los clientes de las maneras siguientes:

- A los clientes nuevos se les cobrará esta tarifa nueva. 
- Para los clientes existentes, la reducción de precio se reflejará con carácter retroactivo al principio del ciclo de facturación en el que entró en vigor la reducción de precio.
Si ya se les ha facturado el ciclo durante el que se produjo la reducción de precio, recibirán un reembolso durante el ciclo de facturación siguiente para cubrir el precio reducido.


<!-- TD: This has been implemented, need to change the SKU Tab topic to reflect and move this section there. -->
### <a name="simplified-currency-pricing"></a>Precios en moneda simplificada

A partir del 1 de septiembre de 2018, se agrega al portal una nueva sección denominada **Precios en moneda simplificada**. Microsoft está simplificando el negocio de Azure Marketplace mediante la habilitación de precios más predecibles y colecciones de sus clientes en todo el mundo. Esta optimización incluirá la reducción del número de monedas en que se factura a los clientes.

En la sección nueva se mostrarán los precios en estas nuevas monedas. Una vez migrados todos los clientes a estas nuevas monedas de liquidación, la sección de precios original se retirará y solo se conservará la sección "Precios en moneda simplificada".

Hasta el 1 de noviembre de 2018 podrá establecer un nuevo precio para las regiones donde se esté modificando la moneda de liquidación. No podrá subir el precio para las regiones donde no cambie la moneda de liquidación.

> [!NOTE] 
> Si usa API para publicar la oferta, es posible que vea una sección nueva en el código JSON de la oferta. Esta podría estar anotada como `virtualMachinePricingV2` o `monthlyPricingV2`, según el tipo de oferta. 

Si tiene alguna pregunta sobre este cambio, póngase en contacto con el [servicio de soporte técnico de Azure Marketplace](../../support-azure-marketplace.md).


## <a name="compare-feature"></a>Característica Comparar

Al realizar cambios en una oferta ya publicada, puede aprovechar la característica **Comparar** para auditar los cambios realizados. Para usar esta característica:

1.  En cualquier punto del proceso de edición, haga clic en el botón **Comparar** de la oferta.

    ![Botón de la característica Comparar](./media/publishvm_037.png)


2.  Vea las versiones en paralelo de los recursos de marketing y metadatos.


## <a name="history-of-publishing-actions"></a>Historial de acciones de publicación

Para ver la actividad de publicación histórica, haga clic en el elemento **Historial** en la barra de menús de navegación de la izquierda de Cloud Partner Portal. Aquí podrá ver las acciones con marca de tiempo que se han realizado durante la vigencia de las ofertas de Azure Marketplace.  
<!-- TD: Add after section authored: For more information, see [History page](../portal-tour/cpp-history-page.md). -->

