---
title: Resumen de pagos de Marketplace comercial | Azure Marketplace
description: En el resumen de pagos se muestran los detalles sobre el dinero que ha ganado con su oferta. También le permite saber cuándo recibirá los pagos y cuánto le pagarán.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.topic: guide
ms.date: 12/10/2019
ms.openlocfilehash: 5a930dfb38007349155581424d03ee7b3e7a6b46
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730205"
---
# <a name="payout-reporting"></a>Informes de pago

El [**resumen de pagos**](https://docs.microsoft.com/windows/uwp/publish/payout-summary) muestra detalles sobre el dinero que ha ganado con Microsoft. También le permite saber cuándo recibirá los pagos y cuánto le pagarán.

Si vende ofertas en Azure Marketplace, también verá información sobre los **pagos realizados correctamente**. Para más información sobre el pago de Azure Marketplace, consulte [Directivas de participación de Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) y [Acuerdo del anunciante de Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkID=699560).

> [!NOTE]
> Para poder acceder a esta opción, sus ganancias deben alcanzar el [umbral de pago](payment-thresholds-methods-timeframes.md) de 50 USD. Para más información sobre el umbral de pago, consulte esta página y revise la información del [Acuerdo del anunciante de Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkID=699560).

- [Roles y permisos para acceder al informe de pago](#roles-and-permission-to-access-the-payout-report)
- [Informe de pago: diferencia entre Cloud Partner Portal y el Centro de partners](#payout-report-difference-between-cloud-partner-portal-and-partner-center)
- [Tipos de clientes](#customer-types)
- [Correlación entre pago y uso](#corelation-between-payout-and-usage)
- [Descarga del historial de transacciones](#transaction-history-download-export)
- [Soporte técnico y preguntas sobre facturación](#billing-questions-and-support)

## <a name="roles-and-permission-to-access-the-payout-report"></a>Roles y permisos para acceder al informe de pago

| Informes/páginas    | Propietario de cuenta    | Manager  | Desarrollador | Colaborador empresarial |  Colaborador financiero | Vendedor |
|------------------|------------------|----------|-----------|----|----|-----|
| Informe de adquisición (incluidos los datos casi en tiempo real) | Puede ver | Puede ver | Sin acceso | Sin acceso | Puede ver | Sin acceso |
| Respuesta/informe de comentarios | Puede ver y enviar comentarios | Puede ver y enviar comentarios | Puede ver y enviar comentarios | Sin acceso | Sin acceso | Puede ver y enviar comentarios |
| Informe de mantenimiento (incluidos los datos casi en tiempo real) | Puede ver | Puede ver | Puede ver | Puede ver | Sin acceso | Sin acceso |
| Informe de uso | Puede ver | Puede ver | Puede ver | Puede ver | Sin acceso | Sin acceso |
| Cuenta de pago | Puede actualizar | Sin acceso | Sin acceso | Sin acceso | Puede actualizar | Sin acceso |
| Perfil fiscal | Puede actualizar | Sin acceso | Sin acceso | Sin acceso | Puede actualizar | Sin acceso |
| Resumen de pagos | Puede ver | Sin acceso | Sin acceso | Sin acceso | Puede ver | Sin acceso |

## <a name="payout-report-difference-between-cloud-partner-portal-and-partner-center"></a>Informe de pago: diferencia entre Cloud Partner Portal y el Centro de partners

| | Cloud Partner Portal | Centro de partners |
|---------|---------|---------|
| Vínculos | [https://cloudpartner.azure.com/](https://cloudpartner.azure.com) | [https://partner.microsoft.com/dashboard/payouts/reports/transactionhistory](https://partner.microsoft.com/dashboard/payouts/reports/transactionhistory) y [https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments](https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments) |
| Navegación | Informes de pago proporcionados en Insights Payout | Informes de pago proporcionados en el Centro de Partners: icono de pago |
| Ámbito | <ul> <li>Transacción por artículo de línea visible, para cobros en curso, realizados y pagados. </li> <li>Informes: se muestran todos los artículos de línea una vez creado el pedido de compra, incluidos los cobros y la facturación en curso; también se muestran el estado de cobro y los artículos de línea que aún no son válidos para su pago. </li> </ul> | <ul> <li>Muestra los artículos de línea una vez que se consideran ganancias válidas.</li> <li>Los clientes pagan primero a Microsoft y entonces los ISV pueden ver que se inicia el informe de pago.</li> <li>El informe de pago no muestra ni el cobro ni la facturación en curso.  </li> </ul>  |
| La transacción no está lista para el pago | Facturación en curso | Siguiente pago estimado: El estado del pago es sin procesar.  |
| Estado del pago |  | Sin procesar: <br> La ganancia es válida para el pago. Permanece en este estado durante un período de enfriamiento, tal como se define en la guía del programa de incentivos. <br> <br> Próximamente: <br> Revisiones internas pendientes generadas por la orden de pago antes de que se procese el pago. <br> <br> Enviado: <br> El pago se ha enviado al banco. |

## <a name="customer-types"></a>Tipos de clientes

### <a name="enterprise-agreement"></a>Contrato Enterprise

A los clientes sin un contrato Enterprise vigente se les facturan mensualmente las licencias de software de Marketplace. A los clientes con un contrato Enterprise se les factura mensualmente a través de una factura que se presenta cada trimestre.

### <a name="credit-cards-and-monthly-invoice"></a>Tarjetas de crédito y factura mensual

Los clientes también pueden pagar con tarjeta de crédito y una factura mensual. En este caso, los honorarios de licencia de software se facturarán mensualmente.

### <a name="csp-and-direct-pay-users"></a>CSP y usuarios de pago directo

Por ejemplo, si el cliente realiza la compra con tarjeta de crédito.

## <a name="corelation-between-payout-and-usage"></a>Correlación entre pago y uso

|Descripción    |    Date  | Pedidos y uso  | Pago |
|----------|----------|-----------|-------------|
|Período del pedido   | Del 15 al 30 de agosto de 2019 | **Pedidos de atributos de correlación** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Uso** <br> <ul> <li>CustomerId </li> <li>Nombre del cliente</li> <li>(UsageReference)PurchaseRecordId/LineItemId</li> <li> Cargo ampliado estimado <br> Estimated Payout (PC) [Pago estimado (PC)] </li> </ul> |  |
|Final del período (mes)   | 30 de agosto de 2019 | | |
|Fecha de facturación | 1 de septiembre de 2019 | | |
|Fecha de pago del cliente | 1 de septiembre de 2019 | | |
|Período de custodia (solo tarjetas de crédito, 30 días) | Del 1 al 30 de septiembre de 2019 | | **Pedidos de atributos de correlación:** <br> <ul><li>AssetId</li> <li>Customer ID</li> <li> Nombre del cliente</li> </ul> <br> **Uso** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Nombre del cliente</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Estado del pago:** Sin procesar |
|Inicio del período de cobro | 1 de septiembre de 2019 | | |
|Final de período de cobro (30 días como máximo) | 30 de septiembre de 2019 | | |
|Fecha de cálculo de pago (mensualmente el día 15) | 1 de octubre de 2019 | | **Atributos de correlación** <br> <ul><li>AssetId</li> <li>Customer ID</li> <li>Nombre del cliente</li> </ul> <br> **Uso** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Nombre del cliente</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Estado del pago:** UpComing |
|Payout Date (Fecha de pago) | 15 de octubre de 2019 | | **Atributos de correlación** <br> <ul><li>AssetId</li> <li>Customer ID</li> <li> Nombre del cliente</li> </ul> <br> **Uso** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Nombre del cliente</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Estado del pago:** Pago enviado |

### <a name="enterprise-agreement-quarterlymonthly-customers"></a>Contrato Enterprise (clientes trimestrales/mensuales)

| Descripción |    Date  | Uso | Pago |
|----------|----------|---------|-----------|
|Período del pedido | Del 15 al 30 de agosto de 2019 | **Pedidos de atributos de correlación** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Informe de uso** <br> <ul> <li>CustomerId </li> <li>Nombre del cliente</li> <li>(UsageReference)PurchaseRecordId/LineItemId</li> <li> Cargo ampliado estimado <br> Estimated Payout (PC) [Pago estimado (PC)] </li> </ul> | |
|Final del período (trimestre) | 30 de septiembre de 2019 | | |
|Fecha de facturación | 15 de octubre de 2019 | | |
|Período de custodia (solo tarjetas de crédito, 30 días) | N/D | | |
|Inicio del período de cobro | 15 de octubre de 2019 | | |
|Solo tarjetas de crédito, 30 días | Del 1 al 30 de noviembre de 2019 | | |
|Final de período de cobro (90 días como máximo) | 15 de enero de 2020 | | |
|Fecha de pago del cliente | 30 de diciembre de 2019 | | |
|Cálculo del pago | 15 de enero de 2020 | | |
|Payout Date (Fecha de pago) | 15 de febrero de 2020 | | **Para clientes de pago trimestral** <br> <br> **Informe de pedidos** <br> <ul><li>AssetId</li> <li>Customer ID</li> <li> Nombre del cliente</li> </ul> <br> **Uso** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Nombre del cliente</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Estado del pago:** enviado |

## <a name="transaction-history-download-export"></a>Exportación de la descarga del historial de transacciones

Esta opción proporciona la descarga de cada artículo de línea de ganancia que se ve en la página del historial de transacciones, el tipo de ganancia, la fecha, el importe de transacción asociado, el cliente, el producto y otros detalles transaccionales aplicables al programa de incentivos.

| Nombre de la columna     | Descripción    |
|-------------|-------------------------------|
| earningId                      | Identificador único de cada ganancia.                                                                                                       |
| participantId                  | Identidad principal del asociado que gana con el programa.                                                                            |
| participantIdType              | Principalmente el identificador de programa de los programas de incentivos y el IF de vendedor de los programas de la tienda y de Azure Marketplace.                                          |
| participantName                | Nombre del asociado que gana.                                                                                                              |
| partnerCountryCode             | Ubicación o país del asociado que gana.                                                                                                  |
| programName                    | Nombre del programa de incentivos/tienda.                                                                                                             |
| transactionId                  | Identificador único de la transacción.                                                                                                    |
| transactionCurrency            | Moneda en la que se produjo la transacción original del cliente (no es la moneda de la ubicación del asociado).                                     |
| transactionDate                | Fecha de la transacción. Útil para programas en los que muchas transacciones contribuyen a una ganancia.                                           |
| transactionExchangeRate        | Tipo de cambio usado para mostrar el importe en USD correspondiente de la transacción.                                                                 |
| transactionAmount              | Importe de la transacción en la moneda original de la transacción en función de la ganancia generada.                                              |
| transactionAmountUSD           | Importe de la transacción en USD.                                                                                                                |
| lever                          | Indica la regla de negocios de la ganancia.                                                                                                  |
| earningRate                    | Tasa de incentivos aplicada sobre el importe de la transacción para generar una ganancia.                                                                      |
| quantity                       | Varía en función del programa. Indica la cantidad facturada en los programas de transacción.                                                            |
| quantityType                   | Indica el tipo de cantidad, por ejemplo: cantidad facturada, MAU.                                                                                     |
| earningType                    | Indica si es honorarios, devolución, cooperativo, venta, etc.                                                                                          |
| earningAmount                  | Importe de la ganancia en la moneda original de la transacción.                                                                                      |
| earningAmountUSD               | Importe de la ganancia en USD.                                                                                                                    |
| earningDate                    | Fecha de la ganancia.                                                                                                                      |
| calculationDate                | Fecha en que se calculó la ganancia en el sistema.                                                                                            |
| earningExchangeRate            | Tipo de cambio usado para mostrar el importe en USD correspondiente.                                                                                  |
| exchangeRateDate               | Fecha del tipo de cambio usada para calcular earningAmountUSD.                                                                                   |
| paymentAmountWOTax             | Importe de la ganancia (sin impuestos) en la moneda del destinatario del pago solo para los pagos enviados.                                                                 |
| paymentCurrency                | Moneda del destinatario del pago elegida por el asociado en el perfil de pago. Solo se muestra para los pagos enviados.                                                   |
| paymentExchangeRate            | Tipo de cambio usado para calcular paymentAmountWOTax en la moneda de pago mediante ExchangeRateDate.                                            |
| paymentId            | Identificador único del pago. Este número es visible en el extracto bancario.                                            |
| paymentStatus            | Estado del pago.                                            |
| paymentStatusDescription            | Descripción detallada del estado del pago.                                            |
| customerId                     | Siempre estará en blanco.                                                                                                                     |
| customerName                   | Siempre estará en blanco.                                                                                                                     |
| partNumber                     | Siempre estará en blanco.                                                                                                                     |
| ProductName                    | Nombre del producto vinculado a la transacción.                                                                                                       |
| productId                      | Identificador único del producto.                                                                                                                |
| parentProductId                | Identificador único del producto principal. Nota: Si no hay un producto principal para la transacción, el identificador del producto principal es igual al identificador del producto. |
| parentProductName              | Nombre del producto principal. Nota: Si no hay un producto principal para la transacción, el nombre del producto principal es igual al nombre del producto.   |
| productType                    | Tipo de producto (por ejemplo, aplicación, complemento, juego, etc.).                                                                                        |
| invoiceNumber                  | Número de factura (aplicable solo para EA).                                                                                                  |
| resellerId                     | Identificador del revendedor.                                                                                                                      |
| resellerName                   | Nombre del revendedor.                                                                                                                            |
| transactionType                | Tipo de transacción (por ejemplo, compra, reembolso, revocación, contracargo, etc.).                                                               |
| localProviderSeller            | Proveedor o vendedor local del registro.                                                                                                          |
| taxRemitted                    | Importe de impuestos remitidos (ventas, uso o impuestos de IVA/GST).                                                                                   |
| taxRemitModel                  | Entidad responsable de la emisión de impuestos (ventas, uso o impuestos de IVA/GST).                                                                    |
| storeFee                       | La cantidad retenida por Microsoft como honorarios por hacer que la aplicación o el complemento estén disponibles en la tienda.                                            |
| transactionPaymentMethod       | Instrumento de pago del cliente usado para la transacción (por ejemplo, tarjeta, facturación del operador móvil, PayPal, etc.)                                |
| tpan                           | Indica la red de anuncios de terceros.                                                                                                     |
| customerCountry                | País del cliente.                                                                                                                         |
| customerCity                   | Ciudad del cliente.                                                                                                                            |
| customerState                  | Estado del cliente.                                                                                                                           |
| customerZip                    | Código postal del cliente.                                                                                                                 |
| TenantID                       |                                                                                                                                          |
| externalReferenceId            | Identificador único del programa.                                                                                                        |
| externalReferenceIdLabel       | Etiqueta del identificador único.                                                                                                                  |
| transactionCountryCode       | Código de país en el que se produjo la transacción.                                                                                                                  |
| taxCountry       | País del cliente del comprador.                                                                                                                  |
| taxState       | Estado del cliente del comprador.                                                                                                                  |
| taxCity       | Ciudad del cliente del comprador.                                                                                                                  |
| taxZipCode       | Código postal del cliente del comprador.                                                                                                                  |
| LicensingProgramName       |                                                                                                                   |
| Código de programa.       | Cadena que se va a asignar con el nombre del programa.                                                                                                                   |
| earningAmountInLastPaymentCurrency       | Importe de la ganancia en la última moneda de pago (el campo estará vacío si no ha habido ningún pago anterior).                                                                                                                   |
| lastPaymentCurrency       | Moneda del último pago (el campo estará vacío si no ha habido ningún pago anterior).                                                                                                                   |
| AssetId       | Identificador único de los pedidos de cliente del servicio de Marketplace.  Representa los artículos de línea de la compra tramitada. Puede haber varios recursos.                                                                                                                   |
| OrderId       | Se relaciona con la factura de un cliente.                                                                                                                   |
| LineItemId       | Línea individual en la factura de un cliente.                                                                                                                   |
| Customer Country (País del cliente)       | Nombre de país proporcionado por el cliente.  Puede ser diferente del país de la suscripción a Azure de un cliente.                                                                                                                   |
| Customer EmailAddress       | Dirección de correo electrónico proporcionada por el cliente final.  Puede ser diferente de la dirección de correo electrónico de la suscripción a Azure del cliente.                                                                                                                   |
| SkuId       | Identificador de la SKU definida durante la publicación. Una oferta puede tener muchas SKU, pero una SKU solo puede asociarse a una única oferta.                                                                                                                   |

>[!Note]
>Todos los informes y las conclusiones de la opción de publicación de la transacción están disponibles en la sección Conclusiones de Cloud Partner Portal o en la sección Análisis del Centro de partners.

## <a name="billing-questions-and-support"></a>Soporte técnico y preguntas sobre facturación

Para obtener ayuda si tiene preguntas sobre facturación, póngase en contacto con el [equipo de soporte técnico para publicadores de Marketplace comercial](https://partner.microsoft.com/support/v2/?stage=1).
