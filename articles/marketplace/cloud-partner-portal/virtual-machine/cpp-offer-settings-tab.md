---
title: Pestaña Configuración de oferta de máquina virtual en Cloud Partner Portal para Azure Marketplace
description: Se describe la pestaña Configuración de oferta que se usa en la creación de una oferta de máquina virtual de Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: abeadf5f3c0a19212a12256e06602e840c9d1fa9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146823"
---
# <a name="virtual-machine-offer-settings-tab"></a>Pestaña Configuración de oferta de máquina virtual

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) para administrar las ofertas migradas.

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
