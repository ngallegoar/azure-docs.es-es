---
title: 'Colaboración con otros usuarios: QnA Maker'
description: ''
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 967fdae49f904f6c1cb450b637a8dbc5c481b135
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776891"
---
# <a name="collaborate-with-other-authors-and-editors"></a>Colaboración con otros creadores y editores

Colabore con otros creadores y editores mediante el control de acceso basado en rol (RBAC) colocado en el recurso de QnA Maker.

## <a name="access-is-provided-on-the-qna-maker-resource"></a>El acceso se proporciona en el recurso de QnA Maker

Todos los permisos se controlan mediante los permisos colocados en el recurso de QnA Maker. Estos permisos se alinean con el acceso de lectura, escritura, publicación y completo.

Esta característica de RBAC incluye lo siguiente:
* Azure Active Directory (AAD) ofrece una compatibilidad con versiones anteriores total con la autenticación basada en claves para propietarios y colaboradores. Los clientes pueden usar la autenticación basada en claves o en RBAC en sus solicitudes.
* Agregue creadores y editores rápidamente a todas las bases de conocimiento del recurso, ya que el control se encuentra en el nivel de recursos, no en el de base de conocimiento.

## <a name="access-is-provided-by-a-defined-role"></a>El acceso lo proporciona un rol definido

[!INCLUDE [RBAC permissions table](../includes/role-based-access-control.md)]

## <a name="authentication-flow"></a>Flujo de autenticación

En el diagrama siguiente se muestra el flujo, desde la perspectiva del creador, para iniciar sesión en el portal de QnA Maker y usar las API de creación.

> [!div class="mx-imgBorder"]
> ![En el diagrama siguiente se muestra el flujo, desde la perspectiva del creador, para iniciar sesión en el portal de QnA Maker y usar las API de creación.](../media/qnamaker-how-to-collaborate-knowledge-base/rbac-flow-from-portal-to-service.png)

|Pasos|Descripción|
|--|--|
|1|El portal adquiere el token para el recurso de QnA Maker.|
|2|El portal llama a la API de creación de QnA Maker (APIM) apropiada y pasa el token en lugar de las claves.|
|3|La API de QnA Maker valida el token.|
|4 |La API de QnA Maker llama al servicio QnAMaker.|

Si tiene previsto llamar a las [API de creación](../How-To/collaborate-knowledge-base.md), obtenga más información sobre cómo configurar la autenticación.

## <a name="authenticate-by-qna-maker-portal"></a>Autenticación mediante el portal de QnA Maker

Si crea y colabora con el portal de QnA Maker, después de [agregar el rol adecuado al recurso para un colaborador](../How-To/collaborate-knowledge-base.md), en el portal de QnA Maker se administran todos los permisos de acceso.

## <a name="authenticate-by-qna-maker-apis-and-sdks"></a>Autenticación mediante API y SDK de QnA Maker

Si crea y colabora mediante API, ya sea a través de REST o los SDK, tendrá que [crear una entidad de servicio](../../authentication.md#assign-a-role-to-a-service-principal) para administrar la autenticación.

## <a name="next-step"></a>Paso siguiente

* Diseño de una base de conocimiento para [lenguajes](design-language-culture.md) o [aplicaciones cliente](integration-with-other-applications.md)
