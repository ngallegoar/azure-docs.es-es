---
title: Facturación de marketplace comercial en Azure Marketplace
description: Obtenga información sobre las opciones de publicación en el marketplace y los modelos de facturación para el marketplace comercial.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.topic: guide
ms.date: 12/12/2019
ms.openlocfilehash: deb4f3f1bb17bff0b09b2e4f79ceb967d4d7ff59
ms.sourcegitcommit: 3beb067d5dc3d8895971b1bc18304e004b8a19b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82744966"
---
# <a name="commercial-marketplace-billing-with-transact-billing-models"></a>Facturación en el marketplace comercial con modelos de facturación de transacciones

En este artículo se tratan temas relacionados con el comercio de Marketplace comercial:

- [Opciones de publicación de Marketplace](#marketplace-publishing-options)
- [Información general sobre las transacciones](#transact-general-overview)
- [Modelos de facturación de las transacciones](#transact-billing-models)

## <a name="marketplace-publishing-options"></a>Opciones de publicación de Marketplace

Marketplace comercial ofrece varias opciones de publicación para los editores.

### <a name="list-and-trial-publishing-options"></a>Opciones de publicación de listas y pruebas

Los editores pueden aprovechar las opciones de publicación de listas, pruebas y traiga su propia licencia (BYOL) con fines promocionales y de adquisición por parte de usuarios. Con estas opciones, Microsoft no participa directamente en las transacciones de licencias de software del publicador, y no hay ningún honorario de transacción asociado. Los publicadores son responsables de todos los aspectos de la transacción de las licencias de software, incluidas, entre otras cosas: el pedido, la entrega, la medición, la facturación, el pago y el cobro. Con las opciones de publicación de listas y pruebas, los publicadores mantienen el 100 % de los honorarios de licencias de software del publicador que se cobran al cliente.

### <a name="transact-publishing-option"></a>Opción de publicación de transacción

Además de las opciones de publicación de listas y pruebas, la opción de publicación de transacción está disponible para los publicadores. Esta opción aprovecha las funcionalidades comerciales de Microsoft disponibles en todo el mundo y permite a Microsoft hospedar las transacciones de Marketplace en la nube en nombre del editor.

## <a name="transact-general-overview"></a>Información general sobre las transacciones

Al usar la opción de publicación de transacción, Microsoft permite la venta del software de terceros y la implementación de algunos tipos de oferta en la suscripción a Azure del cliente. El publicador debe tener en cuenta la facturación de los honorarios de infraestructura y los precios de las licencias de software propias del publicador al seleccionar un modelo de facturación y un tipo de oferta.

Actualmente, la opción de publicación de transacción es compatible con los siguientes tipos de oferta: máquinas virtuales, aplicaciones de Azure y aplicaciones SaaS.

![Transacciones en Azure Marketplace](./media/transact-amp.png)

### <a name="billing-infrastructure-costs"></a>Costos de infraestructura de facturación

#### <a name="for-virtual-machines-and-azure-applications"></a>Para las máquinas virtuales y las aplicaciones de Azure

Para las máquinas virtuales y aplicaciones de Azure, los precios de uso de la infraestructura de Azure se facturan a la suscripción a Azure del cliente. Los precios de uso de la infraestructura se cotizan y presentan por separado de los precios de licencias del proveedor de software en la factura del cliente.

#### <a name="for-saas-apps"></a>Para aplicaciones SaaS

Para aplicaciones SaaS, el publicador debe tener en cuenta los precios de uso de la infraestructura de Azure y los precios de licencias de software como elemento único de costo. Se representa como un honorario plano para el cliente. El asociado administra y factura directamente el uso de la infraestructura de Azure. El cliente no ve los honorarios de uso de infraestructura reales. En general, los publicadores optan por agrupar los honorarios de uso de la infraestructura de Azure en los precios de las licencias de software. Los precios de las licencias de software no se basan en mediciones ni en consumo.

## <a name="transact-billing-models"></a>Modelos de facturación de las transacciones

Según la opción de transacción usada, los precios de las licencias de software del publicador se pueden presentar como sigue:

- *Gratis*: sin cargo por las licencias de software.
- *Traiga su propia licencia (BYOL)* : los cargos aplicables para las licencias de software se administran directamente entre el publicador y el cliente. Microsoft solo traspasa los honorarios de uso de la infraestructura de Azure. (Solo para máquinas virtuales y aplicaciones de Azure).
- *Pago por uso*: los precios de las licencia de software se presentan a una tarifa por hora, por núcleo (vCPU) según la infraestructura de Azure usada. Este modelo solo se aplica a máquinas virtuales y aplicaciones de Azure.
- *Precios de suscripción*: Las cuotas de licencia de software se presentan como cuotas periódicas mensuales o anuales que se facturan como una tarifa plana o por puesto. Este modelo solo se aplica a aplicaciones SaaS y aplicaciones de Azure: aplicaciones administradas.
- *Evaluación de software gratuita*: ningún cargo por las licencias de software durante 30 o 90 días.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Gratis y precios de traiga su propia licencia (BYOL)

Al publicar una oferta de transacción gratuita o de traiga su propia licencia, Microsoft no participa en facilitar la transacción de venta para los precios de las licencias de software. Al igual que con las opciones de publicación de listas y pruebas, el publicador conserva el 100 % de los honorarios de las licencias de software.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Precios de pago por uso y de suscripción (basados en el sitio)

Al publicar una oferta de transacción de pago por uso o de suscripción, Microsoft proporciona la tecnología y los servicios para procesar las compras, las devoluciones y los contracargos de las licencias de software. En este escenario, el publicador autoriza a Microsoft a actuar como agente para estos fines. El publicador permite a Microsoft facilitar la transacción con las licencias de software, a la vez que conserva su designación como vendedor, proveedor, distribuidor y el emisor de licencias.

Microsoft permite a los clientes pedir, licenciar y usar el software del publicador, sujeto a los términos y condiciones de Marketplace comercial de Microsoft y del contrato de licencia para el usuario final del publicador. Los editores deben proporcionar su contrato de licencia para el usuario final o seleccionar el [Contrato estándar](https://docs.microsoft.com/azure/marketplace/standard-contract) al crear la oferta.

### <a name="free-software-trials"></a>Evaluaciones gratuitas del software

Para los escenarios de publicación de transacciones, el publicador puede poner una licencia de software a disposición de manera gratuita durante 30 o 90 días. Esta funcionalidad de descuento no incluye el costo de uso de la infraestructura de Azure que se controla mediante el uso de la solución de asociado.

### <a name="private-offers"></a>Ofertas privadas

Además de usar tipos de ofertas y modelos de facturación para monetizar una oferta, los publicadores pueden comercializar una oferta privada, con precios negociados y específicos para la oferta, o bien con configuraciones personalizadas. Las ofertas privadas se admiten en las tres opciones de publicación de transacciones.

Esta opción permite que los precios sean mayores o menores que la oferta disponible públicamente. Las ofertas privadas pueden usarse para descontar o agregar una prima por una oferta. Las ofertas privadas pueden estar disponibles para uno o más clientes mediante la inclusión en una lista blanca de su suscripción de Azure en el nivel de oferta.

### <a name="examples"></a>Ejemplos

#### <a name="pay-as-you-go"></a>Pago por uso

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

### <a name="bring-your-own-license-byol"></a>Traiga su propia licencia (BYOL)

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

### <a name="saas-app-subscription"></a>Suscripción de aplicación SaaS

Esta opción debe configurarse para vender a través de Microsoft y puede tener un precio con una tarifa plana o por usuario, de forma mensual o anual.

•   Si habilita la opción de venta mediante Microsoft de una oferta de SaaS, obtendrá la siguiente estructura de costos.

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

**Tarifa reducida del servicio Marketplace:** Para determinados productos SaaS que publica en nuestro Marketplace comercial, Microsoft reducirá su tarifa del servicio Marketplace del 20 % (tal como se describe en el Contrato para publicadores de Microsoft) al 10 %.  Para que su producto sea apto, Microsoft debe designar al menos uno de sus productos como IP Co-Sell Ready o IP Co-Sell Prioritized. Para recibir esta tarifa reducida del servicio Marketplace durante el mes, se deben cumplir los criterios de idoneidad al menos cinco (5) días hábiles antes del final del mes natural anterior. La tarifa reducida del servicio Marketplace no se aplicará a las máquinas virtuales, las aplicaciones administradas ni ningún otro producto que esté disponible a través de nuestro Marketplace comercial.  Esta tarifa reducida del servicio Marketplace estará disponible para las ofertas aptas, con los cargos de licencia recaudados por Microsoft entre el 1 de mayo de 2019 y el 30 de junio de 2020.  Cuando pase ese tiempo, la tarifa del servicio Marketplace volverá a su importe normal.
