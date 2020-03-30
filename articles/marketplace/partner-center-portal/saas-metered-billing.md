---
title: Facturación según uso con el servicio de medición de Marketplace | Azure Marketplace
description: Esta documentación es una guía para fabricantes de software independientes que publican ofertas de SaaS con modelos de facturación flexibles.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 8e5a4813301cbab16d1cffabaaa60688f6e826ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281330"
---
# <a name="metered-billing-using-the-marketplace-metering-service"></a>Facturación según uso con el servicio de medición de Marketplace

Con el servicio de medición de Marketplace, puede crear ofertas de software como servicio (SaaS) en el programa comercial de Marketplace que se cobran conforme a unidades no estándar.  Antes de publicar esta oferta, debe definir las dimensiones de facturación, como el ancho de banda, los vales o los correos electrónicos procesados.  A continuación, los clientes pagan en función del consumo que realicen de estas dimensiones, al tiempo que su sistema informa a Microsoft mediante la API del servicio de medición de Marketplace de los eventos facturables cuando se producen.  

## <a name="prerequisites-for-metered-billing"></a>Requisitos previos para la facturación según uso

Para que una oferta de SaaS use la facturación según uso, debe:

* Cumplir todos los requisitos de [una oferta de venta mediante Microsoft](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#sell-through-microsoft), tal y como se describe en [Creación de una oferta SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer).
* Integrarse con las [API de suministro de SaaS](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) para que los clientes puedan aprovisionar y conectarse a su oferta.  
* Configurarse para el modelo de precios de **tarifa plana** para cobrar a los clientes por su servicio.  Las dimensiones son una extensión opcional del modelo de precios de tarifa plana. 
* Integrarse con las [API del servicio de medición de Marketplace](./marketplace-metering-service-apis.md) para informar a Microsoft de eventos facturables.

>[!Note]
>El servicio de medición de Marketplace solo está disponible para el modelo de facturación de tarifa plana y no se aplica al modelo de facturación de usuario.

## <a name="how-metered-billing-fits-in-with-pricing"></a>Cómo encaja la facturación según uso con los precios

A la hora de definir la oferta junto con sus modelos de precios, es importante comprender la jerarquía de las ofertas.

* Cada oferta SaaS está configurada para venderse mediante Microsoft o no.  Esta configuración no puede modificarse una vez publicada una oferta.
* Las ofertas SaaS configuradas para venderse en Microsoft pueden tener uno o más planes. Un usuario se suscribe a la oferta SaaS, pero esta se adquiere en Microsoft en el contexto de un plan.
* Cada plan tiene un modelo de precios asociado: **tarifa plana** o **de usuario**. Todos los planes de una oferta deben estar asociados al mismo modelo de precios. Por ejemplo, no puede haber una oferta en la que uno de sus planes tenga el modelo de precios de tarifa plana y otro tenga el modelo de precios de usuario.
* Dentro de cada plan configurado para un modelo de facturación de tarifa plana, se incluye al menos una cuota periódica (que puede ser de 0 USD):
    * Cuota **mensual** periódica: cuota mensual de tarifa plana que se paga previamente con una periodicidad mensual cuando el usuario compra el plan.
    * Cuota **anual** periódica: cuota anual de tarifa plana que se paga previamente con una periodicidad anual cuando el usuario compra el plan.
* Además de las tarifas periódicas, el plan también puede incluir dimensiones opcionales que se usan para cobrar a los clientes por el uso no incluido en la tarifa plana.   Cada dimensión representa una unidad facturable que el servicio comunicará a Microsoft mediante la [API del servicio de medición de Marketplace](./marketplace-metering-service-apis.md).

## <a name="sample-offer"></a>Oferta de ejemplo

Por ejemplo, Contoso es un editor con un servicio SaaS llamado Contoso Notification Services (CNS). CNS permite que los clientes envíen notificaciones por correo electrónico o por mensaje de texto. Contoso está registrado como editor en el centro de asociados para que el programa comercial de Marketplace publique ofertas para clientes de Azure.  Hay dos planes asociados a CNS, descritos a continuación:

* Plan base
    * Envíe 10 000 correos electrónicos y 1000 mensajes de texto por 0 USD/mes
    * Por encima de 10 000 correos electrónicos, pague 1 USD por cada 100 correos electrónicos
    * Por encima de 1000 mensajes de texto, pague 0,02 USD por cada mensaje de texto
* Plan Premium
    * Envíe 50 000 correos electrónicos y 10 000 mensajes de texto por 350 USD/mes
    * Por encima de 50 000 correos electrónicos, pague 0,5 USD por cada 100 correos electrónicos
    * Por encima de 10 000 mensajes de texto, pague 0,01 USD por cada mensaje de texto

Un cliente de Azure suscrito al servicio CNS podrá enviar la cantidad incluida de mensajes de texto y correos electrónicos al mes en función del plan seleccionado.  Contoso mide el uso hasta la cantidad incluida sin enviar eventos de uso a Microsoft.  Cuando los clientes consumen más de la cantidad incluida, no tienen que cambiar de plan ni hacer nada diferente.  Contoso medirá el exceso sobre la cantidad incluida y comenzará a emitir eventos de uso a Microsoft para determinar el uso adicional mediante la [API del servicio de medición de Marketplace](./marketplace-metering-service-apis.md).  Microsoft, a su vez, cobrará al cliente por el uso adicional, según ha especificado por el editor.

## <a name="billing-dimensions"></a>Dimensiones de facturación

Las dimensiones de facturación se usan para comunicar al cliente cómo se facturará el uso del software y para notificar a Microsoft los eventos de uso. Estas dimensiones se definen de la manera siguiente:

* **Identificador de dimensión**: el identificador inmutable al que se hace referencia al emitir eventos de uso.
* **Nombre de dimensión**: nombre para mostrar asociado a la dimensión, por ejemplo, "mensajes de texto enviados".
* **Unidad de medida**: descripción de la unidad de facturación, por ejemplo, "por mensaje de texto" o "por cada 100 correos electrónicos".
* **Precio por unidad**: el precio de una unidad de la dimensión.  
* **Cantidad incluida para el período mensual**: la cantidad de dimensión incluida al mes para los clientes que pagan la cuota mensual periódica; debe ser un número entero.
* **Cantidad incluida para el período anual**: la cantidad de dimensión incluida al mes para los clientes que pagan la cuota anual periódica; debe ser un número entero.

Las dimensiones de facturación se comparten entre todos los planes de una oferta.  Algunos atributos son aplicables a la dimensión en todos los planes y otros son específicos de un plan.

Los atributos que definen la propia dimensión se comparten entre todos los planes de una oferta.  Antes de publicar la oferta, un cambio realizado en estos atributos desde el contexto de cualquier plan afectará a la definición de la dimensión en todos los planes.  Una vez publicada la oferta, estos atributos ya no se podrán editar.  Estos atributos son los siguientes:

* Identificador
* Nombre
* Unidad de medida

Los demás atributos de una dimensión son específicos de cada plan y pueden tener valores diferentes de un plan a otro.  Antes de publicar el plan, puede editar estos valores y solo se verá afectado este plan.  Una vez publicado el plan, estos atributos ya no se podrán editar.  Estos atributos son los siguientes:

* Precio unitario
* Cantidad incluida para clientes mensuales 
* Cantidad incluida para clientes anuales 

Las dimensiones también tienen dos conceptos especiales, "habilitado" e "infinito":

* **Habilitado** indica que este plan participa en esta dimensión.  Es posible que desee dejar esta opción desactivada si va a crear un nuevo plan que no envía eventos de uso basados en esta dimensión.  Además, las nuevas dimensiones que se agregan después de publicar un plan por primera vez se muestran como "no habilitadas" en el plan ya publicado.  Ahora se mostrará una dimensión deshabilitada en todas las listas de dimensiones de un plan visualizado por los clientes.
* **Infinito**, representado por el símbolo de infinito "∞", indica que este plan participa en esta dimensión, pero que no mide el uso en esta dimensión.  Permite indicar a los clientes que la funcionalidad representada por esta dimensión está incluida en el plan, sin límite de uso.  Una dimensión con uso infinito se mostrará en las listas de dimensiones de un plan visto por los clientes con una indicación de que nunca incurrirá en un cargo por este plan.

>[!Note] 
>Los siguientes escenarios se admiten explícitamente: <br> - Puede agregar una nueva dimensión a un nuevo plan.  La nueva dimensión no se habilitará para los planes ya publicados. <br> - Puede publicar un plan de **tarifa plana** sin dimensiones y, a continuación, agregar un nuevo plan y configurar una nueva dimensión para ese plan. La nueva dimensión no se habilitará para los planes ya publicados.

## <a name="constraints"></a>Restricciones

### <a name="trial-behavior"></a>Comportamiento de prueba

La facturación según uso con el servicio de medición de Marketplace no es compatible con una oferta de evaluación gratuita.  No es posible configurar un plan para usar la facturación según uso y una evaluación gratuita.

### <a name="locking-behavior"></a>Comportamiento de bloqueo

Dado que una dimensión que se usa con el servicio de medición de Marketplace representa un reconocimiento del modo en que el cliente pagará por el servicio, los detalles de una dimensión no se podrán editar una vez publicada.  Es importante que las dimensiones estén totalmente definidas para un plan antes de la publicación.
  
Una vez que se publica una oferta con una dimensión, ya no se pueden cambiar los detalles de nivel de oferta de esa dimensión:

* Identificador
* Nombre
* Unidad de medida

Una vez que se publica un plan, los detalles de nivel de plan ya no se pueden cambiar:

* Precio unitario
* Cantidad incluida para el período mensual
* Cantidad incluida para el período anual
* Si la dimensión está habilitada para el plan

### <a name="upper-limits"></a>Límites superiores

El máximo de dimensiones que se pueden configurar para una única oferta es de 18 dimensiones únicas.

## <a name="get-support"></a>Obtención de soporte técnico

Puede abrir una incidencia de soporte técnico en cualquiera de los casos siguientes:

* Incidencias técnicas de la API del servicio de medición de Marketplace.
* Una incidencia que debe escalarse debido a un error por su parte (por ejemplo, un evento de uso incorrecto).
* Cualquier otra incidencia relacionada con la facturación según uso. 

Siga los pasos siguientes para enviar una incidencia de soporte técnico:

1. Vaya a la [página de soporte técnico](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff). Los primeros menús desplegables se rellenan automáticamente. Para obtener soporte técnico de Marketplace, identifique la familia de productos como **Cloud and Online Services** (Servicios en la nube y en línea) y el producto como **Anunciante de Marketplace**.  No cambie las selecciones de menú desplegable rellenadas previamente.
2. En "Select the product version" (Seleccionar la versión del producto), seleccione **Live offer management** (Administración de ofertas en directo).
3. En "Select a category that best describe the issue" (Seleccionar la categoría que mejor describa la incidencia), seleccione **Aplicaciones SaaS**.
4. En "Select a problem that best describes the issue" (Seleccionar un problema que mejor describa la incidencia), seleccione **Metered billing** (Facturación medida).
5. Al seleccionar el botón **Siguiente**, se le dirigirá a la página **Detalles del problema**, donde puede especificar más detalles de su incidencia.

Consulte [Soporte técnico para el programa Marketplace comercial en el Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/support) para ver las opciones de soporte técnico para editores.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Marketplace metering service APIs](./marketplace-metering-service-apis.md) (API del servicio de medición de Marketplace) para más información.
