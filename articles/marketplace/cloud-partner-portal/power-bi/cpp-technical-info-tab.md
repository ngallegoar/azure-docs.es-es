---
title: Información técnica para una oferta de aplicación de Power BI | Azure Marketplace
description: Configure los campos de información técnica para una oferta de la aplicación Power BI para Microsoft AppSource Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: d814ebc34193f5d7c0c3828d32aa3d2af29f3679
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80981365"
---
# <a name="power-bi-apps-technical-info-tab"></a>Pestaña Información técnica de aplicaciones de Power BI

>[!Important]
>A partir del 13 de abril de 2020, se comenzará a trasladar la administración de las ofertas de aplicaciones de Power BI al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Información general sobre la creación de aplicaciones de Power BI](https://aka.ms/AzureCreatePBIServiceApp) para administrar las ofertas migradas.

En la página **Nueva oferta**, usa la pestaña **Información técnica** para proporcionar la dirección URL del paquete del instalador de Power BI y otra información que necesita para validar la nueva oferta.  Para la versión inicial, todas las aplicaciones de Power BI son gratuitas y están disponibles para su descarga desde AppSource. Por este motivo, no puede definir unidades de referencia de almacén (SKU) para este tipo de oferta.

![Pestaña Información técnica](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Campos de Información técnica 

En la pestaña **Información técnica**, complete los campos descritos en la tabla siguiente. Un asterisco (*) al final de la etiqueta de un campo indica que el campo es obligatorio.

|        Campo          |  Descripción                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **Installer URL\*** (Dirección URL del instalador)     | Power BI genera esta dirección URL cuando se publica la aplicación y se promueve a producción.  Para más información, consulte [Publicación de aplicaciones con paneles e informes en Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Validation instructions** (Instrucciones de validación)  |  Si lo desea, agregue instrucciones (hasta 3000 caracteres) para ayudar al equipo de validación de Microsoft a configurar, conectar y probar la aplicación. Incluya opciones de configuración típicas, cuentas, parámetros u otra información que pueda usarse para probar la opción de conexión de datos. Esta información solo está visible para el equipo de validación y solo se usa para validación.  |
| **¿Se ha creado esta aplicación como un paquete de contenido de Power BI?** | Actualmente, este campo se usa solo internamente. Deje la configuración predeterminada de **No**. Si cambia la configuración a **Sí**, podría detener el proceso de publicación.  |  
|  |  |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña [Detalles del escaparate digital](./cpp-storefront-details-tab.md), proporcione la información de marketing y legal sobre la aplicación.

