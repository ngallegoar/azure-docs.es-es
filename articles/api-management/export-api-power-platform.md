---
title: Exportación de las API de Azure API Management a Power Platform | Microsoft Docs
description: Aprenda a exportar API desde API Management a Power Platform.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/01/2020
ms.author: apimpm
ms.openlocfilehash: 5814ff3909aa36ccfd9e14033f200fb4d3b8d32d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86252867"
---
# <a name="export-apis-from-azure-api-management-to-the-power-platform"></a>Exportación de API de Azure API Management a Power Platform 

Los desarrolladores cívicos que usan Microsoft [Power Platform](https://powerplatform.microsoft.com) a menudo necesitan acceder a las funcionalidades empresariales que desarrollan los desarrolladores profesionales y se implementan en Azure. [Azure API Management](https://aka.ms/apimrocks) permite a los desarrolladores profesionales publicar su servicio back-end como API y exportar fácilmente estas API a Power Platform (Power Apps y Power Automate) como conectores personalizados para que los desarrolladores cívicos los usen. 

Este artículo le guía por los pasos para exportar API desde API Management a Power Platform. 

## <a name="prerequisites"></a>Prerrequisitos

+ Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)
+ Asegúrese de que haya una API en la instancia de API Management que desea exportar a Power Platform.
+ Asegúrese de que tiene un [entorno](/powerapps/powerapps-overview#power-apps-for-admins) de Power Apps o Power Automate. 

## <a name="export-an-api"></a>Exportación de una API

1. Vaya al servicio API Management en Azure Portal y seleccione las **API** en el menú.
2. Haga clic en los tres puntos situados junto a la API que desee exportar. 
3. Seleccione **Export** (Exportar).
4. Seleccione **Power Apps y Power Automate**.
5. Elija un entorno al que exportar la API. 
6. Proporcione un nombre para mostrar, que se usará como nombre del conector personalizado.  
7. Si lo desea, si la API está protegida por un servidor OAuth 2.0, también tendrá que proporcionar detalles adicionales, como `Client ID`, `Client secret`, `Authorization URL`, `Token URL` y `Refresh URL`.  
8. Seleccione **Export** (Exportar). 

Una vez finalizada la exportación, vaya a la aplicación Power App o al entorno de Power Automate. Verá la API como un conector personalizado.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre Power Platform](https://powerplatform.microsoft.com/)
* [Obtenga información sobre las tareas comunes en API Management siguiendo los tutoriales.](./import-and-publish.md)
