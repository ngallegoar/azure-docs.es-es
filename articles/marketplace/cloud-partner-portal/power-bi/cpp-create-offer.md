---
title: Creación de una oferta de aplicación de Power BI | Azure Marketplace
description: Cómo crear una oferta de aplicación de Power BI para Microsoft AppSource Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: c396468bff0edef5cfceccd3d3c3394662111c10
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984851"
---
# <a name="create-a-power-bi-app-offer"></a>Creación de una oferta de aplicación de Power BI

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de aplicaciones de Power BI al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Información general sobre la creación de aplicaciones de Power BI](https://aka.ms/AzureCreatePBIServiceApp) para administrar las ofertas migradas.

En este artículo se enumeran los pasos necesarios para crear una oferta de aplicación de Power BI para [AppSource](https://appsource.microsoft.com). Cada oferta aparece como su propia entidad en AppSource. Al crear una oferta en [Cloud Partner Portal](https://cloudpartner.azure.com/), deben proporcionarse cuatro grupos de recursos para ella.

Los grupos de recursos se describen en la siguiente tabla:

|   Grupo de recursos      | Descripción                                                                         |
| ----------------   | ----------------                                                                    |
| Configuración de oferta     | Identificaciones principales y nombre de la oferta.                                      |
| Información técnica     | Dirección URL del instalador usado para instalar la aplicación en el área de trabajo de Power BI del cliente. Para más información sobre cómo generar esta dirección URL, consulte la [documentación de la aplicación de Power BI](https://go.microsoft.com/fwlink/?linkid=2028636). |
| Detalles del escaparate | Contiene recursos de marketing, legales y de administración de clientes potenciales. Los recursos de marketing incluyen la descripción y los logotipos de la oferta. Los recursos legales incluyen una directiva de privacidad, términos de uso y otra documentación legal. La directiva de administración de clientes potenciales permite especificar cómo se controlan los clientes potenciales desde el portal del usuario de AppSource. |
| Contactos           | Contiene información de contacto y directivas de soporte técnico.                                     |

## <a name="new-offer-form"></a>Formulario Nueva oferta

Tras haber iniciado sesión en Cloud Partner Portal, seleccione **Nueva oferta** en el panel de la izquierda. Para mostrar el formulario Nueva oferta e iniciar el proceso de definición de recursos para una nueva oferta de aplicación, seleccione **Power BI Apps** (Aplicaciones de Power BI).

![Elemento de menú de oferta de Power BI](./media/new-offer-menu.png)

> [!NOTE] 
> Si no se muestra la opción **Power BI Apps** (Aplicaciones de Power BI) o no está habilitada, la cuenta no tiene permiso para crear este tipo de oferta. Asegúrese de que cumple todos los [requisitos previos](./cpp-prerequisites.md) para este tipo de oferta, incluido el registro para una cuenta de desarrollador.


## <a name="next-steps"></a>Pasos siguientes

En los siguientes artículos se explican las pestañas de la página **Nueva oferta** para un tipo de oferta Aplicación de Power BI. En cada artículo se describen los grupos de recursos y servicios de soporte técnico para la nueva oferta de aplicación de Power BI.

-  [Pestaña Configuración de la oferta](./cpp-offer-settings-tab.md)
-  [Pestaña Información técnica](./cpp-technical-info-tab.md)
-  [Pestaña Detalles del escaparate](./cpp-storefront-details-tab.md)
-  [Pestaña Contactos](./cpp-contacts-tab.md)
