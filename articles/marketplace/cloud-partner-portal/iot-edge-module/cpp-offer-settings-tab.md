---
title: Configuración de ofertas de módulos IoT Edge de Azure | Azure Marketplace
description: Configure los valores de la oferta de un módulo IoT Edge.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 7a35277a61217dab2982d6436903cd09abfe8c7d
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984970"
---
# <a name="iot-edge-module-offer-settings-tab"></a>Pestaña Configuración de la oferta de módulo IoT Edge

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de módulos de IoT Edge al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una nueva oferta de módulo de IoT Edge](https://aka.ms/AzureCreateIoT) para administrar las ofertas migradas.

La página **Módulos IoT Edge > Nueva oferta** se abre con el foco en la pestaña **Configuración de la oferta**. 

![Página Nueva oferta de Módulos IoT Edge](./media/iot-edge-module-offer-settings-tab.png)


## <a name="offer-identity-settings"></a>Valor Identidad de la oferta

En **Identidad de la oferta**, debe proporcionar información para los campos descritos en la tabla siguiente. Un asterisco (*) junto al nombre del campo indica que es obligatorio. 

|  **Campo**       |     **Descripción**                                                          |
|  ---------       |     ---------------                                                          |
| **Id. de oferta\***       | Identificador único (en el perfil de un publicador) de la oferta. Este identificador es visible en las direcciones URL del producto y en los informes de Insights. Tiene una longitud máxima de 50 caracteres y puede usar caracteres alfanuméricos en minúscula y guiones (-). (El identificador no puede terminar con un guión). **Nota:** Este campo no se puede modificar una vez lanzada una oferta. <br> Por ejemplo, si Contoso publica una oferta con el Id. de oferta **sample-iot-edge-module**, se asigna a la dirección URL de Azure Marketplace `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-iot-edge-module?tab=Overview`. |
| **Publicador\***     | Identificador único de la organización en Azure Marketplace. Todas las ofertas se deben asociar al identificador de publicador. No se puede cambiar este valor después de guardar la oferta. |
| **Nombre\***          | Nombre para mostrar de la oferta. Este nombre se muestra en Azure Marketplace y en Cloud Partner Portal. Puede tener un máximo de 50 caracteres. Se recomienda usar un nombre de marca que identifique al producto. No incluya el nombre de la organización a menos que sea así como se comercializa el producto. Si comercializa esta oferta en otros sitios web y publicaciones, asegúrese de que el nombre sea exactamente el mismo en todas las publicaciones. |
|  |  |


Seleccione **Guardar** para guardar la configuración de la oferta.


## <a name="next-steps"></a>Pasos siguientes

Use la pestaña [SKU](./cpp-skus-tab.md) para configurar las SKU de la oferta.
