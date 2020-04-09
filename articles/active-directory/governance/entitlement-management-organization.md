---
title: 'Agregar una organización conectada en la administración de derechos de Azure AD: Azure Active Directory'
description: Obtenga información acerca de cómo permitir que los usuarios ajenos a la organización soliciten los paquetes de acceso para que puedan colaborar en proyectos.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee370bc9c381eb11ae7cae53b31d0c987c52733c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128609"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Agregar una organización conectada en la administración de derechos de Azure AD

La administración de derechos de Azure Active Directory (Azure AD) le permite colaborar con personas ajenas a la organización. Si colabora con frecuencia con usuarios en un dominio o directorio externo de Azure AD, puede agregarlos como una organización conectada. En este artículo se describe cómo agregar una organización conectada para que pueda permitir que los usuarios ajenos a la organización soliciten recursos en el directorio.

## <a name="what-is-a-connected-organization"></a>¿Qué es una organización conectada?

Una organización conectada es un dominio o directorio externo de Azure AD con el que tiene relación.

Por ejemplo, supongamos que trabaja en Woodgrove Bank y desea colaborar con dos organizaciones externas. Estas dos organizaciones tienen configuraciones diferentes:

- Graphic Design Institute usa Azure AD y sus usuarios tienen un nombre principal de usuario que termina en *graphicdesigninstitute.com*.
- Contoso todavía no usa Azure AD. Los usuarios de Contoso tienen un nombre principal de usuario que termina en *contoso.com*.

En este caso, puede configurar dos organizaciones conectadas. Crea una organización conectada para Graphic Design Institute y otra para Contoso. Si después agrega las dos organizaciones conectadas a una directiva, los usuarios de cada organización con un nombre principal de usuario que coincida con la directiva podrán solicitar paquetes de acceso. Los usuarios con un nombre principal de usuario que tenga un dominio de *graphicdesigninstitute.com* coincidirían con la organización conectada con Graphic Design Institute y podrían enviar solicitudes. Los usuarios con un nombre principal de usuario que tenga un dominio de *contoso.com* coincidirían con la organización conectada con Contoso y también podrían solicitar paquetes. Además, dado que Graphic Design Institute usa Azure AD, cualquier usuario con un nombre principal que coincida con un [dominio comprobado](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) que se agregue al inquilino, como*graphicdesigninstitute.example*, también podría solicitar paquetes de acceso mediante la misma directiva.

![Ejemplo de una organización conectada](./media/entitlement-management-organization/connected-organization-example.png)

La forma en que los usuarios del directorio de Azure AD o el dominio se autentican depende del tipo de autenticación. Los tipos de autenticación para las organizaciones conectadas son:

- Azure AD
- [Federación directa](../b2b/direct-federation.md)
- [Código de acceso de un solo uso](../b2b/one-time-passcode.md) (dominio)

Para ver una demostración de cómo agregar una organización conectada, vea el vídeo siguiente:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Adición de una organización conectada

Para agregar un directorio o dominio externo de Azure AD como organización conectada, siga las instrucciones de esta sección.

**Rol necesario**: *Administrador global*, *Administrador de usuarios* o *Invitador de usuarios invitados*

1. En Azure Portal, seleccione **Azure Active Directory** y, a continuación, **Gobernanza de identidades**.

1. En el panel de la izquierda, seleccione **Organizaciones conectadas** y, a continuación, **Agregar organización conectada**.

    ![El botón "Agregar organización conectada"](./media/entitlement-management-organization/connected-organization.png)

1. Seleccione la pestaña **Aspectos básicos** y, a continuación, escriba un nombre para mostrar y una descripción para la organización.

    ![Panel Aspectos básicos de "Agregar organización conectada"](./media/entitlement-management-organization/organization-basics.png)

1. Seleccione la pestaña **Directorio y dominio** y, a continuación, **Agregar directorio y dominio**.

    Se abre el panel **Seleccionar directorios y dominios**.

1. En el cuadro de búsqueda, escriba un nombre de dominio para buscar el directorio o el dominio de Azure AD. Asegúrese de escribir el nombre de dominio completo.

1. Compruebe que el nombre de la organización y el tipo de autenticación son correctos. El modo en que los usuarios inician sesión depende del tipo de autenticación.

    ![Panel "Seleccionar directorios y dominios"](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Seleccione **Agregar** para agregar el directorio o dominio de Azure AD. Actualmente, solo puede agregar un directorio o dominio de Azure AD por organización conectada.

    > [!NOTE]
    > Todos los usuarios del directorio o dominio de Azure AD podrán solicitar este paquete de acceso. Esto incluye a los usuarios de Azure AD de todos los subdominios asociados con el directorio, a menos que los dominios estén bloqueados en la lista de permitidos o denegados de B2B de Azure AD. Para obtener más información, consulte [Allow or block invitations to B2B users from specific organizations](../b2b/allow-deny-list.md) (Permitir o bloquear invitaciones a usuarios de B2B procedentes de determinadas organizaciones).

1. Después de agregar el directorio o dominio de Azure AD, elija **Seleccionar**.

    La organización aparece en la lista.

    ![Panel Directorio y dominio](./media/entitlement-management-organization/organization-directory-domain.png)

1. Seleccione la pestaña **Patrocinadores** y, a continuación, agregue patrocinadores opcionales para esta organización conectada.

    Los patrocinadores son usuarios internos o externos ya existentes en el directorio que son el punto de contacto para la relación con esta organización conectada. Los patrocinadores internos son usuarios miembros de su directorio. Los patrocinadores externos son usuarios invitados de la organización conectada a los que se ha invitado previamente y que ya están en el directorio. Los patrocinadores se pueden usar como aprobadores cuando los usuarios de esta organización conectada soliciten acceso a este paquete de acceso. Para obtener información sobre cómo traer a un usuario invitado a su directorio, consulte [Agregar usuarios de colaboración de Azure Active Directory B2B en el Azure Portal](../b2b/add-users-administrator.md).

    Al seleccionar **Agregar o quitar**, se abre un panel en el que puede elegir patrocinadores internos o externos. El panel muestra una lista sin filtrar de usuarios y grupos en el directorio.

    ![Panel Patrocinadores](./media/entitlement-management-organization/organization-sponsors.png)

1. Seleccione la pestaña **Revisar y crear**, revise la configuración de la organización y, luego, elija **Crear**.

    ![Panel Revisar y crear](./media/entitlement-management-organization/organization-review-create.png)

## <a name="update-a-connected-organization"></a>Actualización de una organización conectada 

Si la organización conectada cambia a otro dominio, el nombre de la organización cambia o quiere cambiar los patrocinadores, puede seguir las instrucciones de esta sección para actualizar la organización conectada.

**Rol necesario**: *Administrador global*, *Administrador de usuarios* o *Invitador de usuarios invitados*

1. En Azure Portal, seleccione **Azure Active Directory** y, a continuación, **Gobernanza de identidades**.

1. En el panel de la izquierda, seleccione **Organizaciones conectadas** y, a continuación, seleccione la organización conectada para abrirla.

1. En el panel de información general de la organización conectada, seleccione **Editar** para cambiar el nombre o la descripción de la organización.  

1. En el panel **Directorio y dominio**, seleccione **Actualizar directorio y dominio** para cambiar a otro directorio o dominio.

1. En el panel **Patrocinadores**, seleccione **Agregar patrocinadores internos** o **Agregar patrocinadores externos** para agregar un usuario como patrocinador. Para quitar un patrocinador, selecciónelo y, en el panel derecho, seleccione **Eliminar**.


## <a name="delete-a-connected-organization"></a>Eliminar una organización conectada

Si ya no tiene una relación con un dominio o directorio externo de Azure AD, puede eliminar la organización conectada.

**Rol necesario**: *Administrador global*, *Administrador de usuarios* o *Invitador de usuarios invitados*

1. En Azure Portal, seleccione **Azure Active Directory** y, a continuación, **Gobernanza de identidades**.

1. En el panel de la izquierda, seleccione **Organizaciones conectadas** y, a continuación, seleccione la organización conectada para abrirla.

1. En el panel de información general de la organización conectada, seleccione **Eliminar** para eliminarla.

    Actualmente, solo se puede eliminar una organización conectada si no hay usuarios conectados.

    ![Botón Eliminar de la organización conectada](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso para usuarios externos](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users)
- [Administración del acceso para los usuarios que no están en el directorio](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
