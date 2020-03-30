---
title: Especificación de imágenes de Marketplace para un laboratorio en Azure Lab Services
description: En este artículo se muestra cómo especificar las imágenes de Marketplace que el creador del laboratorio puede usar para crear laboratorios en una cuenta de laboratorio de Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: ad56041f853d030e3a286610fe4872bffecaee12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444897"
---
# <a name="specify-marketplace-images-available-to-lab-creators"></a>Especificación de las imágenes de Marketplace disponibles para los creadores de laboratorios
Como propietario de una cuenta de laboratorio, puede especificar las imágenes de Marketplace que los creadores de laboratorios pueden usar para este fin en la cuenta de laboratorio. 

## <a name="select-images-available-for-labs"></a>Selección de las imágenes disponibles para los laboratorios
Seleccione **Imágenes de Marketplace** en el menú a la izquierda. De forma predeterminada, aparecerá la lista completa de las imágenes (habilitadas y deshabilitadas). Puede filtrar la lista para ver solamente las imágenes habilitadas o deshabilitadas seleccionando la opción **Enabled only**(Solo habilitadas)/**Disabled only** (Solo deshabilitadas) en la lista desplegable en la parte superior. 
    
![Página de imágenes de Marketplace](../media/tutorial-setup-lab-account/marketplace-images-page.png)

Las imágenes de Marketplace que se muestran en la lista son las únicas que cumplen las condiciones siguientes:
    
- Crea una sola máquina virtual.
- Utiliza Azure Resource Manager para aprovisionar las máquinas virtuales.
- No requiere la compra de un plan de licencias adicional.

## <a name="disable-images-for-a-lab"></a>Deshabilitar imágenes para un laboratorio 
Para deshabilitar una sola imagen para un laboratorio, seleccione **... (puntos suspensivos)**  en la última columna y seleccione **Deshabilitar imagen**. 

![Deshabilitar una imagen](../media/tutorial-setup-lab-account/disable-one-image.png) 

También puede activar la casilla que aparece delante del nombre de la imagen y seleccionar **Deshabilitar las imágenes seleccionadas** en la barra de herramientas. 

Para deshabilitar varias imágenes al mismo tiempo, seleccione las casillas que aparecen delante de los nombres de las imágenes y seleccione **Deshabilitar las imágenes seleccionadas** en la barra de herramientas. 

![Deshabilitar varias imágenes](../media/tutorial-setup-lab-account/disable-multiple-images.png) 


## <a name="enable-images-for-a-lab"></a>Habilitar imágenes para un laboratorio
Para habilitar una imagen deshabilitada, seleccione **... (puntos suspensivos)**  en la última columna y seleccione **Habilitar imagen**. También puede activar la casilla que aparece delante del nombre de la imagen y seleccionar **Habilitar las imágenes seleccionadas** en la barra de herramientas. 

Para deshabilitar varias imágenes al mismo tiempo, active las casillas que aparecen delante de los nombres de las imágenes y seleccione **Deshabilitar las imágenes seleccionadas** en la barra de herramientas. 

## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
