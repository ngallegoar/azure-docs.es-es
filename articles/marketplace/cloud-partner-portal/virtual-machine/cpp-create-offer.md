---
title: Creación de una oferta de máquina virtual en Azure Marketplace
description: Se enumeran los pasos necesarios para crear una oferta de máquina virtual (VM) para Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: dsindona
ms.openlocfilehash: 9d06809df2774224b61fd3fb643ab628dd2890f6
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273994"
---
# <a name="create-virtual-machine-offer"></a>Creación de una oferta de máquina virtual

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://aka.ms/CreateAzureVMoffer) para administrar las ofertas migradas.

En esta sección se enumeran los pasos necesarios para crear una solicitud de oferta de máquina virtual (VM) para Azure Marketplace.  Todas las ofertas aparecen como una entidad propia en Azure Marketplace y se asocian a una o varias SKU.  Una oferta de máquina virtual se compone de los siguientes grupos de recursos y servicios complementarios: 

![Recursos para una oferta de máquina virtual](./media/publishvm_002.png)

donde:

|  **Grupo de recursos**   |  **Descripción**  |
|  ---------------   |  ---------------  |
|    SKU            |  La unidad de compra más pequeña de una oferta. Una única oferta (clase de producto) puede tener varias SKU asociadas, para diferenciar entre las características admitidas, los tipos de imagen de máquina virtual y los modelos de facturación. |
|  Marketplace       | Contiene recursos y especificaciones de marketing, legales y de administración de clientes potenciales.  <ul><li> Los recursos de marketing incluyen el nombre de la oferta, la descripción y los logotipos</li> <li> Los recursos legales incluyen una directiva de privacidad, términos de uso y otra documentación legal</li>  <li> La directiva de administración de clientes potenciales permite especificar cómo se controlan los clientes potenciales desde el portal del usuario final de Azure Marketplace.</li> </ul> |
| Soporte técnico            | Contiene información de contacto y directivas de soporte técnico |
| Versión de prueba         | Define los recursos que permiten a los usuarios probar la oferta antes de comprarla |
|  |  |


## <a name="new-offer-form"></a>Formulario Nueva oferta

Una vez que inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/), haga clic en el elemento **+ Nueva oferta** de la barra de menús de la izquierda. En el menú que aparece, haga clic en **Máquinas virtuales** para mostrar el formulario **Nueva oferta** e iniciar el proceso de definición de recursos para una nueva oferta de máquina virtual. 
<!-- not all publishers see corevm or azure apps test, you need to be whitelisted to see them. we should hide those in these images. -->

![Selección de interfaz de usuario de nueva oferta de máquina virtual](./media/publishvm_003.png)

> [!WARNING]
> Si no se muestra la opción **Máquinas virtuales** o no está habilitada, la cuenta no tiene permiso para crear este tipo de oferta.  Compruebe que cumple todos los [requisitos previos](./cpp-prerequisites.md) para este tipo de oferta, incluido el registro para una cuenta de desarrollador.


## <a name="next-steps"></a>Pasos siguientes

En los temas siguientes de esta sección se reflejan las pestañas de la página **Nueva oferta** (para un tipo de oferta de máquina virtual).  En cada artículo se explica cómo usar la pestaña asociada para definir los grupos de recursos y servicios complementarios para la nueva oferta de máquina virtual.

- [Pestaña Configuración de la oferta](./cpp-offer-settings-tab.md)
- [Pestaña SKU](./cpp-skus-tab.md)
- [Pestaña Versión de prueba](./cpp-test-drive-tab.md)
- [Pestaña Marketplace](./cpp-marketplace-tab.md)
- [Pestaña Soporte técnico](./cpp-support-tab.md)
