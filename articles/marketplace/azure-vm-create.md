---
title: Cree una oferta de máquina virtual de Azure en Azure Marketplace.
description: Obtenga información sobre cómo crear una oferta de máquina virtual en el marketplace comercial de Microsoft.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 10/20/2020
ms.openlocfilehash: 5964292d7037465456efe22283a5552a2bfc7ad8
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331267"
---
# <a name="how-to-create-a-virtual-machine-offer-on-azure-marketplace"></a>Creación de una oferta de máquina virtual de Azure en Azure Marketplace

En este artículo se explica cómo crear una oferta de máquina virtual de Azure para [Azure Marketplace](https://azuremarketplace.microsoft.com/). Se dirige a máquinas virtuales basadas en Windows y en Linux que contienen un sistema operativo, un disco duro virtual (VHD) y hasta 16 discos de datos.

Antes de comenzar, [cree una cuenta de marketplace comercial en el Centro de partners](partner-center-portal/create-account.md). Asegúrese de que la cuenta esté inscrita en el programa Marketplace comercial.

## <a name="before-you-begin"></a>Antes de empezar

Si todavía no lo ha hecho, revise [Planeación de una oferta de máquina virtual](marketplace-virtual-machines.md). Ahí se explican los requisitos técnicos de la máquina virtual y se enumera la información y los recursos que necesitará al crear la oferta. 

## <a name="create-a-new-offer"></a>Crear una nueva oferta

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard/home).
2. En el panel izquierdo, seleccione **Marketplace comercial** > **Información general** .
3. En la página **Información general** , seleccione **Nueva oferta** > **Máquina virtual de Azure** .

    ![Captura de pantalla que muestra las opciones de menú del panel izquierdo y el botón "Nueva oferta".](./media/create-vm/new-offer-azure-virtual-machine.png)

> [!NOTE]
> Una vez publicada la oferta, las modificaciones que realice en el Centro de partners aparecerán en Azure Marketplace solo después de volver a publicarla. Tras realizar cambios en la oferta, asegúrese siempre de volver a publicarla.

Escriba un **Identificador de oferta** . Se trata de un identificador único para cada oferta de su cuenta.

- Este identificador se muestra a los clientes en la dirección web de la oferta de Azure Marketplace y en Azure PowerShell y la CLI de Azure, si procede.
- Use solo letras minúsculas y números. El identificador puede incluir guiones y caracteres de subrayado, pero no espacios, y está limitado a 50 caracteres. Por ejemplo, si escribe **test-offer-1** , la dirección web de la oferta será `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- El identificador de oferta no se puede cambiar después de seleccionar **Crear** .

Escriba un **Alias de la oferta** . El alias de la oferta es el nombre que se usa para la oferta en el Centro de partners.

- Este nombre no se utiliza en Azure Marketplace. Es diferente del nombre de la oferta y de otros valores que se muestran a los clientes.

## <a name="enable-a-test-drive-optional"></a>Habilitación de una versión de prueba (opcional)

Una versión de prueba es una excelente manera de exhibir la oferta a posibles clientes al concederles acceso a un entorno preconfigurado durante un número fijo de horas. La oferta de una versión de prueba tiene como resultado una mayor tasa de conversión y genera clientes potenciales muy cualificados. Para más información sobre las versiones de prueba, consulte [¿Qué es una versión de prueba?](partner-center-portal/test-drive.md).

> [!TIP]
> Una versión de prueba es diferente de una evaluación gratuita. Puede ofrecer una versión de prueba, una evaluación gratuita o ambas. Las dos proporcionan a los clientes la solución durante un período fijo. Sin embargo, una versión de prueba también incluye un práctico recorrido autoguiado por las principales características y ventajas del producto demostradas en un escenario de implementación real.

**Habilitación de una versión de prueba**
1.  En **Versión de prueba** , active la casilla **Habilitar una versión de prueba** .
1.  En la lista que aparece, seleccione el tipo de versión de prueba.

## <a name="configure-lead-management"></a>Configuración de la administración de clientes potenciales

Al publicar la oferta en el marketplace comercial con el Centro de partners, conéctela a un sistema de administración de relaciones con clientes (CRM). Esto le permite recibir información de contacto del cliente en cuanto alguien expresa interés en el producto o lo usa. Es necesario conectarse a un CRM si desea habilitar una versión de prueba (consulte la sección anterior). De lo contrario, la conexión a CRM es opcional.

1. En **Clientes potenciales** , seleccione el vínculo **Conectar** .
1. En el cuadro de diálogo **Detalles de conexión** , seleccione un destino de cliente potencial de la lista.
1. Rellene los campos que aparecen. Para más información, consulte los siguientes artículos:

   - [Configuración de la oferta para enviar clientes potenciales a la tabla de Azure](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md#configure-your-offer-to-send-leads-to-the-azure-table)
   - [Configuración de la oferta para enviar clientes potenciales a Dynamics 365 Customer Engagement](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md#configure-your-offer-to-send-leads-to-dynamics-365-customer-engagement) (antes Dynamics CRM Online)
   - [Configuración de la oferta para enviar clientes potenciales al punto de conexión HTTPS](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md#configure-your-offer-to-send-leads-to-the-https-endpoint)
   - [Configuración de la oferta para enviar clientes potenciales al Marketo](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md#configure-your-offer-to-send-leads-to-marketo)
   - [Configurar la oferta para enviar clientes potenciales a Salesforce](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md#configure-your-offer-to-send-leads-to-salesforce)

1. Para validar la configuración proporcionada, seleccione el vínculo **Validar** .
1. Para cerrar el cuadro de diálogo, seleccione **Aceptar** .

## <a name="resell-through-csps"></a>Revender mediante los CSP

Amplíe el alcance de la oferta haciendo que esté disponible para los asociados en el programa [Proveedores de soluciones en la nube](https://azure.microsoft.com/offers/ms-azr-0145p/) (CSP). Todos los planes Traiga su propia licencia (BYOL) se incorporan automáticamente al programa. También puede optar por participar en los planes que no sean BYOL.

Seleccione **Crear** para generar la oferta y continuar.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de las propiedades de la oferta de máquinas virtuales](azure-vm-create-properties.md)
- [Procedimientos recomendados para la publicación de ofertas](gtm-offer-listing-best-practices.md)