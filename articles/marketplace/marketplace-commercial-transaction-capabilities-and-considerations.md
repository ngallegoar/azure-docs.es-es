---
title: 'Marketplace: ‎Funcionalidades y consideraciones sobre transacciones comerciales | Azure'
description: En este artículo se describen las consideraciones de precios, facturación y pago de transacciones para un tipo de oferta.
services: Azure, Marketplace, Compute, Storage, Networking, Transact Offer Type
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: dsindona
ms.openlocfilehash: de93a3c48e0b0aceb447e54e11190f487d1aa2d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279834"
---
# <a name="commercial-marketplace-transaction-capabilities-and-considerations"></a>Funcionalidades y consideraciones sobre transacciones comerciales de Marketplace

En este artículo se tratan los siguientes temas relacionados con el comercio en Marketplace comercial

* Opciones de publicación de Marketplace
* Información general sobre las transacciones
* Modelos de facturación de las transacciones
* Requisitos de las transacciones

## <a name="marketplace-publishing-options"></a>Opciones de publicación de Marketplace

Las opciones de publicación siguientes están disponibles para los publicadores de Marketplace comercial.

### <a name="list--trial-publishing-options"></a>Opciones de publicación de listas y pruebas

Los publicadores pueden aprovechar las opciones de publicación de listas, pruebas y BYOL con fines promocionales y de adquisición por parte de usuarios. Con estas opciones, Microsoft no participa directamente en las transacciones de licencias de software del publicador, y no hay ningún honorario de transacción asociado. Los publicadores son responsables de todos los aspectos de la transacción de las licencias de software, incluidas, entre otras cosas: el pedido, la entrega, la medición, la facturación, el pago y el cobro. Con las opciones de publicación de listas y pruebas, los publicadores mantienen el 100 % de los honorarios de licencias de software del publicador que se cobran al cliente. 

### <a name="transact-publishing-option"></a>Opción de publicación de transacción

Además de las opciones de publicación de listas y pruebas, la opción de publicación de transacción está disponible para los publicadores. Esta aprovecha las funcionalidades comerciales de Microsoft disponibles en todo el mundo y permite a Microsoft hospedar las transacciones de Marketplace en la nube en nombre del publicador.

## <a name="transact-general-overview"></a>Información general sobre las transacciones

Al usar la opción de publicación de transacción, Microsoft permite la venta del software de terceros y la implementación de algunos tipos de oferta en la suscripción a Azure del cliente. El publicador debe tener en cuenta la facturación de los honorarios de infraestructura y los precios de las licencias de software propias del publicador al seleccionar un modelo de facturación y un tipo de oferta.

La opción de publicación de transacción se admite actualmente para los siguientes tipos de oferta: máquinas virtuales, aplicaciones de Azure y aplicaciones SaaS.


![[Ofertas Enterprise de transacción en Azure Marketplace]](./media/marketplace-publishers-guide/Transact-enterprise-deals.png)

### <a name="billing-infrastructure-costs"></a>Costos de infraestructura de facturación

**Para máquinas virtuales y las aplicaciones de Azure**

Para las máquinas virtuales y aplicaciones de Azure, los precios de uso de la infraestructura de Azure se facturan a la suscripción de Azure del cliente.  Los precios de uso de la infraestructura se cotizan y presentan por separado de los precios de licencias del proveedor de software en la factura del cliente.

**Para aplicaciones SaaS**

Para aplicaciones SaaS, el publicador debe tener en cuenta los precios de uso de la infraestructura de Azure y los precios de licencias de software como elemento único de costo.  Se representa como un honorario plano para el cliente. El asociado administra y factura directamente el uso de la infraestructura de Azure.  El cliente no ve los honorarios de uso de infraestructura reales.  En general, los publicadores optan por agrupar los honorarios de uso de la infraestructura de Azure en los precios de las licencias de software.  Los precios de las licencias de software no se basan en mediciones ni en consumo.

## <a name="transact-billing-models"></a>Modelos de facturación de las transacciones

Según la opción de transacción usada, los precios de las licencias de software del publicador se pueden presentar como sigue:  

* Gratis: sin cargo por las licencias de software. 

* Traiga su propia licencia (BYOL): los cargos aplicables para las licencias de software se administran directamente entre el publicador y el cliente. Microsoft solo traspasa los honorarios de uso de la infraestructura de Azure. (Solo para máquinas virtuales y aplicaciones de Azure).

* Pago por uso: los precios de las licencia de software se presentan a una tarifa por hora, por núcleo (vCPU) según la infraestructura de Azure usada. Esta lista solo se aplica a máquinas virtuales y aplicaciones de Azure.

* Precios de suscripción: Las cuotas de licencia de software se presentan como cuotas periódicas mensuales o anuales que se facturan como una tarifa plana o por puesto. Esto solo se aplica a aplicaciones SaaS y aplicaciones de Azure: aplicaciones administradas.

* Prueba de software gratuita: ningún cargo por las licencias de software durante 30 o 90 días.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Gratis y precios de traiga su propia licencia (BYOL)

Al publicar una oferta de transacción gratuita o de traiga su propia licencia, Microsoft no participa en facilitar la transacción de venta para los precios de las licencias de software. Al igual que con las opciones de publicación de listas y pruebas, el publicador conserva el 100 % de los honorarios de las licencias de software. 

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Precios de pago por uso y de suscripción (basados en el sitio)

Al publicar una oferta de transacción de pago por uso o de suscripción, Microsoft proporciona la tecnología y los servicios para procesar las compras, las devoluciones y los contracargos de las licencias de software. En este escenario, el publicador autoriza a Microsoft a actuar como agente para estos fines. El publicador permite a Microsoft facilitar la transacción con las licencias de software, a la vez que conserva su designación como vendedor, proveedor, distribuidor y el emisor de licencias.

Microsoft permite a los clientes pedir, licenciar y usar el software del publicador, sujeto a los términos y condiciones de Marketplace comercial de Microsoft y del contrato de licencia para el usuario final del publicador. Los publicadores deben proporcionar su contrato de licencia para el usuario final o seleccionar el [contrato estándar](https://docs.microsoft.com/azure/marketplace/standard-contract) al crear la oferta.


### <a name="free-software-trials"></a>Evaluaciones gratuitas del software

Para los escenarios de publicación de transacciones, el publicador puede poner una licencia de software a disposición de manera gratuita durante 30 o 90 días. Esta funcionalidad de descuento no incluye el costo de uso de la infraestructura de Azure que se controla mediante el uso de la solución de asociado.

### <a name="private-offers"></a>Ofertas privadas

Además de usar tipos de ofertas y modelos de facturación para monetizar una oferta, los publicadores pueden comercializar una oferta privada, con precios negociados específicos para el negocio, o bien con configuraciones personalizadas. Las ofertas privadas se admiten en las tres opciones de publicación de transacciones.

Esta opción permite que los precios sean mayores o menores que la oferta disponible públicamente. Las ofertas privadas pueden usarse para descontar o agregar una prima por una oferta. Las ofertas privadas pueden estar disponibles para uno o más clientes mediante la inclusión en una lista blanca de su suscripción de Azure en el nivel de oferta.


### <a name="examples"></a>Ejemplos

**Pay-As-You-Go** 

* Si habilita la opción de Pago por uso, obtendrá la siguiente estructura de costos.

|Costo de licencia  | 1,00 USD por hora  |
|---------|---------|
|Costo de uso de Azure (D1/1 núcleo)    |   0,14 USD por hora     |
|*Microsoft factura al cliente*    |  *1,14 USD por hora*       |

* En este escenario, Microsoft factura 1,14 USD por hora de uso de la imagen de máquina virtual publicada.

|Microsoft factura  | 1,14 USD por hora  |
|---------|---------|
|Microsoft le paga el 80 % del costo de licencia|   0,80 USD por hora     |
|Microsoft se queda con el 20 % del costo de licencia  |  0,20 USD por hora       |
|Microsoft se queda con el 100 % costo de uso de Azure | 0,14 USD por hora |

**Traiga su propia licencia (BYOL)**

* Si habilita la opción BYOL, obtendrá la siguiente estructura de costos.

|Costo de licencia  | Usted negocia y factura el precio de licencia  |
|---------|---------|
|Costo de uso de Azure (D1/1 núcleo)    |   0,14 USD por hora     |
|*Microsoft factura al cliente*    |  *0,14 USD por hora*       |

* En este escenario, Microsoft factura 0,14 USD por hora de uso de la imagen de máquina virtual publicada.

|Microsoft factura  | 0,14 USD por hora  |
|---------|---------|
|Microsoft se queda con el costo de uso de Azure    |   0,14 USD por hora     |
|Microsoft se queda con 0 % del costo de licencia   |  0,00 USD por hora       |

**Suscripción de aplicación SaaS**

Esta opción debe configurarse para vender a través de Microsoft y puede tener un precio con una tarifa plana o por usuario, de forma mensual o anual.
*    Si habilita la opción de venta mediante Microsoft de una oferta de SaaS, obtendrá la siguiente estructura de costos.

|Costo de licencia       | 100,00 USD al mes  |
|--------------|---------|
|Costo de uso de Azure (D1/1 núcleo)    | Facturado directamente al publicador, no al cliente |
|*Microsoft factura al cliente*    |  *100,00 USD al mes (nota: el publicador debe tener en cuenta en el precio de las licencias los costos de infraestructura en los que se incurra o traspasados)*  |

* En este escenario, Microsoft factura 100,00 USD por su licencia de software y paga 80,00 USD al publicador.
* Los asociados que sean aptos para la tarifa reducida del servicio Marketplace verán una cuota de transacción reducida en las ofertas de SaaS desde mayo de 2019 hasta junio de 2020. En este escenario, Microsoft factura 100,00 USD por su licencia de software y paga 90,00 USD al anunciante.

|Microsoft factura  | 100,00 USD al mes  |
|---------|---------|
|Microsoft le paga el 80 % del costo de licencia <br> \* Microsoft le paga el 90 % del costo de licencia de las aplicaciones SaaS aptas.   |   80,00 USD al mes <br> \* 90,00 USD al mes    |
|Microsoft se queda con el 20 % del costo de licencia <br> \* Microsoft se queda con el 10 % del costo de licencia de las aplicaciones SaaS aptas.  |  20,00 USD al mes <br> \* 10,00 USD     |

* **Tarifa reducida del servicio Marketplace:** Para determinados productos SaaS que publica en nuestro Marketplace comercial, Microsoft reducirá su tarifa del servicio Marketplace del 20 % (tal como se describe en el Contrato para publicadores de Microsoft) al 10 %.  Para que su producto sea apto, Microsoft debe designar al menos uno de sus productos como IP Co-Sell Ready o IP Co-Sell Prioritized. Para recibir esta tarifa reducida del servicio Marketplace durante el mes, se deben cumplir los criterios de idoneidad al menos cinco (5) días hábiles antes del final del mes natural anterior. La tarifa reducida del servicio Marketplace no se aplicará a las máquinas virtuales, las aplicaciones administradas ni ningún otro producto que esté disponible a través de nuestro Marketplace comercial.  Esta tarifa reducida del servicio Marketplace estará disponible para las ofertas aptas, con los cargos de licencia recaudados por Microsoft entre el 1 de mayo de 2019 y el 30 de junio de 2020.  Cuando pase ese tiempo, la tarifa del servicio Marketplace volverá a su importe normal.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Facturación, pago y cobro al cliente

**Facturación y pago**

El publicador puede usar el método de facturación preferido del cliente para comunicar los precios de las licencias del software de suscripción o de pago por uso.

**Contrato Enterprise** 

Si el método de facturación preferido del cliente es el contrato Enterprise de Microsoft, los honorarios de las licencias de software se facturarán mediante este método de facturación como un costo detallado, independiente de los costos de uso específicos de Azure.

**Tarjetas de crédito y factura mensual** 

Los clientes también pueden pagar con tarjeta de crédito y una factura mensual. En este caso, los precios de las licencias de software se facturarán igual que en el escenario de contrato Enterprise, como un costo detallado, independiente de los costos de uso específicos de Azure.

Por ejemplo, si el cliente compra con tarjeta de crédito:

|Descripción    |    Date  |
|----------|----------|
|Período del pedido   | Del 15 de agosto al 30 de agosto de 2018 |
|Final del período (mes)   | 30 de agosto de 2018 |
|Fecha de facturación | 1 de septiembre de 2018 |
|Fecha de pago del cliente | 1 de septiembre de 2018 |
|Período de custodia (solo tarjetas de crédito, 30 días) | Del 1 de septiembre al 30 de septiembre de 2018 |
|Inicio del período de cobro | 1 de septiembre de 2018 |
|Final de período de cobro (30 días como máximo) | 30 de septiembre de 2018 |
|Fecha de cálculo de pago (mensualmente el día 15) | 1 de octubre de 2018 |
|Payout Date (Fecha de pago) | 15 de octubre de 2018 |

Si el cliente compra mediante un contrato Enterprise:

| Descripción |    Date  |
|----------|----------|
|Período del pedido | Del 15 de agosto al 30 de agosto de 2018 |
|Final del período (trimestre) | 30 de septiembre de 2018 |
|Fecha de facturación | 15 de octubre de 2018 |
|Período de custodia (solo tarjetas de crédito, 30 días) | N/D |
|Inicio del período de cobro | 15 de octubre de 2018 |
|Final de período de cobro (90 días como máximo) | 15 de enero de 2019 |
|Fecha de pago del cliente | 30 de diciembre de 2018 |
|Fecha de cálculo de pago (mensualmente el día 15) | 15 de enero de 2019 |
|Payout Date (Fecha de pago) | 15 de febrero de 2019 |

**Créditos gratuitos y compromiso monetario** 

Algunos clientes eligen pagar Azure por adelantado con un compromiso monetario en el contrato Enterprise, o han recibido créditos gratuitos para su uso con Azure. Aunque estos créditos se pueden usar para pagar por el uso de Azure, no se puede usar para pagar los honorarios de las licencias de software del publicador.

**Facturación y cobro** 

La facturación de las licencias de software del publicador se presenta mediante el método de facturación seleccionado por el cliente y sigue el cronograma de facturación. A los clientes sin un contrato Enterprise vigente se les facturan mensualmente las licencias de software de Marketplace. A los clientes con un contrato Enterprise se les factura mensualmente a través de una factura que se presenta cada trimestre.

Cuando se han seleccionado los modelos de precios de suscripción o de pago por uso, Microsoft actúa como agente del publicador y es responsable de todos los aspectos de facturación, pago y cobro.

### <a name="publisher-payout-and-reporting"></a>Pagos e informes al publicador

* Los honorarios de las licencias de software cobrados por Microsoft como agente están sujetos a una tarifa de transacción del 20 %, a menos que se especifique lo contrario, y se deducen en el momento del pago al publicador.

* Normalmente, los clientes compran con contrato Enterprise o mediante contrato de pago por uso con tarjeta de crédito. El tipo de contrato determina los plazos de facturación, cobro y pago.

>[!NOTE] 
>Todos los informes y las conclusiones de la opción de publicación de la transacción están disponibles en la sección Conclusiones de Cloud Partner Portal o en la sección Análisis del Centro de partners.

#### <a name="billing-questions-and-support"></a>Soporte técnico y preguntas sobre facturación

Para obtener más información y las directivas legales, consulte el [contrato del publicador](https://cloudpartner.azure.com/Content/Unversioned/PublisherAgreement2.pdf) (disponible en Cloud Partner Portal).

Para obtener ayuda si tiene preguntas sobre facturación, póngase en contacto con el [equipo de soporte técnico para publicadores de Marketplace comercial](https://aka.ms/marketplacepublishersupport).

## <a name="transact-requirements"></a>Requisitos de las transacciones

En esta sección se tratan los requisitos de transacción para distintos tipos de oferta.

### <a name="requirements-for-all-offer-types"></a>Requisitos para todos los tipos de ofertas

- Se necesitan una cuenta de Microsoft y la información financiera para la opción de publicación de transacciones, independientemente del modelo de precios de la oferta.
- Entre la información financiera obligatoria se incluyen el perfil fiscal y la cuenta de pago.

Para obtener más información sobre cómo configurar estas cuentas, vea [Administración de la cuenta del Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/manage-account).


### <a name="requirements-for-specific-offer-types"></a>Requisitos para tipos de ofertas específicos

La opción de publicación de transacciones solo está disponible para su uso con los siguientes tipos de oferta de Marketplace: 

**Máquina virtual** 

Seleccione entre los modelos de precios gratuito, traiga su propia licencia o pago por uso, y preséntelo como SKU definida en el nivel de oferta. En la factura de Azure para el cliente, Microsoft presenta los precios de las licencias de software del publicador por separado de los precios de la infraestructura de Azure subyacente. Los precios de la infraestructura de Azure corresponden al uso del software del publicador.

**Aplicaciones de Azure: plantilla de soluciones o aplicación administrada** 

Debe aprovisionar una o varias máquinas virtuales y extrae la suma de los precios de máquina virtual. Para aplicaciones administradas en un único plan, se puede seleccionar una suscripción mensual con tarifa plana como modelo de precios, en su lugar del precio de las máquinas virtuales. En algunos casos, el precio por uso de la infraestructura de Azure se traslada al cliente por separado de los precios de las licencias de software, pero en el mismo extracto de facturación. En cambio, si configura una oferta de aplicación administrada para los cargos por infraestructura de ISV, los recursos de Azure se facturan al publicador y el cliente recibe una tarifa plana que incluye el costo de la infraestructura, las licencias de software y los servicios de administración.

## <a name="next-steps"></a>Pasos siguientes

* Revise los requisitos de idoneidad de las opciones de publicación por tipo de oferta para finalizar la selección y la configuración de la oferta.
* Revise los patrones de publicación por escaparate para ver ejemplos de cómo la solución se asigna a un tipo de oferta y configuración.
