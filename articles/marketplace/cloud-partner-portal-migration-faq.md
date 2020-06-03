---
title: Preguntas comunes sobre la migración al Centro de partners | Marketplace comercial de Microsoft
description: Respuestas a las preguntas comunes sobre la migración de ofertas de Cloud Partner Portal al Centro de partners.
author: anbene
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mingshen
ms.openlocfilehash: c4fdde94ea703d194e2de27a8df429f62ea374f4
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727483"
---
# <a name="common-questions-about-migrating-from-the-cloud-partner-portal-to-partner-center"></a>Preguntas comunes sobre la migración de Cloud Partner Portal al Centro de partners

En este artículo se abordan las preguntas más frecuentes sobre la migración de ofertas de Cloud Partner Portal al Centro de partners.

## <a name="what-does-offer-migration-mean"></a>¿Qué significa "migración de ofertas"?

Estamos trasladando los datos de las ofertas desde Cloud Partner Portal al Centro de partners con cambios en la experiencia de administración y publicación de ofertas.

| Área  | Cambios  |
|-------|----------|
| **Experiencia de administración de ofertas y publicación** | Podrá disfrutar de una experiencia de usuario mejorada con una interfaz intuitiva en el Centro de partners. Para obtener más información, consulte [¿Cuáles son las diferencias entre el Centro de partners y Cloud Partner Portal?](#what-are-the-differences-between-partner-center-and-the-cloud-partner-portal) |
| **Disponibilidad de las ofertas en Marketplace** | Sin cambios. Si su oferta está activa en Marketplace, así permanecerá durante la migración y después de esta. |
| **Nuevas compras e implementaciones** | Sin cambios. Los clientes podrá seguir comprando e implementando sus ofertas sin interrupciones. |
| **Pagos** | Todas las compras e implementaciones que se produzcan durante la migración o después de esta se seguirán pagando de forma habitual. |
|**Integraciones de API con las [API de Cloud Partner Portal existentes](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview)** | Las API de Cloud Partner Portal existentes seguirán siendo compatibles después de la migración y las integraciones existentes seguirán funcionando. Para obtener más información, consulte [¿Se admitirán las API REST de Cloud Partner Portal después de la migración?](#will-the-cloud-partner-portal-rest-apis-be-supported-post-migration) |
| | |

## <a name="can-i-still-access-the-cloud-partner-portal-and-manage-my-offers-during-migration"></a>¿Puedo seguir accediendo a Cloud Partner Portal y administrar mis ofertas durante la migración?

Todas las ofertas se han migrado al Centro de partners. El período de migración de cada oferta debería haber sido inferior a 24 horas. Después de migrar la oferta, debería haber recibido una notificación por correo electrónico.

Una vez que se hayan migrado las ofertas, se encontrarán en modo de solo lectura **durante un período de tiempo limitado** en Cloud Partner Portal. Su estado reflejará el traslado al Centro de partners e incluirá un vínculo a la oferta en el Centro de partners, como se muestra en las capturas de pantalla siguientes. Desde este momento, administrará las actualizaciones de todas las ofertas o creará nuevas ofertas exclusivamente a través del Centro de partners.

:::image type="content" source="media/migration-faq/all-offers-2.png" alt-text="Muestra el mensaje proporcionado para las ofertas que se han migrado al Centro de partners.":::

:::image type="content" source="media/migration-faq/offer-has-moved.png" alt-text="Muestra la página Cloud Partner Portal para una oferta migrada.":::

## <a name="how-will-i-create-new-offers"></a>¿Cómo se crean nuevas ofertas?

En Cloud Partner Portal, se le dirigirá a crear nuevas ofertas en el Centro de partners.

:::image type="content" source="media/migration-faq/create-new-offer-1.png" alt-text="Muestra el menú para crear una nueva oferta en Cloud Partner Portal.":::

Después de seleccionar una nueva oferta, verá un mensaje como el siguiente.

:::image type="content" source="media/migration-faq/create-new-offer-2.png" alt-text="Muestra el mensaje recibido al crear una nueva oferta en CPP":::

## <a name="do-i-need-to-create-a-new-account-to-manage-offers-in-partner-center"></a>¿Es necesario crear una nueva cuenta para administrar las ofertas en el Centro de partners?

No. La cuenta subyacente se conservará, y ya debería estar administrándola en el Centro de partners. Esto significa que, si ya es un partner, podrá usar sus credenciales existentes de la cuenta de Cloud Partner Portal para iniciar sesión en el Centro de partners después de la migración. Solo se están trasladando las ofertas y la experiencia de administración asociada de Cloud Partner Portal al Centro de partners. Le pedimos que no cree ninguna cuenta nueva, ya que sus ofertas no se asociarán con ella.

## <a name="i-see-a-message-in-the-cloud-partner-portal-to-activate-my-account-what-does-this-mean"></a>Veo un mensaje en Cloud Partner Portal para activar mi cuenta, ¿qué significa esto?

Necesitamos que nos proporcione unos detalles más para migrar correctamente su cuenta al Centro de partners y que pueda administrar sus ofertas en el Centro de partners una vez completada la migración de la oferta. Para obtener más información sobre la activación de cuentas, consulte [Migración de cuentas de Cloud Partner Portal al Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/account-migration-from-cpp-to-pc).

Los pasos necesarios para completar la activación de la cuenta varían según el rol de la cuenta.

| Rol de la cuenta | Pasos de activación |
|--------------|----------------|
|Propietario | Vaya a la página [Perfil de anunciante](https://cloudpartner.azure.com/#profile) en Cloud Partner Portal y, a continuación, haga clic en el vínculo del banner para activar. Se le redirigirá al Centro de partners para completar la activación de la cuenta. |
| Colaborador | Solo un usuario de la cuenta con rol de propietario puede activar la cuenta. Póngase en contacto con los propietarios de la cuenta para completar su activación. Los propietarios de la cuenta deben aparecer en el mensaje del banner. |
| | |

## <a name="im-having-trouble-logging-in-to-my-account-and-opening-a-support-ticket"></a>Tengo problemas para iniciar sesión en mi cuenta y abrir una incidencia de soporte técnico.

Si no puede iniciar sesión en su cuenta, puede abrir una [incidencia de soporte técnico](https://partner.microsoft.com/support/v2/?stage=1).

## <a name="where-can-i-find-documentation-on-the-new-partner-center-publishing-experience"></a>¿Dónde puedo encontrar documentación sobre la nueva experiencia de publicación del Centro de partners?

Vaya a la [documentación comercial de Marketplace](https://docs.microsoft.com/azure/marketplace/). A continuación, expanda el **portal comercial de Marketplace en el Centro de partners**  > **Crear una nueva oferta** para ver los temas de ayuda para crear cada tipo de oferta.

:::image type="content" source="media/migration-faq/marketplace-help-topics.png" alt-text="Muestra los temas de ayuda del Centro de partners.":::

### <a name="what-are-the-differences-between-partner-center-and-the-cloud-partner-portal"></a>¿Cuáles son las diferencias entre el Centro de partners y Cloud Partner Portal?

Es posible que observe las siguientes diferencias entre Cloud Partner Portal y el Centro de partners.

### <a name="modular-publishing-capabilities"></a>Funcionalidades de publicación modular

El Centro de partners proporciona una opción de publicación modular que le permite seleccionar los cambios que desea publicar en lugar de publicar siempre todas las actualizaciones a la vez. Por ejemplo, en la siguiente captura de pantalla se muestra que los únicos cambios seleccionados para su publicación son los cambios en las **propiedades** y la **descripción de la oferta**.

:::image type="content" source="media/migration-faq/review-and-publish-migration.png" alt-text="Muestra la página de vista previa y publicación.":::

Las actualizaciones que no publique se guardan como borradores. Siga usando la vista previa de la oferta para revisar su oferta antes de hacerla pública.

### <a name="rich-text-format"></a>Formato de texto enriquecido

Mejore la descripción de la oferta y el plan mediante el editor de texto enriquecido en la página de descripción de la oferta y del plan.

:::image type="content" source="media/migration-faq/rich-text-editor-migration.png" alt-text="Muestra el editor de texto enriquecido.":::

### <a name="enhanced-preview-options"></a>Opciones de vista previa mejoradas

El Centro de partners incluye una [característica de comparación](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#compare-changes-to-marketplace-offers) con opciones de filtrado mejoradas. Esto le ofrece la posibilidad de comparar con la versión preliminar y las versiones publicadas de la oferta.

:::image type="content" source="media/migration-faq/enhanced-preview.png" alt-text="Muestra la característica de comparación.":::

### <a name="branding-and-navigation-changes"></a>Cambios de personalización de marca y navegación

Observará algunos cambios en la personalización de marca. Por ejemplo, las "SKU" aparecen como "Planes" en el Centro de partners.

:::image type="content" source="media/migration-faq/plan-overview-migration.png" alt-text="Muestra la información general del plan.":::

Además, la información que antes se proporcionaba en las páginas **Marketplace** o **Detalles del escaparate** (servicio de consultoría, aplicación Power BI) de Cloud Partner Portal se recopila en la página **Descripción de la oferta** del Centro de partners.

:::image type="content" source="media/migration-faq/offer-listing-migration.png" alt-text="Muestra la página de descripción de la oferta.":::

La información que se antes se proporcionaba para las SKU en una única página de Cloud Partner Portal ahora pueden recopilarse en varias páginas en el Centro de partners:

* Página de configuración del plan
* Página de descripción del plan
* Página de disponibilidad del plan
* Página de configuración técnica del plan, como se muestra en esta captura de pantalla.

:::image type="content" source="media/migration-faq/tech-config-migration.png" alt-text="Muestra la página Configuración técnica del plan.":::

El identificador de la oferta se muestra ahora en la barra de navegación izquierda de la oferta.

:::image type="content" source="media/migration-faq/offer-id-offer-overview.png" alt-text="Muestra el menú de navegación izquierdo con el identificador de la oferta.":::

### <a name="stop-selling-an-offer"></a>Detención de la venta de una oferta

Puede solicitar [dejar de vender una oferta](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan) en Marketplace directamente desde el portal del Centro de partners. La opción está disponible en la página **Información general de la oferta** de la oferta.

:::image type="content" source="media/migration-faq/stop-selling-migration.png" alt-text="Muestra la página Información general de la oferta con la opción de detener la venta.":::

## <a name="what-pages-in-partner-center-correspond-to-pages-i-used-in-the-cloud-partner-portal"></a>¿Qué páginas del Centro de partners corresponden a las páginas que usaba en Cloud Partner Portal?

En la tabla siguiente se muestran los vínculos correspondientes entre los dos portales.

| Página | Vínculo de Cloud Partner Portal | Vínculo de Centro de partners |
|------|---------------------------|---------------------|
| **Página Todas las ofertas** | https://cloudpartner.azure.com/#alloffers | https://partner.microsoft.com/dashboard/commercial-marketplace/overview |
| **Página Todos los publicadores** | https://cloudpartner.azure.com/#publishers | https://partner.microsoft.com/dashboard/account/v3/publishers/list |
| **Perfil de anunciante** | https://cloudpartner.azure.com/#profile | https://partner.microsoft.com/dashboard/account/management |
| **Página Usuarios** | https://cloudpartner.azure.com/#users | https://partner.microsoft.com/dashboard/account/usermanagement |
| **Página Historial** | https://cloudpartner.azure.com/#history | La característica de Historial todavía no se admite en el Centro de partners. |
| **Panel de información** | https://cloudpartner.azure.com/#insights | https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary |
| **Informe de pago** | https://cloudpartner.azure.com/#insights/payout | https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments |
| | |

## <a name="will-the-cloud-partner-portal-rest-apis-be-supported-post-migration"></a>¿Se admitirán las API REST de Cloud Partner Portal después de la migración?

Las API de Cloud Partner Portal se integran con el Centro de partners y seguirán funcionando después de migrar a él las ofertas. La integración presenta pequeños cambios. Revise los cambios en la tabla siguiente para asegurarse de que el código sigue funcionando después de la migración al Centro de partners.

| **API** | **Descripción del cambio** | **Impacto** |
| ------- | ---------------------- | ---------- |
| POST Publish, GoLive, Cancel | En el caso de las ofertas migradas, el encabezado de respuesta tendrá un formato diferente, pero continuará funcionando de la misma manera, lo que denota una ruta de acceso relativa para recuperar el estado de la operación. | Al enviar cualquiera de las solicitudes POST correspondientes para una oferta, el encabezado de ubicación tendrá alguno de los dos formatos disponibles, según el estado de migración de la oferta:<ul><li>Ofertas no migradas<br>`/api/operations/{PublisherId}${offerId}$2$preview?api-version=2017-10-31`</li><li>Ofertas migradas<br>`/api/publishers/{PublisherId}/offers/{offereId}/operations/408a4835-0000-1000-0000-000000000000?api-version=2017-10-31`</li> |
| Operación GET | En el caso de las ofertas que anteriormente admitían el campo "Dirección de correo electrónico para notificaciones" en la respuesta, este campo quedará en desuso y ya no se devolverá para las ofertas migradas. | En el caso de las ofertas migradas, ya no se enviarán notificaciones a la lista de correos electrónicos especificados en las solicitudes. En su lugar, el servicio de API se alineará con el proceso de correo electrónico de notificaciones del Centro de partners para enviar correos electrónicos. En concreto, las notificaciones se enviarán a la dirección de correo electrónico establecida en la sección de información de contacto del vendedor de la configuración de la cuenta en el Centro de partners, para notificarle el progreso de la operación.<br><br>Revise la dirección de correo electrónico establecida en la sección de información de contacto del vendedor de la [configuración de la cuenta](https://partner.microsoft.com/dashboard/account/management) en el Centro de partners para asegurarse de que se proporciona el correo electrónico correcto para las notificaciones.  |
| | | |
