---
title: Configuración de una oferta para una aplicación de Power BI | Azure Marketplace
description: Configure una oferta para una aplicación de Power BI para Microsoft AppSource Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: ddedc15b41b2b163f85f5443275daf9fbb56bac8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82143028"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Pestaña Configuración de oferta de aplicaciones Power BI

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de aplicaciones de Power BI al Centro de partners. Después de la migración, creará y administrará sus ofertas en el Centro de partners. Siga las instrucciones que se indican en [Información general sobre la creación de aplicaciones de Power BI](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-power-bi-app-offer) para administrar las ofertas migradas.

Al abrir la página **Nueva oferta** para las aplicaciones de servicio, consulte primero la pestaña **Configuración de oferta**. En esta pestaña, proporciona los identificadores principales y el nombre de la oferta. Un asterisco (*) indica un campo obligatorio.

![Pestaña Configuración de la oferta](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Campos de Configuración de oferta 

En la pestaña **Configuración de oferta**, debe proporcionar la información en los siguientes campos obligatorios. Los campos obligatorios se indican con un asterisco (*).

|  Campo        |  Descripción                                                               |
|---------------|----------------------------------------------------------------------------|
| **Id. de oferta\***  | Identificador único (en el perfil de un publicador) de la oferta. Este identificador será visible en las direcciones URL de producto, las plantillas de Azure Resource Manager y los informes de facturación. La longitud máxima es de 50 caracteres. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). No puede terminar con un guión. Este identificador no se puede modificar una vez lanzada una oferta. Si Contoso publica una oferta con el Id. de oferta `sample-SvcApp`, se asigna a la oferta la dirección URL de AppSource `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Publicador\*** | Identificador único de la organización en [AppSource](https://appsource.microsoft.com). Todas las ofertas se deben asociar al identificador de publicador. No se puede cambiar este valor después de guardar la oferta.                         |
| **Nombre\***      | Nombre para mostrar de la oferta. Este nombre aparecerá en AppSource y en Cloud Partner Portal. La longitud máxima es de 50 caracteres. Incluya un nombre de marca con el que se reconozca su producto. No incluya aquí el nombre de la organización a menos que la aplicación se comercialice con ese nombre. Si proporciona esta oferta en otros sitios web y publicaciones, use el mismo nombre en todas las publicaciones.    <br/>Si publica una oferta durante el período de versión preliminar de las aplicaciones de Power BI, agregue la cadena `(Preview)` al final del nombre de la aplicación, de este modo: `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña siguiente, especificará la [información técnica](./cpp-technical-info-tab.md) para la oferta.
