---
title: Configuración de la oferta de aplicación de Azure | Azure Marketplace
description: Defina la configuración de una oferta de aplicación de Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: d111471e909a091d8c887f13ad89d1da50cf72b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280123"
---
# <a name="azure-application-offer-settings-tab"></a>Pestaña Configuración de la oferta de aplicación de Azure

En este artículo se describe cómo establecer la configuración de la oferta de una aplicación de Azure.

La página **Aplicaciones de Azure > Nueva oferta** se abre con el foco sobre la pestaña **Configuración de la oferta**. Un asterisco (*) junto al nombre del campo indica que es obligatorio.

![Formulario Identidad de la oferta](./media/azureapp-offer-settings-tab.png)

## <a name="offer-identity-settings"></a>Valor Identidad de la oferta

En **Identidad de la oferta**, debe proporcionar información para los campos descritos en la tabla siguiente.  

|    Campo         |       Descripción                                                            |
|  ---------       |     ---------------                                                          |
| **Id. de oferta\***       | Identificador único (en el perfil de un publicador) de la oferta. Este identificador es visible en las direcciones URL del producto y en los informes de Insights. Tiene una longitud máxima de 50 caracteres y puede usar caracteres alfanuméricos en minúscula y guiones (-). (El identificador no puede terminar con un guión). **Nota:** Este campo no se puede modificar una vez lanzada una oferta. <br> Por ejemplo, si Contoso publica una oferta con el identificador de oferta **sample-container**, se le asigna la dirección URL de Azure Marketplace `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-container?tab=Overview`. |
| **Id. del anunciante\***     | Identificador único de la organización en Azure Marketplace. Todas las ofertas se deben asociar al identificador de publicador. No se puede cambiar este valor después de guardar la oferta. |
| **Nombre\***          | Nombre para mostrar de la oferta. Este nombre se muestra en Azure Marketplace y en Cloud Partner Portal. Puede tener un máximo de 50 caracteres. Se recomienda usar un nombre de marca que identifique al producto. No incluya el nombre de la organización a menos que sea así como se comercializa el producto. Si comercializa esta oferta en otros sitios web y publicaciones, asegúrese de que el nombre sea exactamente el mismo en todas las publicaciones. |
|  |  |

Seleccione **Guardar** para guardar la configuración de la oferta.

## <a name="next-steps"></a>Pasos siguientes

Use la pestaña [SKU](./cpp-skus-tab.md) para configurar las SKU de la oferta.
