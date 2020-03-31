---
title: Crear una oferta de módulo Azure IoT Edge | Azure Marketplace
description: Cómo publicar un nuevo módulo IoT Edge para Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: dsindona
ms.openlocfilehash: 164b26560020054f4da8bd1aa0ba2a3ecff1875a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286632"
---
# <a name="create-a-new-iot-edge-module-offer-with-the-cloud-partner-portal"></a>Crear una nueva oferta de módulo IoT Edge con Cloud Partner Portal

En este artículo se explica cómo crear y publicar una entrada de oferta de módulo IoT Edge para Azure Marketplace. Todas las ofertas aparecen como una entidad propia en Azure Marketplace y se asocian a una o varias SKU.  Una oferta de módulo IoT Edge se compone de los siguientes grupos de recursos y servicios de soporte técnico:

|  **Grupo de recursos**   |  **Descripción**  |
|  ---------------   |  ---------------  |
|    SKU            |  La unidad más pequeña que se puede implementar de una oferta. Una única oferta (clase de producto) puede tener varias SKU asociadas a ella. Puede usar SKU para diferenciar entre características admitidas y modelos de facturación. |
|  Marketplace       | Contiene recursos y especificaciones de marketing, legales y de administración de clientes potenciales.  <ul><li> Los recursos de marketing incluyen el nombre de la oferta, la descripción y los logotipos</li> <li> Los recursos legales incluyen una directiva de privacidad, términos de uso y otra documentación legal</li>  <li> La directiva de administración de clientes potenciales permite especificar cómo se controlan los clientes potenciales desde el portal del usuario final de Azure Marketplace.</li> </ul> |
| Soporte técnico            | Contiene información de contacto y directivas de soporte técnico |


## <a name="new-offer-form"></a>Formulario Nueva oferta 

Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/) y seleccione **+ Nueva oferta** en la barra de menús de la izquierda. En el menú Nueva oferta, seleccione **Módulos IoT Edge** para mostrar el formulario **Nueva oferta** e iniciar el proceso de definición de recursos de una nueva oferta de módulo ioT Edge. 

![Selección de interfaz de usuario de nueva oferta de módulo IoT Edge](./media/new-iot-edge-module-offer.png)

## <a name="next-steps"></a>Pasos siguientes

La página **Nueva oferta** del tipo de oferta de módulo IoT Edge proporciona un conjunto de pestañas y campos de formulario que se van a usar para crear una nueva oferta. Cada uno de los siguientes artículos explica cómo usar la pestaña para definir los grupos de recursos y servicios de soporte técnico de la nueva oferta de módulo IoT Edge.

- [Pestaña Configuración de la oferta](./cpp-offer-settings-tab.md)
- [Pestaña SKU](./cpp-skus-tab.md)
- [Pestaña Marketplace](./cpp-marketplace-tab.md)
- [Pestaña Soporte técnico](./cpp-support-tab.md)
