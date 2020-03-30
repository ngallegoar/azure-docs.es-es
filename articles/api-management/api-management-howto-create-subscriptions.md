---
title: Creación de suscripciones en Azure API Management | Microsoft Docs
description: Aprenda a crear suscripciones en Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: f8b2238eb0fab9aeeb42d11b4176c0d681b5f8e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70073536"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Creación de suscripciones en Azure API Management

Al publicar las API mediante Azure API Management, es sencillo y frecuente proteger el acceso a ellas mediante claves de suscripción. Las aplicaciones cliente que necesiten usar las API publicadas deben incluir una clave de suscripción válida en las solicitudes HTTP al realizar llamadas a esas API. Para obtener una clave de suscripción para acceder a las API se necesita una suscripción. Para más información sobre las suscripciones, consulte [Suscripciones en Azure API Management](api-management-subscriptions.md).

Este artículo le guiará por los pasos necesarios para crear suscripciones en Azure Portal.

## <a name="prerequisites"></a>Prerequisites

Para aprovechar los pasos de este artículo, los requisitos previos son los siguientes:

+ [Crear una instancia de API Management](get-started-create-service-instance.md).
+ Entender las [suscripciones en API Management](api-management-subscriptions.md).

## <a name="create-a-new-subscription"></a>Crear una nueva suscripción

1. Seleccione **Suscripciones** en el menú de la izquierda.
2. Seleccione **Agregar suscripción**.
3. Proporcione un nombre de suscripción y seleccione el ámbito.
4. Si lo desea, elija si la suscripción se debe asociar a un usuario.
5. Seleccione **Guardar**.

![Suscripciones flexibles](./media/api-management-subscriptions/flexible-subscription.png)

Después de crear la suscripción, se proporcionan dos claves para acceder a las API. Una clave es la principal y la otra, la secundaria. 

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre API Management:

+ Aprenda otros [conceptos](api-management-terminology.md) en API Management.
+ Siga nuestros [tutoriales](import-and-publish.md) para más información sobre API Management.
+ Consulte nuestra [página de preguntas frecuentes](api-management-faq.md) para ver las preguntas habituales.