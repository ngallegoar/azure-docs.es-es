---
title: Revisión de la factura del Contrato Enterprise de Azure
description: Aprenda a leer y a comprender el uso y la facturación de los contratos Enterprise de Azure.
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: tutorial
ms.date: 03/06/2020
ms.author: banders
ms.openlocfilehash: 5de6baf81f67ef6bee43b54e7aa0d62088c15be9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78898651"
---
# <a name="understand-your-azure-enterprise-agreement-bill"></a>Descripción de la factura de Azure Enterprise

Los clientes de Azure con un Contrato Enterprise reciben una factura cuando superan el crédito de la organización o usan servicios que no están incluidos en ese crédito.

El crédito de su organización incluye su compromiso monetario. El compromiso monetario es la cantidad que su organización paga por adelantado por el uso de los servicios de Azure. Para agregar fondos al compromiso monetario de su Contrato Enterprise, póngase en contacto con el administrador de la cuenta Microsoft o el distribuidor.

Este tutorial se aplica solo a los clientes de Azure con un Contrato Enterprise de Azure.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Revisar los cargos facturados
> * Revisar los cargos por uso del servicio por encima del límite
> * Revisar la factura de Marketplace

## <a name="prerequisites"></a>Prerrequisitos

Para revisar y comprobar los gastos en su factura, debe ser un administrador de empresa. Para más información, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](../manage/understand-ea-roles.md). Si no sabe quién es el administrador de la empresa para su organización, [póngase en contacto con el equipo de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="review-invoiced-charges-for-most-customers"></a>Revisión de los cargos facturados para la mayoría de los clientes

Esta sección no se aplica a los clientes de Azure en Australia, Japón ni Singapur.

Recibe una factura de Azure cuando se produce alguno de estos eventos durante el ciclo de facturación:

- **Uso por encima del límite del servicio**: los gastos por uso de la organización superan el saldo de crédito.
- **Gastos facturados por separado**: los servicios que ha usado la organización no están cubiertos por el crédito. Se le facturará por los siguientes servicios a pesar del saldo de crédito:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop
    - Usuario registrado
    - Openlogic
    - Usuario registrado de Remote Access Rights XenApp Essentials
    - Ubuntu Advantage
    - Visual Studio Enterprise (mensual)
    - Visual Studio Enterprise (anual)
    - Visual Studio Professional (mensual)
    - Visual Studio Professional (anual)
- **Cargos de Marketplace**: las compras y el uso de Azure Marketplace no están incluidos en el crédito de la organización. Por lo tanto, los cargos de Marketplace se le facturan a pesar del saldo de crédito. En Enterprise Portal, un administrador de Enterprise puede habilitar y deshabilitar las compras de Marketplace.

La factura mostrará los cargos de uso de Azure con los costos asociados en primer lugar, seguidos de los cargos de Marketplace. Si tiene un saldo de crédito, se aplica al uso de Azure y la factura mostrará dicho uso y el uso de Marketplace sin ningún costo último.

Compare el importe total combinado que se muestra en Enterprise Portal en **Informes** > **Resumen de uso** con la factura de Azure. Los importes que aparecen en **Resumen de uso** no incluyen impuestos.

Inicie sesión en el [portal del Contrato Enterprise de Azure](https://ea.azure.com). Seleccione **Informes**. En la esquina superior derecha de la pestaña, cambie la vista de **M** a **C** y haga que coincida con el período de la factura.  

![Captura de pantalla que muestra la opción M + C en Resumen de uso.](./media/review-enterprise-agreement-bill/ea-portal-usage-sumary-cm-option.png)

El importe combinado del **uso total** y **Azure Marketplace** debe coincidir con el **importe total extendido** en la factura. Para más detalles sobre los cargos, vaya a **Descarga del uso**.  

![Captura de pantalla que muestra la pestaña Descargar uso](./media/review-enterprise-agreement-bill/ea-portal-download-usage.png)

## <a name="review-invoiced-charges-for-other-customers"></a>Revisión de cargos facturados para otros clientes

Esta sección solo se aplica a los clientes de Azure en Australia, Japón o Singapur.

Recibirá una o más facturas de Azure cuando se produzcan los siguientes eventos:

- **Uso por encima del límite del servicio**: los gastos por uso de la organización superan el saldo de crédito.
- **Gastos facturados por separado**: los servicios que ha usado la organización no están cubiertos por el crédito. Se le facturarán los siguientes servicios:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop
    - Usuario registrado
    - Openlogic
    - Usuario registrado de Remote Access Rights XenApp Essentials
    - Ubuntu Advantage
    - Visual Studio Enterprise (mensual)
    - Visual Studio Enterprise (anual)
    - Visual Studio Professional (mensual)
    - Visual Studio Professional (anual)
- **Cargos de Marketplace**: las compras y el uso de Azure Marketplace no están incluidos en el crédito de la organización y se facturan por separado. En Enterprise Portal, un administrador de Enterprise puede habilitar y deshabilitar las compras de Marketplace.

Cuando tenga cargos por usos por encima del límite del servicio y cargos que se facturan por separado durante el período de facturación, recibirá una factura. Esta incluye ambos tipos de cargos. Los gastos de Marketplace siempre se facturan de forma independiente.

## <a name="review-service-overage-charges-for-other-customers"></a>Revisión de los cargos por uso por encima del límite del servicio para otros clientes

Esta sección solo se aplica si está en Australia, Japón o Singapur.

Compare el importe total por uso en Enterprise Portal en **Informes** > **Resumen de uso** con su factura por uso por encima del límite del servicio. La factura por uso por encima del límite del servicio incluye el uso que ha superado el crédito de la organización y los servicios que no están incluidos en ese crédito. Los importes que aparecen en **Resumen de uso** no incluyen impuestos.

Inicie sesión en el [portal del Contrato Enterprise de Azure](https://ea.azure.com) y seleccione **Informes**. En la esquina superior derecha de la pestaña, cambie la vista de **M** a **C** y haga que coincida con el período de la factura.  

![Captura de pantalla que muestra la opción M + C en Resumen de uso.](./media/review-enterprise-agreement-bill/ea-portal-usage-sumary-cm-option.png)

El importe de **Uso total** debe coincidir con el **Importe total extendido** en la factura por un uso por encima del límite del servicio. Para más información sobre los gastos, vaya a **Descargar uso** > **Descargar informe avanzado**. El informe no incluye los impuestos ni los gastos por reservas o de Marketplace.  

![Captura de pantalla que muestra Descargar informe avanzado en la pestaña Descargar uso.](./media/review-enterprise-agreement-bill/ea-portal-download-usage-advanced.png)

La tabla siguiente enumera los términos y las descripciones que se muestran en la factura y en el **Resumen de uso** de Enterprise Portal:

|Término de facturación|Término de Resumen de uso|Descripción|
|---|---|---|
|Importe total extendido|Uso total|El gasto por uso total antes de impuestos durante el período específico anterior a la aplicación del crédito.|
|Uso de compromiso|Uso de compromiso|El crédito que se aplica durante ese período específico.|
|Total venta|Uso por encima del límite total|El gasto por uso por encima del límite total que supera el importe del crédito. Este importe no incluye impuestos.|
|Importe de impuestos|No aplicable|Impuestos que se aplican al importe total de venta durante el período específico.|
|Importe total|No aplicable|El importe a pagar de la factura una vez aplicado el crédito y agregado los impuestos.|

### <a name="review-marketplace-invoice"></a>Revisar la factura de Marketplace

Esta sección solo se aplica si está en Australia, Japón o Singapur.

Compare el total de Azure Marketplace en **Informes** > **Resumen de uso** en Enterprise Portal con la factura de Marketplace. La factura de Marketplace es solo para el uso y las compras de Azure Marketplace. Los importes en el **Resumen de uso** ya incluyen un impuesto determinado por el publicador.

Inicie sesión en el [portal de Enterprise](https://ea.azure.com) y seleccione **Informes**. En la esquina superior derecha de la pestaña, cambie la vista de **M** a **C** y haga que coincida con el período de la factura.  

![Captura de pantalla que muestra la opción M + C en Resumen de uso.](./media/review-enterprise-agreement-bill/ea-portal-usage-sumary-cm-option.png)  

El importe total de **Azure Marketplace** debe coincidir con el importe **Total venta** en su factura de Marketplace. Para más información sobre los gastos basados en el uso, vaya a **Descargar uso**. En **Gastos de Marketplace**, seleccione **Descargar**. El precio de Marketplace incluye un impuesto determinado por el editor. Los clientes no recibirán ninguna factura independiente del editor para el cobro de impuestos por la transacción.

![Captura de pantalla que muestra la opción Descargar en Gastos de Marketplace.](./media/review-enterprise-agreement-bill/ea-portal-download-usage-marketplace.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Revisar los cargos facturados
> * Revisar los cargos por uso del servicio por encima del límite
> * Revisar la factura de Marketplace

Continúe con el siguiente artículo para más información con el uso del portal del Contrato Enterprise de Azure.

> [!div class="nextstepaction"]
> [Introducción a Azure Enterprise Portal](../manage/ea-portal-get-started.md)
