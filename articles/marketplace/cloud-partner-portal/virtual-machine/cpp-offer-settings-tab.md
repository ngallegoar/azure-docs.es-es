---
title: Pestaña Configuración de oferta de máquina virtual en Cloud Partner Portal para Azure Marketplace
description: Se describe la pestaña Configuración de oferta que se usa en la creación de una oferta de máquina virtual de Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: e5353797aaa7d40e1b6a95685b79cb3fe99f5ecd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277657"
---
# <a name="virtual-machine-offer-settings-tab"></a>Pestaña Configuración de oferta de máquina virtual

La página **Nueva oferta** para las máquinas virtuales se abre por la primera pestaña denominada **Configuración de oferta**.  

![Página Nueva oferta para máquinas virtuales](./media/publishvm_004.png)


## <a name="offer-settings-fields"></a>Campos de Configuración de oferta

En la pestaña **Configuración de oferta**, debe proporcionar los siguientes campos.  Un asterisco (*) anexo al nombre del campo indica que es obligatorio. 

|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
| **Id. de oferta\***   | Identificador único (en el perfil de un publicador) de la oferta. Este identificador será visible en las direcciones URL de producto, las plantillas de Azure Resource Manager y los informes de facturación. Tiene una longitud máxima de 50 caracteres, solo puede estar formado por caracteres alfanuméricos en minúscula y guiones (-), pero no puede terminar con un guión. Este campo no se puede modificar una vez lanzada una oferta. <br> Por ejemplo, si Contoso publica una oferta con el Id. de oferta **sample-vm**, se le asigna la dirección URL de Azure Marketplace `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview`. |
| **Publicador\***  | Identificador único de la organización en Azure Marketplace. Todas las ofertas se deben asociar al identificador de publicador. Este valor no se puede modificar una vez que se haya guardado la oferta. |
| **Nombre\***       | Nombre para mostrar de la oferta. Este nombre se mostrará en Azure Marketplace y en Cloud Partner Portal. Puede tener un máximo de 50 caracteres. Aquí se recomienda incluir un nombre de marca que identifique el producto. No incluya aquí el nombre de la organización a menos que sea así como se comercializa. Si comercializa esta oferta en otros sitios web y publicaciones, asegúrese de que el nombre sea exactamente el mismo en todas las publicaciones. |
|   |   |
 
Haga clic en **Guardar** después de proporcionar todos los campos. 


## <a name="next-steps"></a>Pasos siguientes

En la pestaña siguiente, agregará las [SKU](./cpp-skus-tab.md) a la oferta.
