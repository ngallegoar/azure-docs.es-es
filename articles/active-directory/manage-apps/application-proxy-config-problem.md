---
title: Problema al crear una aplicación de proxy de aplicación | Microsoft Docs
description: Cómo solucionar los problemas que surgen al crear aplicaciones de proxy de aplicación en el portal de administración de Azure AD
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/21/2018
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2593f80db4c271d6ae4773f324cc9777e5400550
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84764968"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problema al crear una aplicación de proxy de aplicación 

A continuación, se muestran algunos de los problemas habituales a los que se enfrentan los usuarios cuando crean una aplicación de proxy de aplicación.

## <a name="recommended-documents"></a>Documentos recomendados 

Para más información sobre la creación de una aplicación de proxy de aplicación mediante el portal de administración, consulte [Publicación de aplicaciones mediante el proxy de aplicación de Azure AD](application-proxy-add-on-premises-application.md).

Si está siguiendo los pasos de ese documento y obtiene un error al crear la aplicación, consulte los detalles del error para obtener información y sugerencias para corregir la aplicación. La mayoría de los mensajes de error incluyen una sugerencia de corrección. 

## <a name="specific-things-to-check"></a>Aspectos específicos que comprobar

Para evitar errores habituales, compruebe que:

-   Sea un administrador con permiso para crear una aplicación de proxy de aplicación;

-   La dirección URL interna sea única;

-   La dirección URL externa sea única;

-   Las direcciones URL empiecen por http o https y terminen en "/";

-   La dirección URL debe ser un nombre de dominio y no una dirección IP.

El mensaje de error debería aparecer en la esquina superior derecha cuando cree la aplicación. También puede seleccionar el icono de notificación para ver los mensajes de error.

   ![Mensaje de notificación](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Pasos siguientes
[Habilitación del proxy de aplicación en Azure Portal](application-proxy-add-on-premises-application.md)
