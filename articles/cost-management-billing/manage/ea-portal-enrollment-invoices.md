---
title: Facturas de inscripciones de Azure Enterprise
description: En este artículo se explica cómo administrar y actuar en su factura de Azure Enterprise.
author: bandersmsft
ms.author: banders
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: boalcsva
ms.openlocfilehash: 9fbc2ffcf1158f436c060817f8f25152f839c3e0
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "79037266"
---
# <a name="azure-enterprise-enrollment-invoices"></a>Facturas de inscripciones de Azure Enterprise

En este artículo se explica cómo administrar y actuar en su factura de Contrato Enterprise de Azure (Azure EA). La factura es una representación del recibo. Revise que esté correcta. También debe familiarizarse con otras tareas que podrían ser necesarias para administrar la factura.

## <a name="change-a-po-number-for-an-overage-invoice"></a>Cambio de un número de pedido de compra para una factura de uso por encima del límite

Azure Enterprise Portal genera automáticamente un número de pedido de compra predeterminado, a menos que el administrador de empresa defina uno antes de la fecha de la factura. Un administrador de empresa puede actualizar el número de pedido de compra hasta siete días después de recibir un correo electrónico de notificación de factura automática.

### <a name="to-update-the-azure-services-purchase-order-number"></a>Para actualizar el número de pedido de compra de los servicios de Azure:

1. En Azure Enterprise Portal, seleccione **Informes** > **Resumen de uso**.
1. Seleccione **Edit PO Numbers** (Editar números de pedidos de compra) en la esquina superior derecha.
1. Seleccione el botón de radio **Azure Services** (Servicios de Azure).
1. Seleccione un **período de facturación** en el menú desplegable de intervalos de fechas.

   Puede editar un número de pedido de compra durante un período de siete días después de recibir una notificación de factura, pero antes de haber pagado la factura.
1. Escriba un número de pedido de compra nuevo en el campo**PO Number** (Número de pedido de compra).
1. Seleccione **Guardar** para enviar el cambio.

### <a name="to-update-the-azure-marketplace-purchase-order-number"></a>Para actualizar el número de pedido de compra de Azure Marketplace:

1. En Azure Enterprise Portal, seleccione **Informes** > **Resumen de uso**.
1. Seleccione **Edit PO Numbers** (Editar números de pedidos de compra) en la esquina superior derecha.
1. Seleccione el botón de radio **Marketplace**.
1. Seleccione un **período de facturación** en el menú desplegable de intervalos de fechas.

   Puede editar un número de pedido de compra durante un período de siete días después de recibir una notificación de factura, pero antes de haber pagado la factura.
1. Escriba un número de pedido de compra nuevo en el campo**PO Number** (Número de pedido de compra).
1. Seleccione **Guardar** para enviar el cambio.

## <a name="cadence-of-azure-enterprise-billing"></a>Cadencia de facturación de Azure Enterprise

Microsoft factura anualmente en la fecha de entrada en vigor de la inscripción las compras de compromiso de los servicios de Microsoft Azure. Microsoft factura como atrasado el uso que supere las cantidades de compromiso.

- Las tarifas de compromiso se ofrecen a razón de una tarifa mensual y se facturan anualmente por adelantado.
- Las tarifas de uso por encima del límite se calculan cada mes y se facturan a período vencido al final del período de facturación.

### <a name="billing-intervals"></a>Intervalos de facturación

El intervalo de facturación depende de cómo decida hacer sus compras de compromisos. Su compromiso anual coincidente con una de las siguientes:

- La fecha de aniversario de la inscripción
- La fecha efectiva de la suscripción de modificación de un año.

La fecha en que reciba la factura de uso por encima del límite depende de la fecha de inicio de su inscripción y de su configuración:

- **Inscripciones directas con una fecha de inicio anterior al 1 de mayo de 2018**:
  - En el caso de un Contrato Enterprise (EA) directo, se encuentra en un período de facturación anual para los servicios de Azure, excepto los servicios de Azure Marketplace. El período de facturación se basa en la fecha de aniversario, la fecha en la que el contrato entró en vigor.
  - Si supera el 150 % del umbral de su compromiso monetario de EA, se convertirá automáticamente en un período de facturación trimestral basado en la fecha de su aniversario. También recibirá una factura por uso por encima del límite de los servicios de Azure.
  - Si no supera el 150 % del umbral del compromiso monetario, la inscripción permanecerá en un período de facturación anual. La factura de uso por encima del límite se recibirá al final del año del compromiso.

- **Inscripciones directas con una fecha de inicio posterior al 1 de mayo de 2018**:
  - Las facturas de consumo y cargos de Azure facturados por separado tendrán un período de facturación mensual.
  - Los cargos que no cubra el compromiso monetario se deben como pago de uso por encima del límite.  

- **Inscripciones indirectas con una inscripción iniciada antes del 1 de mayo de 2018**:

  Si es cliente de un Contrato Enterprise (EA) indirecto con una fecha de inicio anterior al 1 de mayo de 2018, se encuentra en un período de facturación trimestral. El partner de canal (CP) le factura directamente.  

- **Inscripciones indirectas con una fecha de inicio posterior al 1 de mayo de 2018**:

  Se encuentra en un período de facturación mensual.  

### <a name="increase-your-monetary-commitment"></a>Aumentar el compromiso monetario

Puede aumentar su compromiso en cualquier momento. Se le facturará el número de meses restantes en el período de compromiso de ese año. Por ejemplo, si se registra en una suscripción de enmienda de un año y aumenta el compromiso durante el sexto mes, se le facturarán los seis meses restantes de ese período. Las cantidades de compromiso se actualizarán durante los últimos seis meses del período de compromiso. Estas nuevas cantidades se usarán para determinar los cargos por uso por encima del límite.

### <a name="overage"></a>Superávit

En el caso del uso por encima del límite, se le facturará el uso o las reservas que superen su compromiso durante el período de facturación. Para ver un desglose del cálculo de las cantidades de uso por encima del límite de elementos individuales, consulte el informe de resumen de uso o póngase en contacto con su partner de canal.

Para cada elemento de la factura, verá:

- **Extended Amount** (Importe extendido): El total de cargos.
- **Commitment Usage** (Uso del compromiso): El importe del compromiso usado para cubrir los cargos.
- **Net Amount** (Importe neto): Los cargos que superan su compromiso.

Los impuestos correspondientes se calculan solo sobre el importe neto que supera el compromiso.

La facturación de uso por encima del límite está automatizada. Los plazos para las notificaciones y las facturas dependen de la fecha de finalización del período de facturación.

- Normalmente, la notificación de uso por encima del límite se envía siete días después de la fecha de finalización de la facturación.
- Las facturas de uso por encima del límite se envían de siete a nueve días después de la notificación.
- Puede revisar los cargos y actualizar los números de pedido de compra generados por el sistema durante los siete días entre la notificación de uso por encima del límite y la facturación.

### <a name="azure-marketplace"></a>Azure Marketplace

A partir del período de facturación de abril de 2019, los clientes comenzaron a recibir una sola factura de Azure, que combina todos los cargos de Azure y Azure Marketplace en una sola factura, en lugar de dos facturas independientes. Este cambio no afecta a los clientes de Australia, Japón ni Singapur.

Durante la transición a una factura combinada, recibirá una factura parcial de Azure Marketplace. Esta factura final independiente mostrará los cargos de Azure Marketplace antes de la fecha de la actualización de la facturación. Los cargos de Azure Marketplace en los que incurra después de esa fecha aparecerán en la factura de Azure. Después del período de transición, verá todos los cargos de Azure y Azure Marketplace en la factura combinada.  

## <a name="adjust-billing-frequency"></a>Ajuste de la frecuencia de facturación

La frecuencia de facturación de un cliente es anual, trimestral o mensual. El período de facturación se determina cuando un cliente firma su contrato. La facturación mensual es el intervalo de facturación más pequeño.

- Es necesaria la **aprobación** del administrador de empresa para cambiar un período de facturación de anual a trimestral para las inscripciones directas. Es necesaria la aprobación de un administrador del partner para las inscripciones indirectas. El cambio se hace efectivo al final del trimestre de facturación actual.
- Es necesaria una **modificación** para cambiar un período de facturación de anual o trimestral a mensual.  Cualquier cambio en el período de facturación de una inscripción existente requiere la aprobación de un administrador de empresa o del usuario identificado como "contacto de facturación".
- **Envíe** la aprobación a [soporte técnico de Azure Enterprise Portal](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c). Seleccione la categoría del problema: **Billing and Invoicing** (Facturación).

El cambio se hace efectivo al final del trimestre de facturación actual.

Si se firma una enmienda M503, puede pasar cualquier contrato de cualquier frecuencia a la facturación mensual. 

## <a name="credits-and-adjustments"></a>Créditos y ajustes

Puede ver todos los créditos o ajustes aplicados a su inscripción en la sección **Informes** de [Azure Enterprise Portal](https://ea.azure.com).

Para ver los créditos:

1. En [Azure Enterprise Portal](https://ea.azure.com), seleccione la sección **Informes**.
1. Seleccione **Resumen de uso**.
1. En la esquina superior derecha, cambie de la vista **M** a la **C**.
1. Amplíe el campo de ajustes de la tabla de compromisos de servicio de Azure.
1. Verá los créditos aplicados a su inscripción y una explicación breve. Por ejemplo: Crédito de Acuerdo de Nivel de Servicio.

## <a name="request-an-invoice-copy"></a>Solicitud de una copia de la factura

Para solicitar una copia de la factura, póngase en contacto con su asociado.

## <a name="overage-offset"></a>Compensación de uso por encima del límite

Para aplicar su compromiso monetario a usos por encima del límite, debe cumplir los siguientes criterios:

- Haber incurrido en cargos de uso por encima del límite que no se han pagado y están en el plazo de un año de la fecha de finalización del servicio facturado.
- El importe del compromiso monetario disponible cubre el número total de cargos incurridos, incluidas todas las facturas de Azure pasadas no pagadas.
- El plazo de facturación que quiere completar debe estar cerrado por completo. La facturación se cierra por completo después del quinto día de cada mes.
- El período de facturación que quiere compensar debe estar cerrado por completo.
- El descuento del compromiso de Azure (ACD) se basa en el nuevo compromiso real menos los fondos planeados para el consumo anterior. Este requisito solo se aplica a los cargos incurridos por uso por encima del límite. Esto solo es válido para los servicios que consumen el compromiso monetario, así que no se aplica a los cargos de Azure Marketplace. Los cargos de Azure Marketplace se facturan por separado.

Para completar una compensación de uso por encima del límite, usted o el equipo de cuentas pueden abrir una solicitud de soporte técnico. Se requiere una aprobación por correo electrónico del administrador de empresa o del contacto de facturación.

## <a name="view-price-sheet-information"></a>Visualización de la información de la hoja de precios

Los administradores de empresa pueden ver la lista de precios asociada a su inscripción para los servicios de Azure.

Para ver la hoja de precios actual:

1. En Azure Enterprise Portal, seleccione **Informes** y, a continuación, seleccione **Hoja de precios**.
2. Vea la hoja de precios o seleccione **Descargar**.

![Ejemplo que muestra la información de la hoja de precios](./media/ea-portal-enrollment-invoices/ea-create-view-price-sheet-information.png)

Para descargar una lista de precios histórica:

1. En Azure Enterprise Portal, seleccione **Informes** y, a continuación, seleccione **Descargar uso**.
2. Descargue la hoja de precios.

![Ejemplo que muestra dónde descargar una hoja de precios anterior](./media/ea-portal-enrollment-invoices/create-ea-view-price-sheet-download-historical-price-list.png)

Algunos motivos para las diferencias de precios:

- Los precios pueden haber cambiado entre la inscripción anterior y la nueva inscripción. Los cambios de precio pueden producirse porque el precio es contractual para la inscripción específica desde la fecha de inicio hasta la fecha de finalización de un contrato.
- Cuando se transfiere a una nueva inscripción, los precios cambian con el nuevo contrato. Los precios se definen por la hoja de precios, que podrían ser más altos en la nueva inscripción.
- Si una inscripción entra en un período extendido, los precios también cambian. Los precios cambian a las tarifas de pago por uso.

## <a name="request-detailed-usage-information"></a>Solicitud de información de uso detallada

Los administradores de empresa pueden ver un resumen de sus datos de uso, el compromiso monetario consumido y los cargos asociados con el uso adicional en Azure Enterprise Portal. Los cargos se presentan en el nivel de resumen en todas las cuentas y suscripciones.

Para ver el uso detallado en cuentas específicas, descargue el informe de detalles de uso en **Informes** > **Descargar uso**.

> [!NOTE]
> El informe de detalles de uso no incluye ningún impuesto aplicable. Puede haber una latencia de hasta ocho horas desde el momento en que se incurrió en el uso hasta el momento en que este se ve reflejado en el informe.

En el caso de inscripciones indirectas, el asociado debe habilitar la función de incremento antes de que pueda ver cualquier información relacionada con el costo.

## <a name="reports"></a>Informes

Los administradores de empresa pueden ver un resumen de sus datos de uso, el compromiso monetario consumido y los cargos asociados con el uso adicional en Azure Enterprise Portal. Los cargos se presentan en el nivel de resumen en todas las cuentas y suscripciones.

### <a name="azure-enterprise-reports"></a>Informes de Azure Enterprise

- Resumen de uso y gráficos
- Informe de uso de servicio
- Informe de saldo y cargos
- Informe de detalles de uso
- Informe de cargos de Azure Marketplace
- Hoja de precios
- Descarga de informes avanzada
- Formato de informe CSV

### <a name="to-view-the-usage-summary-reports-and-graphs"></a>Para ver los informes y gráficos del resumen de uso:

1. Vaya a Azure Enterprise Portal.
1. Seleccione **Informes** en el panel izquierdo.
1. Seleccione la pestaña **Resumen de uso**.
1. Seleccione el período de compromiso en el menú de rangos de fechas en la parte superior izquierda.
1. Seleccione un periodo o el mes en el gráfico para ver detalles adicionales.
1. En esta pestaña, puede:
   - Ver un gráfico del uso mes a mes con un desglose del uso, el cargo extra por los servicios, los cargos facturados por separado y cargos de Azure Marketplace.
   - Filtrar por departamentos, cuentas y suscripciones debajo del gráfico.
   - Cambiar entre los desgloses **Charge by Services** (Cargo por servicios) y **Charge by Hierarchy** (Cargo por jerarquía).
   - Ver los detalles de los servicios de Azure, los cargos facturados por separado y los cargos de Azure Marketplace.

## <a name="service-usage-report"></a>Informe de uso de servicio

La página de informe de uso de los servicios permite a los administradores de empresa ver un resumen de sus datos de uso de servicio. El uso se presenta en el nivel de resumen en todas las cuentas y suscripciones. Para ver el uso detallado, puede filtrar el informe por cuentas o suscripciones.

> [!NOTE]
> Puede haber una latencia de hasta cinco días entre la fecha en que se realizó el uso y el momento en que el uso se muestra en este informe.

### <a name="to-view-the-report"></a>Para ver el informe:

1. Inicie sesión en Azure Enterprise Portal.
1. Seleccione **Informes** en el panel de navegación izquierdo.
1. Seleccione la pestaña **Resumen de uso**.
1. Seleccione el rango de fechas.
1. Elija qué cuentas o suscripciones desea ver.
1. Como alternativa, puede:
   - Cambiar la vista entre **Charge by Services** (Cargo por servicios) y **Charge by Hierarchy** (Cargo por jerarquía) para mostrar distintos desgloses.
   - Ver los detalles, incluidos el nombre del servicio, la unidad de medida, las unidades consumidas, la tasa efectiva y el costo ampliado.

## <a name="download-csv-reports"></a>Descarga de informes CSV

La página de descarga de informes mensuales permite a los administradores de empresa descargar varios informes como archivos CSV. Entre los informes que se pueden descargar se incluyen:

- Informe de saldo y cargos
- Informe de detalles de uso
- Informe de cargos de Azure Marketplace
- Hoja de precios

### <a name="to-download-reports"></a>Para descargar informes:

1. En Azure Enterprise Portal, seleccione **Informes**.
1. Seleccione **Usage Download** (Descarga de uso) en la cinta de opciones superior.
1. Seleccione **Download** (Descargar) junto al informe del mes correspondiente.

### <a name="csv-report-formatting-issues"></a>Problemas de formato de los informes CSV

Los clientes que ven los informes CSV de Azure Enterprise Portal en euros pueden tener problemas de formato relacionados con las comas y los puntos.

Por ejemplo, es posible que vea lo siguiente:

| **ServiceResource** | **ResourceQtyConsumed** | **ResourceRate** | **ExtendedCost** |
| --- | --- | --- | --- |
| Horas | 24 | 0,0535960591133005 | 12,863,054,187,192,100,000,000 |

Debería ver lo siguiente:

| ServiceResource | ResourceQtyConsumed | ResourceRate | ExtendedCost |
| --- | --- | --- | --- |
| Horas | 24 | 0,0535960591133005 | 1,2863054187192120000000 |

Este problema de formato se debe a la configuración predeterminada de la funcionalidad de importación de Excel. Excel importa todos los campos como texto "General" y supone que un número está separado con el estándar matemático. Por ejemplo: "1,000.00".

Si una moneda europea use un punto (.) como separador de miles y una coma (,) como separador de decimales, se mostrará incorrectamente. Por ejemplo: "1.000,00". Los resultados de la importación pueden variar en función de la configuración regional del idioma.

### <a name="to-import-the-csv-file-without-formatting-issues"></a>Para importar el archivo CSV sin problemas de formato:

1. En Microsoft Excel, vaya a **Archivo** > **Abrir**.
   Aparecerá el Asistente para importar texto.
1. En **Tipo de los datos originales**, elija **Delimitado**.  El valor predeterminado es **De ancho fijo**.
1. Seleccione **Next** (Siguiente).
1. En Delimitadores, active la casilla de **Coma**. Desactive **Tabulación** si está seleccionada.
1. Seleccione **Next** (Siguiente).
1. Desplácese a las columnas **Tasa de recursos** y **Coste ampliado**.
1. Seleccione la columna **Tasa de recursos**. Aparece resaltada en negro.
1. En la sección **Formato de los datos en columnas**, seleccione **Texto** en lugar de **General**. El encabezado de columna cambiará de **General** a **Texto**.
1. Repita los pasos 8 y 9 en la columna **Coste ampliado** y seleccione **Finalizar**.

> [!TIP]
> Si ha establecido que los archivos CSV se abran automáticamente en Excel, debe usar la función **Abrir** en Excel en su lugar. En Excel, vaya a **Archivo** > **Abrir**.

## <a name="balance-and-charge-report"></a>Informe de saldo y cargos

El informe de saldo y cargos ofrece un resumen mensual de información sobre saldos, nuevas compras, gastos de servicios en Azure Marketplace, ajustes y gastos por de uso por encima del límite.

Todas las filas de la tabla de resumen Compromiso de servicio de Azure siguen siendo estáticas mes a mes. Encontrará detalles de todas las compras y los ajustes en las secciones **New Purchase Details** (Detalles de nuevas compras) y **Adjustment Details** (Detalles de ajustes).

### <a name="download-the-balance-and-charge-report"></a>Descarga del informe de saldo y cargos

1. Inicie sesión en Azure Enterprise Portal como administrador de empresa.
1. Seleccione **Informes** en el panel izquierdo.
1. Seleccione la pestaña **Descargar informe**.
1. Seleccione el mes correspondiente en la columna **Balance and Charge** (Saldo y cargos) y seleccione para descargar el informe.

## <a name="usage-detail-report"></a>Informe de detalles de uso

El informe de detalles de uso ofrece un resumen mensual del consumo de servicios y cantidades de una inscripción. Incluye información sobre los nombres de medidores, los tipos de medidores y las cantidades consumidas.

### <a name="download-the-usage-detail-report"></a>Descarga del informe de detalles de uso.

1. Inicie sesión en Azure Enterprise Portal como administrador de empresa.
1. Seleccione **Informes** en el panel de navegación izquierdo.
1. Seleccione la pestaña **Descargar uso**.
1. Seleccione el mes correspondiente en la columna **Usage Detail** (Detalles de uso) y seleccione para descargar el informe.

## <a name="azure-marketplace-charges-in-azure-enterprise-portal-reports"></a>Cargos de Azure Marketplace en informes de Azure Enterprise Portal

Existen dos tipos de cargos de Azure Marketplace:

- **Basado en uso:** productos medidos en unidades transaccionales.  Por ejemplo, las máquinas virtuales se cobran por hora, mientras que las búsquedas de Bing API se cobran según el número de búsquedas.
- **Precio mensual**: Se factura mensualmente según el uso o el acceso.

Para más información sobre los cargos de Azure Marketplace, consulte las [preguntas más frecuentes sobre Azure Marketplace](https://azure.microsoft.com/marketplace/faq/).

Para ver los distintos cargos en Azure Enterprise Portal:

- **Usage summary report** (Informe de resumen de uso): Muestra **tanto** los cargos de Azure Marketplace de tarifa mensual como los basados en el uso.
- **Marketplace charges report** (Informe de cargos de Marketplace): Muestra **solo** los cargos de Azure Marketplace basados en el uso.  No se muestran las tarifas puntuales.

> [!NOTE]
> Azure Marketplace no está disponible para inscripciones de MPSA.

## <a name="advanced-report-download"></a>Descarga de informes avanzada

Para los informes de cuentas o rangos de fechas específicos, puede usar la descarga de informes avanzados. A partir del 30 de agosto de 2016, el formato del archivo de salida es CSV para alojar conjuntos de registros mayores.

1. En Azure Enterprise Portal, seleccione **Descarga de informes avanzada**.
1. Seleccione **Appropriate Date Range** (Intervalo de fechas adecuado).
1. Seleccione **Appropriate Accounts** (Cuentas adecuadas).
1. Seleccione **Request Usage Data** (Solicitar datos de uso).
1. Seleccione el botón **Actualizar** hasta que el estado del informe se actualice a **Descargar**.
1. Descargue el informe.

## <a name="reporting-for-non-enterprise-administrators"></a>Informes para administradores que no son de empresa

Los administradores de empresa pueden conceder a los administradores de departamento (DA) y propietarios de cuenta (AO) permisos para ver los cargos en una inscripción. Los propietarios de cuenta con acceso pueden descargar los informes CSV específicos de su cuenta y sus suscripciones. También pueden ver la información de forma visual en la sección de informes de Azure Enterprise Portal.

### <a name="to-enable-access"></a>Para habilitar el acceso:

 1. Inicie sesión en Azure Enterprise Portal como administrador de empresa.
 1. Seleccione **Administrar** en el panel de navegación izquierdo.
 1. Seleccione la pestaña **Inscripción**.
 1. En la sección **Enrollment Detail** (Detalles de inscripción), seleccione el icono de lápiz situado junto a **DA View Charges** (El administrador del departamento ve los cargos) o **AO View Charges** (El propietario de la cuenta ve los cargos).
 1. Seleccione **Habilitado**.
 1. Seleccione **Guardar**.

### <a name="to-view-reports"></a>Para ver informes:

1. Inicie sesión en Azure Enterprise Portal como administrador de departamento o como propietario de cuenta.
1. Seleccione **Informes** en el panel de navegación izquierdo.
1. Seleccione la pestaña **Usage Summary** (Resumen de uso) para ver la información de la cuenta y la suscripción visualmente.
1. Seleccione **Usage Download** (Descarga de uso) para ver los informes CSV.

Los administradores de departamento y los propietarios de cuenta no pueden ver los cargos al usar la función **Advanced Report Download** (Descarga de informes avanzada). Los costos se muestran como $0.

Los permisos del propietario de la cuenta para ver los cargos se extienden a los propietarios de cuenta y a todos los usuarios que tienen permisos en las suscripciones asociadas. Si es un cliente indirecto, el partner de canal debe habilitar las características de costos.

## <a name="move-usage-data-to-another-enrollment"></a>Traslado de los datos de uso a otra inscripción

Los datos de uso solo se trasladan cuando se atrasa una transferencia. Hay dos opciones para trasladar los datos de uso de una inscripción a otra:

- Transferencia de cuentas desde una inscripción a otra
- Transferencias de inscripciones de una inscripción a otra

Para cualquiera de las opciones, debe enviar [una solicitud de soporte técnico](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) al equipo de soporte técnico del Contrato Enterprise para obtener ayuda. 

## <a name="azure-ea-pricing-overview"></a>Introducción a los precios de Azure EA

En esta sección se proporcionan detalles sobre cómo se calcula el uso. Responde a muchas de las preguntas más frecuentes sobre los cargos de varios servicios de Azure en una Contrato Enterprise, donde los cálculos son más complejos.

### <a name="price-protection"></a>Protección de precios

Como cliente o partner de canal, tiene la garantía de recibir precios a los precios que figuran en la hoja de precios del cliente (CPS), o menos, o al precio vigente en la fecha de vigor de la compra de Azure. Este precio se denomina "precio base de referencia". En el caso de los servicios introducidos después de la compra de Azure, se le cobra el precio vigente en el descuento de nivel aplicable cuando se introduce el servicio por primera vez. Esta protección de precios es aplicable durante la vigencia del período de compromiso, de uno o tres años, en función del Contrato Enterprise.

### <a name="price-changes"></a>Cambios en los precios

Microsoft puede reducir el precio del Contrato Enterprise actual para servicios individuales de Azure durante la vigencia de una inscripción. Estas tarifas reducidas se extenderán a los clientes existentes, mientras el precio reducido siga vigente. Si estas tarifas aumentan más adelante, el precio de su inscripción para un servicio no aumentará más allá del límite superior de la protección de precios, tal como se definió anteriormente. Sin embargo, la tasa podría aumentar en relación con el precio reducido anterior. En cualquier caso, informaremos a los clientes y a los partners de canal indirectos de los próximos cambios en los precios mediante un correo electrónico a los administradores de empresa y de partners asociados con la inscripción.

### <a name="current-effective-pricing"></a>Precios vigentes actuales

Para ver los precios actuales de una inscripción, los clientes y partner de canal pueden iniciar sesión en [Azure Enterprise Portal](https://ea.azure.com/) e ir a la página de la hoja de precios de esa inscripción. Si adquiere Azure indirectamente a través de uno de nuestros partners de canal, deberá obtener las actualizaciones de precios de su partner de canal, a menos que este haya habilitado el marcado de precios para su inscripción.

### <a name="introduction-of-new-azure-services"></a>Introducción de nuevos servicios de Azure

Estamos mejorando continuamente Azure y agregamos periódicamente nuevos servicios que tienen un precio independiente de los servicios existentes. Algunos servicios en versión preliminar están disponibles automáticamente, mientras que otros requieren una acción del cliente en el [portal de cuentas de Azure](https://account.windowsazure.com/PreviewFeatures).

Algunos servicios se proporcionan con precios promocionales en vigor cuando se introducen por primera vez, y pueden aumentar con posterioridad.

A medida que los servicios pasen de la versión preliminar a disponibilidad general (GA), los precios pueden aumentar a medida que se aplican acuerdos de nivel de servicio de rendimiento y confiabilidad completos. Este aumento no está limitado por la protección de precios base normal. El uso de esos servicios se cobra a la tarifa aumentada. Para evitar cargos relacionados con estos nuevos servicios, tendrá que dejar de usarlos.

### <a name="introduction-of-regional-pricing"></a>Introducción de precios regionales

Además de la introducción de nuevos servicios, los servicios también cambian periódicamente de un único modelo global a otro más regional, a medida que aumenta la compatibilidad regional para esos servicios.

Cuando se introduce la regionalización de un servicio por primera vez, se aplica la protección de precios base para esas nuevas regiones. Sin embargo, si posteriormente se introducen regiones adicionales para ese mismo servicio, se consideran nuevos servicios y se ofrecen con sus propias tarifas individuales independientes de la protección de precios base.

### <a name="enterprise-devtest"></a>Desarrollo/pruebas - Enterprise

Los administradores de empresa pueden permitir que los propietarios de cuenta creen suscripciones basadas en la oferta Desarrollo/pruebas - Enterprise. El propietario de la cuenta debe configurar las suscripciones de Desarrollo/pruebas - Enterprise necesarias para los suscriptores subyacentes. Esta configuración permite a los suscriptores activos de Visual Studio ejecutar cargas de trabajo de desarrollo y pruebas en Azure a tarifas especiales de Desarrollo/pruebas - Enterprise. Para más información, consulte [Desarrollo/pruebas - Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="enterprise-agreement-usage-calculations"></a>Cálculos de uso de Contratos Enterprise

Consulte [Servicios de Azure](https://azure.microsoft.com/services/) y [Precios de Azure](https://azure.microsoft.com/pricing/) para obtener información pública básica sobre precios, unidades de medida, preguntas frecuentes y directrices de informes de uso para cada servicio individual. Puede encontrar más información sobre los cálculos de EA en las secciones a continuación.

### <a name="enterprise-agreement-units-of-measure"></a>Unidades de medida de Contratos Enterprise

Las unidades de medida de los Contratos Enterprise suelen ser diferentes de las que se ven en nuestros otros programas, como el programa del Contrato Microsoft Online Subscription (MOSA). Esta disparidad significa que, para una serie de servicios, se agrega la unidad de medida para proporcionar los precios normalizados. La unidad de medida que se muestra en la vista del resumen de uso de Azure Enterprise Portal siempre es la medida empresarial. En el archivo de Excel [Friendly Service Names](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx), se proporciona una lista completa de las unidades de medida y conversiones actuales de cada servicio.

### <a name="rounding-rules"></a>Reglas de redondeo

Azure Enterprise Portal sigue la lógica de redondeo bancario o redondeo gaussiano del estándar IEEE. Esta lógica redondea los números a la cifra par más cercana para los valores de medio dígito. La lógica de redondeo más habitual de redondeo de mitad hacia arriba siempre redondea los medios dígitos hacia arriba hasta la siguiente cifra más alta. El método de Azure Enterprise Portal proporciona una suma total más precisa sobre el grupo en comparación con la lógica de Excel estándar.

Para ilustrar esto: cuando la primera cifra que se quitará es un 5 y no le sigue ninguna otra cifra o las cifras siguientes son ceros, se redondea a la cifra par más cercana. Por ejemplo: tanto 2,315 como 2,325 redondeados a la centésima más cercana se convierten en 2,32.

Como referencia, en la tabla siguiente se muestran las fórmulas de Excel que puede usar para modelar las reglas de Azure Enterprise Portal para el redondeo y la conversión:

| Escenario | Fórmula de lógica bancaria |
| --- | --- |
| Redondeo de uso | =REDOND.MULT({_source_}, 0.0002) |
| Redondeo de precios (2 decimales) | =REDOND.MULT({_source_}, 0.02) |
| Redondeo de precios (0 decimales) | =REDOND.MULT({_source_}, 2) |

### <a name="conversion-between-usage-detail-report-and-the-usage-summary-page"></a>Conversión entre el informe de detalles de uso y la página de resumen de uso

En el informe de descarga de datos de uso, puede ver el uso de recursos sin procesar hasta seis lugares decimales. Sin embargo, los datos de uso que se muestran en Azure Enterprise Portal se redondean hasta cuatro posiciones decimales para las unidades de compromiso y se truncan en cero decimales para las unidades de uso por encima del límite. Los datos de uso sin procesar se redondean primero a cuatro cifras antes de la conversión a las unidades usadas en Azure Enterprise Portal. A continuación, las unidades de Enterprise convertidas se redondean de nuevo a cuatro cifras. Solo puede ver el consumo de horas real antes de la conversión en el informe de uso descargado y no en Azure Enterprise Portal mismo.

Por ejemplo: Si se notifican 694,533404 horas reales de SQL Server en el informe de detalles de uso. Estas unidades se convierten en 6,94533404 unidades de 100 horas de proceso, y luego se redondean a 6,9453 y se muestran en Azure Enterprise Portal.

- Para determinar el importe de facturación ampliado, las unidades mostradas se multiplican por el precio de la unidad de compromiso y el resultado se trunca a dos decimales. En el caso del yen japonés (JPY) y el won coreano (KRW), el importe extendido se redondea a cero decimales.
- Para el uso por encima del límite, las unidades de facturación se truncarían en seis cifras y, a continuación, se multiplicarían por el precio unitario de uso por encima del límite para determinar el importe de facturación ampliado.
- Para la facturación de proveedores de servicios administrados (MSP), todo el uso asociado a un departamento marcado como MSP se trunca a cero decimales después de la conversión a la unidad de medida de EA. Como resultado, la suma de este uso podría ser menor que la suma total de todo el uso notificado en Azure Enterprise Portal. Depende de si el MSP está dentro de su saldo de compromiso monetario o está en uso por encima del límite.

### <a name="graduated-pricing"></a>Precios escalonados

Los precios de los Contratos Enterprise no incluyen precios escalonados, donde los cargos por unidad disminuyen a medida que aumenta el uso. Al pasar de un programa MOSA con precios graduados a un Contrato Enterprise, los precios se establecen en proporción con el nivel base del servicio menos los ajustes correspondientes para los descuentos del Contrato Enterprise.

### <a name="partial-hour-billing"></a>Facturación por horas parciales

Todo el uso facturado se basa en minutos convertidos a horas parciales, y no en incrementos de horas completas. Las tarifas de Enterprise por hora enumeradas se aplican a las horas totales más las horas parciales.

### <a name="average-daily-consumption"></a>Consumo diario medio

Algunos servicios tienen un precio mensual, pero el uso se informa diariamente. En estos casos, el uso se evalúa una vez al día, se divide entre 31 y se suma según el número de días de ese mes de facturación. Por lo tanto, las tarifas nunca son superiores a lo previsto para ningún mes y son ligeramente inferiores para los meses con menos de 31 días.

### <a name="compute-hours-conversion"></a>Conversión de horas de proceso

Antes del 28 de enero de 2016, el uso de servicios en la nube y máquinas virtuales de nivel Básico y Estándar A0, A2, A3 y A4 se emitía como minutos del medidor para las máquinas virtuales A1. Las VM A0 se contaban como fracciones de minutos de VM A1, mientras que las A2, A3 y A4 se convertían en múltiplos. Debido a que esta directiva provocaba cierta confusión entre nuestros clientes, implementamos un cambio para asignar el uso por minuto a medidores dedicados para A0, A2, A3 y A4.

La nueva medición entró en vigor entre el 27 y el 28 de enero de 2016. En la descarga del archivo CSV que muestra el uso durante este período de transición, puede ver ambas:

- Uso emitido en el medidor A1
- Uso emitido en el nuevo medidor dedicado correspondiente al tamaño de la implementación.

| **Tamaño de la implementación** | **Uso emitido como múltiplo de A1 hasta el 26 de enero de 2016** | **Uso emitido conforme a una medida dedicada a partir del 27 de enero de 2016** |
| --- | --- | --- |
| A0 | 0,25 horas de A1 | 1 hora de A0 |
| A2 | 2 horas de A1 | 1 hora de A2 |
| A3 | 4 horas de A1 | 1 hora de A3 |
| A4 | 8 horas de A1 | 1 hora de A4 |

### <a name="regions"></a>Regions

En el caso de los servicios en los que la zona y la región afectan a los precios, consulte la tabla siguiente para ver un mapa de las regiones y zonas geográficas:

| Geoárea | Regiones de transferencia de datos | Regiones de CDN |
| --- | --- | --- |
| Zona 1 | Norte de Europa <br> Oeste de Europa <br> Oeste de EE. UU. <br> Este de EE. UU. <br> Centro y norte de EE. UU. <br> Centro y Sur de EE. UU. <br> Este de EE. UU. 2 <br> Centro de EE. UU. | Norteamérica <br> Europa |
| Zona 2 | Este de Asia Pacífico <br> Sudeste de Asia Pacífico <br> Japón Oriental <br> Japón Occidental <br> Este de Australia <br> Sudeste de Australia | Asia Pacífico <br> Japón <br> América Latina <br> Oriente Medio y África <br> Este de Australia <br> Sudeste de Australia |
| Zona 3 | Sur de Brasil |   |

No hay cargos por la salida de datos entre servicios hospedados en el mismo centro de datos. Por ejemplo, Office 365 y Azure.

### <a name="monetary-commitment-and-unbilled-usage"></a>Compromiso monetario y uso no facturado

El compromiso monetario de Azure es un importe pagado por adelantado para los servicios de Azure. El compromiso monetario se consume a medida que se usan los servicios. La facturación de los servicios propios de Azure utiliza el compromiso monetario. Sin embargo, algunos cargos se facturan por separado, y los servicios de Azure Marketplace no consumen el compromiso monetario.

### <a name="charges-billed-separately"></a>Gastos facturados por separado

Algunos productos y servicios proporcionados por orígenes de terceros no consumen compromiso monetario de Azure. En su lugar, estos elementos se facturan por separado como parte de la factura de uso por encima del límite del período de facturación estándar.

Hemos combinado todos los cargos de Azure y Azure Marketplace en una sola factura alineada con el período de facturación de la inscripción. La factura combinada no se aplica a los clientes en Australia, Japón ni Singapur.

La factura combinada muestra el uso de Azure en primer lugar, seguido de los cargos de Azure Marketplace. Los clientes de Australia, Japón o Singapur ven los cargos de Azure Marketplace en una factura independiente.

Si no hay uso por encima del límite al final del período de facturación estándar, los cargos independientes se facturan por separado. Este proceso se aplica a los clientes de Australia, Japón y Singapur.

Los servicios siguientes se facturan por separado:

- Canonical
- Citrix XenApp Essentials
- Usuario registrado de Citrix XenDesktop
- Openlogic
- Usuario registrado de Remote Access Rights XenApp Essentials
- Ubuntu Advantage
- Visual Studio Enterprise (mensual)
- Visual Studio Enterprise (anual)
- Visual Studio Professional (mensual)
- Visual Studio Professional (anual)

## <a name="azure-marketplace-for-ea-customers"></a>Azure Marketplace para clientes de EA

Para los clientes directos, los cargos de Azure Marketplace se pueden ver en Azure Enterprise Portal. Las compras y los consumos de Azure Marketplace se facturan fuera del compromiso monetario, en plazos mensuales o trimestrales a pago vencido.

Los clientes indirectos pueden encontrar sus suscripciones a Azure Marketplace en la página **Administrar suscripciones** de Azure Enterprise Portal, pero no se muestran los precios. Los clientes deben ponerse en contacto con su Proveedor de soluciones de licencia (LSP) para obtener información sobre los cargos en Azure Marketplace.

Las nuevas compras periódicas mensuales o anuales de Azure Marketplace se facturan por completo durante el período en que se compran los elementos de Azure Marketplace. Estos artículos se renovarán automáticamente en el siguiente período el mismo día de la compra original.

Los cargos periódicos mensuales existentes se seguirán renovando el primero de cada mes calendario. Los cargos anuales se renovarán en el aniversario de la fecha de compra.

Algunos servicios de terceros revendedores disponibles en Azure Marketplace ahora consumen el saldo del compromiso monetario del Contrato Enterprise (EA). Antes, estos servicios se facturaban por separado, fuera del compromiso monetario de EA. El compromiso monetario de EA para estos servicios de Azure Marketplace simplifica las compras y la administración de los pagos para los clientes. Para ver una lista completa de los servicios que actualmente consumen compromiso monetario, consulte la [actualización del 6 de marzo de 2018 en el sitio web de Azure](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment/).

### <a name="partners"></a>Asociados

Los LSP pueden descargar una lista de precios de Azure Marketplace en la página de la hoja de precios en Azure Enterprise Portal. Seleccione el vínculo **Lista de precios de Marketplace** en la esquina superior derecha. La lista de precios de Azure Marketplace muestra todos los servicios disponibles y sus precios.

Para descargar la hoja de precios:

1. En Azure Enterprise Portal, vaya a **Informes** > **Hoja de precios**.
1. En la esquina superior derecha, busque el vínculo a la lista de precios de Azure Marketplace debajo de su nombre de usuario.
1. Haga clic con el botón derecho en el vínculo y seleccione **Guardar destino como**.
1. En la ventana **Guardar**, cambie el título del documento a `AzureMarketplacePricelist.zip`, que cambiará el archivo de .xlsx a .zip.
1. Después de completada la descarga, tendrá un archivo ZIP con listas de precios específicas del país o región.
1. Los LSP deben hacer referencia al archivo individual del país para obtener los precios específicos del país o región. Los LSP pueden usar la pestaña **Notificaciones** para estar al tanto de las SKU que son nuevas o retiradas.
1. Los cambios de precio son poco frecuentes. Los LSP reciben notificaciones por correo electrónico de los aumentos de precios y las variaciones en los tipos de cambio (FX) con 30 días de antelación.
1. Los LSP reciben una factura por inscripción, por ISV y por trimestre.

### <a name="enabling-azure-marketplace-purchases"></a>Habilitación de compras en Azure Marketplace

Los administradores de empresa pueden deshabilitar o habilitar las compras de Azure Marketplace para todas las suscripciones de Azure en su inscripción. Si el administrador de empresa deshabilita las compras, y hay suscripciones de Azure que ya tienen suscripciones a Azure Marketplace, esas suscripciones a Azure Marketplace no se cancelarán ni se verán afectadas.

Aunque los clientes pueden convertir sus suscripciones de Azure directas en Azure EA si las asocian a su inscripción en Azure Enterprise Portal, esta acción no convierte automáticamente las suscripciones secundarias.

Para habilitar compras en Azure Marketplace:

1. Inicie sesión en Azure Enterprise Portal como administrador de empresa.
1. Vaya a **Administrar**.
1. En **Enrollment Detail** (Detalles de la inscripción), seleccione el icono de lápiz situado junto al elemento de línea **Azure Marketplace**.
1. Cambie entre **Habilitado/Deshabilitado** o **Solo SKU BYOL o libres**, según corresponda.
1. Seleccione **Guardar**.

> [!NOTE]
> Las opciones BYOL (traiga su propia licencia) y solo gratis limitan la compra y adquisición de SKU de Azure Marketplace a solo SKU BYOL y gratuitas.

Obtenga más información sobre [Cargos de Azure Marketplace en informes de Azure Enterprise Portal](#azure-marketplace-charges-in-azure-enterprise-portal-reports).

### <a name="services-billed-hourly-for-azure-ea"></a>Servicios facturados por horas para Azure EA

Los siguientes servicios se facturan por horas en un Contrato Enterprise, en lugar de la tarifa mensual de MOSP:

- Red de entrega de aplicaciones
- Firewall de aplicaciones web

### <a name="azure-remoteapp"></a>Azure RemoteApp

Si tiene un Contrato Enterprise, paga por Azure RemoteApp según el nivel de precios del Contrato Enterprise. No existen cargos adicionales. El precio estándar incluye 40 horas iniciales. El precio ilimitado cubre 80 horas iniciales. RemoteApp deja de emitir el uso por encima de las 80 horas.

## <a name="azure-marketplace-faq"></a>P+F sobre Azure Marketplace

En esta sección se explica cómo se puede aplicar su compromiso monetario de Azure a algunos servicios de terceros revendedores en Azure Marketplace.

### <a name="what-changed-with-azure-marketplace-services-and-ea-monetary-commitment"></a>¿Qué ha cambiado con los servicios de Azure Marketplace y el compromiso monetario de EA?

A partir del 1 de marzo de 2018, algunos servicios de terceros revendedores consumen el compromiso monetario (MC) de EA. A excepción de las instancias reservadas (RI) de VM de Azure, los servicios se facturaban antes fuera del compromiso monetario de EA y se facturaban por separado.

Ampliamos el uso del compromiso monetario para incluir ahora algunos de los servicios de Azure Marketplace publicados por terceros que se compran con más frecuencia. El compromiso monetario de EA para estos servicios de Azure Marketplace simplifica las compras y la administración de los pagos.

### <a name="why-did-we-make-this-change"></a>¿Por qué hemos realizado este cambio?

Los clientes buscan continuamente otras formas de aprovechar el pago por adelantado de MC. Los clientes solicitaron este cambio a menudo, y afectó a una gran parte de los clientes de Azure Marketplace.

### <a name="how-do-you-benefit"></a>¿En qué se beneficia?

Obtiene una experiencia de facturación más sencilla y podrá gastar mejor su compromiso monetario de EA. Dado que estos servicios se incluyen en el compromiso monetario de prepago, el compromiso monetario de EA es más valioso.

### <a name="what-azure-marketplace-services-use-ea-monetary-commitment-and-how-do-i-know"></a>¿Qué servicios de Azure Marketplace usan el compromiso monetario de EA y cómo lo averiguo?

Cuando compra un servicio que usa compromiso monetario, Azure Marketplace presenta una declinación de responsabilidades. Se admiten algunos servicios publicados por Red Hat, SUSE, Autodesk y Oracle. Actualmente, los servicios con el mismo nombre publicados por otros terceros no se deducen del compromiso monetario. Al final de esta sección de preguntas frecuentes encontrará una lista completa.

### <a name="what-if-my-ea-monetary-commitment-runs-out"></a>¿Qué ocurre si se agota mi compromiso monetario de EA?

Si consume todo su compromiso monetario y pasa a uso por encima del límite, los cargos relacionados con estos servicios aparecerán en la próxima factura de uso por encima del límite, junto con cualquier otro servicio de consumo. Antes del cambio del 1 de marzo de 2018, estos cargos se facturaban con otros servicios de Azure Marketplace.

### <a name="why-dont-all-azure-marketplaces-consume-ea-monetary-commitment"></a>¿Por qué no todas las instancias de Azure Marketplace consumen compromiso monetario de EA?

Hacemos lo posible por ofrecer la mejor experiencia de cliente relacionada con el compromiso monetario de EA. Este cambio afectó un gran número de clientes y a una parte importante del gasto total en Azure Marketplace. Es posible que se agreguen otros servicios en el futuro.

### <a name="how-does-this-impact-indirect-enrollment-and-partners"></a>¿Cómo afecta esto a la inscripción indirecta y los asociados?

Nuestros asociados o clientes de inscripciones indirectas no se verán afectados. Estos servicios están sujetos a las mismas funcionalidades de marcado de asociados que otros servicios de consumo. El único cambio es que los cargos aparecen en una factura diferente, y el pago de los cargos queda fuera del compromiso monetario de EA del cliente.

### <a name="is-there-a-list-of-azure-marketplace-services-that-consume-ea-monetary-commitment"></a>¿Hay una lista de servicios de Azure Marketplace que consumen compromiso monetario de EA?

Algunas ofertas específicas de Azure Marketplace pueden usar fondos del compromiso monetario. Consulte los [servicios de terceros que usan compromiso monetario](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment) para obtener una lista completa de productos que participan en este programa.

## <a name="power-bi-reporting"></a>Informes de Power BI

Los informes de Power BI están disponibles para clientes directos, asociados e indirectos de Contrato Enterprise que tengan acceso a la información de facturación.

### <a name="power-bi-pro"></a>Power BI Pro

Power BI Pro está disponible para los clientes de EA. Con Power BI Pro, puede generar y compartir informes para administrar de forma eficaz datos de costos. También tiene características adicionales de colaboración y de actualización de datos. Power BI Pro ofrece mayor capacidad de datos y límites de streaming de datos.

<!--We plan to add new cost management features for Azure Enterprise customers.

Current Power BI (free) users who use the Microsoft Azure Consumption Insights content pack can get a 60-day free trial of Power BI Pro. After the trial is over, you can continue using Power BI Pro by adding a license.

To sign up for the free Power BI Pro trial:

1. From the gear icon in Power BI, select **Manage personal storage**.
1. Select **Try Pro for free** on the right.

See [Power BI self-service sign up](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/#power-bi-pro-60-day-trial) for more information on the Power BI Pro free trial.

### Azure EA Power BI Pro trial terms

- **General purpose**: The extended Power BI Pro for the "Microsoft Azure Enterprise" content pack trial offer (the "Offer") is available to existing qualified users during the term of the Offer, to allow them to access certain insights related to their Azure consumption through the use of a specific Power BI content pack.
- **Eligibility**: Users under an Enterprise Agreement (EA) can participate in the Offer if they have a function related to their organization's Azure billing, service, or cost management.
- **Exclusions**:
  - Users already participating in the Extended Power BI Pro trial will continue to qualify under the pre-existing offer and can't enter into the Azure EA Power BI Pro trial offer.
  - Users participating in the Offer can only use Power BI Pro with the Microsoft Azure Enterprise content pack. Any other use of Power BI Pro is prohibited.
  - Term: The Offer began on June 1, 2017 and ended on May 31, 2018.  Acceptance can occur at any time during the 12-month period, though the offer will terminate on May 31, 2018 for all users regardless of when they accepted the Offer.
  -->

### <a name="to-access-microsoft-azure-consumption-insights"></a>Para acceder a Microsoft Azure Consumption Insights:

1. Vaya a [Microsoft Azure Consumption Insights](https://app.powerbi.com/getdata/services/azureconsumption?cpcode=MicrosoftAzureConsumptionInsights&amp;getDataForceConnect=true&amp;WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26).
1. Seleccione **Obtenerla ahora**.
1. Proporcione un número de inscripción y el número de meses y, a continuación, seleccione **Siguiente**.
1. Proporcione la clave de acceso de la API para conectarse. Puede encontrar la clave para la inscripción en [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26).
1. Seleccione **Iniciar sesión** para iniciar automáticamente el proceso de importación.
1. Cuando haya finalizado, aparecerán un nuevo panel de información, un informe y un modelo en el panel de navegación. Seleccione el panel de información para ver los datos importados.

> [!TIP]
>
> - Para obtener más información sobre cómo generar la clave de API para su inscripción, consulte el archivo de ayuda de API Reports en [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26).
> - Para obtener más información sobre cómo conectar Power BI al consumo de Azure, consulte [Microsoft Azure Consumption Insights](/power-bi/desktop-connect-azure-cost-management).

### <a name="to-access-the-legacy-power-bi-ea-content-pack"></a>Para acceder al paquete de contenido de EA de Power BI heredado:

1. Vaya al [sitio web de Power BI](https://app.powerbi.com/getdata/services/azure-enterprise).
1. Inicie sesión con una cuenta profesional o educativa válida.

   La cuenta profesional o educativa puede ser la misma o distinta de la que usa para tener acceso a la inscripción mediante Azure Enterprise Portal.
1. En el panel de servicios, seleccione **Microsoft Azure Enterprise** y, luego, **Conectar**.
1. En la pantalla **Conectar con Azure Enterprise**, rellene los campos:
    - Dirección URL del entorno de Azure: [https://ea.azure.com](https://ea.azure.com/)
    - Número de meses: de 1 a 36
    - Número de inscripción: el número de inscripción
1. Seleccione **Next** (Siguiente).
1. En el cuadro **Clave de autenticación**, escriba la clave de API.

    Puede obtener la clave de API en Azure Enterprise Portal, en la pestaña **Descargar uso**. Seleccione **API Access Key** (Clave de acceso de la API) y, a continuación, pegue la clave en el cuadro **Clave de cuenta**.
1. Los datos tardarán aproximadamente de 5 a 30 minutos en cargarse en Power BI, según el tamaño de los conjuntos de datos.

## <a name="reports-faq"></a>Preguntas más frecuentes sobre informes

En esta sección se tratan las preguntas comunes sobre los informes.

### <a name="why-is-my-cost-showing-as-0"></a>¿Por qué se muestra el costo como $0?

Por los clientes de **inscripción directa**, los administradores de empresa pueden proporcionar a los propietarios de la cuenta y administradores de departamento acceso a la información de costos y precios en los informes de uso. Siga estos pasos:

1. En Azure Enterprise Portal, seleccione **Administrar** en el panel de navegación izquierdo.
1. Seleccione el lápiz azul que hay junto a DA View Charges (El administrador del departamento ve los cargos).
1. Seleccione **Enabled** (Habilitado) y guarde.
1. Seleccione el lápiz azul que hay junto a AO View Charges (El propietario de la cuenta ve los cargos).
1. Seleccione **Enabled** (Habilitado) y guarde.

> [!NOTE]
> Si es propietario de la cuenta o administrador de departamento, póngase en contacto con su administrador de empresa para habilitar la característica de precios.

Para los clientes de **inscripción indirecta**, consulte con su partner para comprobar que haya habilitado la característica de precios por usted. Esto solo lo puede realizar el partner. Una vez habilitado, puede ver los costos y los precios de su inscripción como administrador de empresa.

Si un partner desea habilitar la característica para ver los cargos para un administrador de la cuenta o administrador de departamento, debe seguir los pasos descritos en **inscripción directa**.

### <a name="why-is-there-no-sku-information-on-my-usage-detail-report"></a>¿Por qué no hay información de SKU en el informe de detalles de uso?

El informe de detalles de uso no contiene información de SKU. Sin embargo, el informe sí contiene información de uso para que pueda descargar el informe de la hoja de precios para obtener la información de las SKU.

### <a name="why-doesnt-the-total-amount-on-azure-marketplace-match-the-reports-for-usage-summary-and-detail"></a>¿Por qué la cantidad total en Azure Marketplace no coincide con los informes de resumen y detalles de uso?

El informe de cargos de Azure Marketplace muestra solo los cargos basados en el uso. No se muestran las tarifas puntuales. Consulte la página de resumen de uso para ver los cargos más actualizados basados en el uso y las tarifas puntuales.

### <a name="why-is-there-no-information-on-my-api-report"></a>¿Por qué no hay información en el informe de la API?

Las claves de API expiran cada seis meses. Si tiene algún problema, un administrador de empresa debe generar una nueva clave de API. Recuerde seguir los pasos descritos en las preguntas más frecuentes sobre informes de API.

### <a name="why-isnt-my-power-bi-report-working"></a>¿Por qué mi informe de Power BI no funciona?

Si tiene problemas con Power BI, registre un vale en el [equipo de Soporte técnico de Power BI](https://support.powerbi.com).

### <a name="why-dont-my-resource-tags-show-on-my-reports"></a>¿Por qué las etiquetas de recursos no aparecen en mis informes?

Las etiquetas de recursos se administran en Azure Portal. Puede ponerse en contacto con el equipo de suscripciones de Azure en [Azure Portal](https://portal.azure.com). Siga los pasos descritos en el artículo [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

### <a name="why-does-my-resource-rate-change-every-day"></a>¿Por qué cambia la tasa de recursos cada día?

La tasa de recursos que se muestra en el informe de detalles de uso es un valor calculado. Representa la tasa mensual media que se cargó por el servicio. Esta tasa de recursos se calcula a partir del promedio del compromiso mensual y los cargos mensuales de uso por encima del límite de una unidad de servicio. La parte del uso que se aplica a las tasas de uso por encima del límite y al compromiso cambia hasta el día en que se cierre el mes. Por lo tanto, la tasa de recursos que se enumera también cambia durante el mes. La tasa de recursos se bloquea al quinto día después del final del mes.

### <a name="glossary-of-processes-for-calculating-the-resource-rate"></a>Glosario de procesos para calcular la tasa de recursos

- **Total de unidades sin procesar:** Cantidad consumida en el informe de uso detallado.
- **Recurso de MOCP por unidad:** Sistema de uso ascendente que emite los usos de cada servicio en distintas unidades. (Valor preestablecido)
- **Consumo por unidad:** Unidad de medida de Azure Enterprise. (Valor preestablecido)
- **Precio:** Precio unitario de Azure Enterprise Portal.
- **Costo total:** Costo ampliado del informe de uso detallado o del uso del compromiso más el uso por encima del límite de Azure Enterprise Portal.

### <a name="charges-calculations"></a>Cálculos de los cargos

- **Convertir a recursos de MOCP consumidos** = `ROUND(Total RAW Units * MOCP Resource Per Unit,4)`
- **Convertir a unidades consumidas** = `Consumed MOCP Resources / Consumption per Unit`
- **Calcular costo total** = `Consumed Units * Price`

### <a name="logic-in-the-usage-calculation-logic"></a>Lógica del cálculo de uso

**Tasa de recursos** = `Total Cost /(Total RAW Units / MOCP Resource Per Unit)`

La tasa de recursos se obtiene en función de los cargos. Podría no coincidir con el precio unitario real que aparece en la hoja de precios.

En el informe de descarga de datos de uso, puede ver el uso de recursos sin procesar hasta seis lugares decimales. Estos datos se usan para el cálculo de los cargos de uso por encima del límite. Sin embargo, los datos de uso que se muestran en Azure Enterprise Portal se redondean hasta cuatro posiciones decimales para las unidades de compromiso y se truncan en cero decimales para las unidades de uso por encima del límite. En Azure Enterprise Portal, todo el uso por encima del límite se cobra por las unidades completas únicamente. Puede que vea una gran diferencia entre el precio unitario y la tasa de recursos para el uso que se cobra como uso por encima del límite o en meses mixtos.

## <a name="next-steps"></a>Pasos siguientes

- Los siguientes archivos de Excel proporcionan detalles sobre los servicios de Azure y se actualizan los días 6 y 20 de cada mes:

   | Título | Descripción | Nombre de archivo |
   | --- | --- | --- |
   | [Friendly Service Names](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx) | Enumera todos los servicios activos e incluye: <br>  <ul><li>categoría del servicio</li>   <li>nombre descriptivo del servicio</li>   <li>nombre del compromiso y número de pieza</li> <li>nombre del consumo y número de pieza</li>   <li>unidad de medida</li>   <li>factores de conversión entre el uso informado y el uso de Enterprise Portal que se muestra</li></ul> | Friendly\_Service\_Names.xlsx |
   | [Service Download Fields](https://azurepricing.blob.core.windows.net/supplemental/Service_Download_Fields.xlsx) | Esta hoja de cálculo proporciona una lista de todas las posibles combinaciones de campos relacionados con el servicio del informe de descarga de uso. | Service\_Download\_Fields.xlsx |

- Para más información sobre la facturación y los cargos, consulte [Descripción de la factura de Azure Enterprise](../understand/review-enterprise-agreement-bill.md).
- Para empezar a usar Azure Enterprise Portal, consulte [Introducción al portal del Contrato Enterprise de Azure](ea-portal-get-started.md).
