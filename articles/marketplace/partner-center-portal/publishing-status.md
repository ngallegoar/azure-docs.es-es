---
title: Comprobación del estado de publicación de una oferta de Marketplace comercial
description: Compruebe el estado de la validación, la certificación y los pasos de versión preliminar necesarios para publicar una oferta a través de Marketplace comercial en el centro de partners de Microsoft.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 012a574887d9980e0c71c3af84ff70ca8d31312c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275686"
---
# <a name="check-the-publishing-status-of-your-commercial-marketplace-offer"></a>Comprobación del estado de publicación de una oferta de Marketplace comercial

Puede ver las ofertas existentes en la pestaña **Publishing status** (Estado de publicación) de la pestaña **Información general de la oferta** del [portal de Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) del centro de partners.

Uno de los siguientes indicadores de estado se debe mostrar para cada oferta.

| **Estado**    | **Descripción**  |
| :---------- | :-------------------|
| **Draft** | La oferta se ha creado pero no se va a publicar. |
| **Publicación en curso** | Se están realizando los pasos del proceso de publicación para la oferta o el plan. |
| **Atención necesaria** | Se ha detectado un problema crítico durante la certificación de Microsoft o en cualquiera de los pasos de publicación. |
| **Versión preliminar** | Microsoft ha certificado la oferta y está en espera de la comprobación final del anunciante. Seleccione Transmitir para lanzar la oferta. |
| **Publicación** | La oferta está activa en Marketplace y los clientes la pueden ver y adquirir. |
| **Interrupción de venta pendiente** | El anunciante ha seleccionado "Stop sell" (Detener venta) en la oferta o el plan, pero aún no se ha completado la acción. |
| **Not available in the marketplace** (No disponible en Marketplace) | Una oferta o un plan publicados anteriormente se han quitado de Marketplace. |

## <a name="automated-validation"></a>Validación automatizada

El primer paso del proceso de publicación es realizar un conjunto de validaciones automatizadas. Cada paso de validación corresponde a una característica que eligió para habilitar la creación de la oferta. Si esa característica no se ha habilitado, la validación salta hasta el siguiente paso de publicación. Para que se apruebe el estado de publicación deben completarse todas las comprobaciones de validación.

- **Configuración del flujo de compra de la oferta (<10 min)**

En este paso nos aseguramos de que la oferta se cumple cuando los clientes la compran mediante Azure Portal. Este paso solo es aplicable para las ofertas de venta mediante Microsoft.

- **Validación de los datos de la versión de prueba (~5 min)**

En este paso validamos los datos que proporcionó en la sección de configuración técnica de la versión de prueba de la oferta. La funcionalidad de la versión de prueba se ha probado y aprobado. Este paso solo es aplicable para las ofertas con una versión de prueba habilitada.

- **Aprovisionamiento de la versión de prueba (~30 min)**

En este paso, después de validar los datos y la funcionalidad de la versión de prueba en el paso anterior, implementamos y replicamos las instancias de la versión de prueba para que estén listas para que las usen los clientes.  Este paso solo es aplicable para las ofertas con una versión de prueba habilitada.

- **Registro y validación de la administración de clientes potenciales (<15 min)**

En este paso confirmamos que su sistema de administración de clientes potenciales puede recibir clientes potenciales en función de los detalles proporcionados en la configuración de la oferta. Este paso solo es aplicable para las ofertas con la administración de clientes potenciales habilitada.

## <a name="certification"></a>Certificación

Antes de que se publiquen, las ofertas enviadas a Marketplace comercial del centro de partners se deben certificar. Las ofertas enviadas se someten a pruebas rigurosas, unas automatizadas y otras manuales, como una comprobación de cumplimiento de [Azure Marketplace certification policies](https://docs.microsoft.com/legal/marketplace/general-policies) (Directivas de certificación de Azure Marketplace). Las ofertas enviadas deben marcarse como aptas para la certificación para que puedan continuar al siguiente paso del flujo de publicación.

### <a name="types-of-validation-that-take-place-during-certification"></a>Tipos de validación que se realizan durante la certificación

El proceso de certificación incluye tres niveles de validación para cada oferta enviada.

- Aptitud de la empresa anunciante
- Validación del contenido
- Validación técnica

#### <a name="publisher-business-eligibility"></a>Aptitud de la empresa anunciante

Con cada tipo de oferta se comprueba un conjunto de criterios de aptitud básicos que el anunciante debe cumplir. Estos pueden incluir el estado de MPN del anunciante, las competencias y su grado, etc.

#### <a name="content-validation"></a>Validación del contenido

Durante la validación del contenido se comprueba la calidad y pertinencia de la información que escribió al crear la oferta. Con estas comprobaciones se revisan las entradas de los detalles del anuncio en Marketplace, los precios, la disponibilidad, los planes asociados, etc. Para cumplir los criterios de los anuncios de Azure Marketplace o AppSource, validamos que la oferta incluya:

- un título que describa la oferta con precisión;
- descripciones bien escritas que proporcionen información general detallada y una propuesta de valor;
- capturas de pantalla de calidad y vídeos adjuntos, y
- una explicación de cómo la oferta usa las plataformas y las herramientas de Microsoft.

Para más información sobre los criterios de validación del contenido, consulte las [directivas generales de los anuncios](https://docs.microsoft.com/legal/marketplace/certification-policies#100-general).

#### <a name="technical-validation"></a>Validación técnica

Durante la validación técnica, la oferta (paquete o binario) se somete a las siguientes comprobaciones.
- Examen en busca de malware
- Monitorización de las llamadas de red
- Análisis de los paquetes
- Análisis exhaustivo de la funcionalidad real de la oferta

La oferta se prueba en varias plataformas y con distintas versiones para garantizar su solidez.

Revise los detalles de configuración específicos necesarios para la oferta en la sección de configuración técnica de este documento.

### <a name="certification-failure-report"></a>Informe de error de certificación

Tras la finalización de la revisión, si la oferta ha pasado la certificación, continúa al paso siguiente del proceso de publicación. Si se ha producido algún error en las comprobaciones del anuncio, técnicas o de las directivas, o si no es apto para enviar una oferta de ese tipo, se genera un informe de error de certificación, que se le envía por correo electrónico.

Este informe contiene descripciones de las directivas con error, junto con notas de revisión. Revise el informe del correo electrónico, aborde los problemas mediante la actualización de la oferta como sea necesario y vuelva a enviar esta a través del [portal de Marketplace comercial](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) del centro de partners. (Puede volver a enviar las ofertas las veces que sea necesario hasta que se supere la certificación).

## <a name="preview-creation"></a>Creación de versión preliminar

Durante el paso **Creación de versión preliminar** creamos una versión de la oferta accesible solo a la audiencia que especificó en la sección de versión preliminar de esta.

>[!Note]
> No utilice este paso para otorgar visibilidad sobre la oferta a personas ajenas a la organización. En su lugar, use la opción **Oferta privada**. En este momento, la oferta no se ha probado y validado por completo y no está lista para distribuirse fuera de la organización.

## <a name="publisher-approval"></a>Aprobación del anunciante

En este paso le enviaremos un correo electrónico con una solicitud para que revise y apruebe la versión preliminar de la oferta antes de pasar al paso final de publicación.

Si ha seleccionado vender la oferta mediante Microsoft, podrá probar la adquisición y la implementación de esta para asegurarse de que cumple sus requisitos durante esta etapa de aprobación del anunciante. La oferta no estará aún disponible en Marketplace público. Después de probar y aprobar esta versión preliminar deberá seleccionar **Transmitir** en el panel [**Información general de la oferta**](https://partner.microsoft.com/dashboard/commercial-marketplace/overview).

Si desea realizar cambios en la oferta durante esta fase de versión preliminar, puede editar y volver a enviar la oferta para una nueva versión preliminar. Consulte la sección [Actualización de ofertas existentes en Marketplace](#update-existing-marketplace-offers) para más información sobre otros cambios.

Si la oferta ya está activa y disponible para el público en Marketplace, las actualizaciones que realice no se activarán hasta que seleccione **Transmitir** en el panel [**Información general de la oferta**](https://partner.microsoft.com/dashboard/commercial-marketplace/overview).

### <a name="publish-offer-to-the-public"></a>Publicación de la oferta para el público

Inicie sesión en el centro de partners y acceda a la oferta. Se le redirigirá a la página **Información general de la oferta**. En la parte superior de esta página verá la opción **Transmitir**. Seleccione **Transmitir** y, después de la confirmación, la oferta empezará a publicarse para el público. Cuando la oferta esté activa, recibirá una notificación por correo electrónico.

## <a name="publish"></a>Publicar

Ahora que ha seleccionado **Transmitir** la oferta al hacer que esté disponible en Marketplace, hay una serie de comprobaciones de validación finales que deben realizarse para garantizar que la oferta activa está configurada igual que en la versión preliminar.

- **Configuración del flujo de compra de la oferta (>10 min)**

En este paso nos aseguramos de que la oferta se cumple cuando los clientes la compran mediante Azure Portal. Este paso solo es aplicable para las ofertas de venta mediante Microsoft.

- **Validación de los datos de la versión de prueba (~5 min)**

En este paso validamos los datos que proporcionó en la sección de configuración técnica de la versión de prueba de la oferta. La funcionalidad de la versión de prueba se ha probado y aprobado. Este paso solo es aplicable para las ofertas con una versión de prueba habilitada.

- **Aprovisionamiento de la versión de prueba (~30 min)**

En este paso implementamos y replicamos instancias de la versión de prueba para que estén listas para que las usen los clientes.  Este paso solo es aplicable para las ofertas con una versión de prueba habilitada.

- **Registro y validación de la administración de clientes potenciales (>15 min)**

En este paso confirmamos que su sistema de administración de clientes potenciales puede recibir clientes potenciales en función de los detalles proporcionados en la configuración de la oferta. Este paso solo es aplicable para las ofertas con la administración de clientes potenciales habilitada.

- **Publicación final (>30 minutos)**

En este paso nos aseguramos de que la oferta esté públicamente disponible en Marketplace.

## <a name="update-existing-marketplace-offers"></a>Actualización de ofertas existentes en Marketplace

Si desea realizar cambios en una oferta que ya se ha publicado, deberá actualizar primero la oferta existente y volver a publicarla.

## <a name="next-steps"></a>Pasos siguientes

- [Actualización de una oferta existente en Marketplace comercial](./update-existing-offer.md)
