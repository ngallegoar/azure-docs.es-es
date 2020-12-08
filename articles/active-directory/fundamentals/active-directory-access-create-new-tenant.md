---
title: 'Inicio rápido: Acceso y creación de un inquilino (Azure AD)'
description: Instrucciones para buscar Azure Active Directory y crear un inquilino para su organización.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18, fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: eeea88d8c21ba754fbeadbb24891126b639616c7
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437247"
---
# <a name="quickstart-create-a-new-tenant-in-azure-active-directory"></a>Inicio rápido: Creación de un inquilino en Azure Active Directory
Puede hacer todas las tareas administrativas mediante el portal de Azure Active Directory (Azure AD), incluida la creación de un inquilino para su organización. 

En este tutorial, obtendrá información sobre cómo acceder a Azure Portal y a Azure Active Directory y sobre cómo crear un inquilino básico para su organización.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-a-new-tenant-for-your-organization"></a>Creación de un inquilino para la organización
Después de iniciar sesión en Azure Portal, puede crear un inquilino para su organización. El nuevo inquilino representa a su organización y le ayuda a administrar una instancia específica de Servicios en la nube de Microsoft para los usuarios internos y externos.

### <a name="to-create-a-new-tenant"></a>Para crear un inquilino

1. Inicie sesión en la instancia de [Azure Portal](https://portal.azure.com/) de la organización.

1. En el menú de Azure Portal, seleccione **Azure Active Directory**.  

    <kbd>![Azure Active Directory: página de información general sobre la creación de un inquilino](media/active-directory-access-create-new-tenant/azure-ad-portal.png)</kbd>  

1. Seleccione **Create a tenant** (Crear un inquilino).

1. En la pestaña Conceptos básicos, seleccione el tipo de inquilino que desea crear, **Azure Active Directory** o **Azure Active Directory (B2C)** .

1. Seleccione **Siguiente: Configuration** (Siguiente: Configuración) para pasar a la pestaña Configuración.

    <kbd>![Azure Active Directory: pestaña de configuración de la página de creación de un inquilino](media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)</kbd>

1.  En la pestaña Configuración, escriba la siguiente información:
    
    - _Contoso Organization_ en el cuadro **Nombre de organización**.

    - _Contosoorg_ en el cuadro **Nombre de dominio inicial**.

    - Deje la opción _Estados Unidos_ en el cuadro **País o región**.

1. Seleccione **Siguiente: Revisar y crear**. Revise la información que ha escrito y, si es correcta, seleccione **Crear**.

    <kbd>![Azure Active Directory: página de revisión y creación del inquilino](media/active-directory-access-create-new-tenant/azure-ad-review.png)</kbd>

El nuevo inquilino se crea con el dominio contoso.onmicrosoft.com.

## <a name="clean-up-resources"></a>Limpieza de recursos
Si no va a seguir usando esta aplicación, puede eliminar el inquilino mediante los siguientes pasos:

- Asegúrese de que ha iniciado sesión en el directorio que desea eliminar mediante el filtro **Directorio y suscripción** de Azure Portal y cambie al directorio de destino, si es necesario.
- Seleccione **Azure Active Directory** y, después, en la página **Contoso - Información general**, seleccione **Eliminar directorio**.

    El inquilino y su información asociada se eliminarán.

    <kbd>![Página de información general con el botón para eliminar directorio resaltado](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)</kbd>

## <a name="next-steps"></a>Pasos siguientes
- Para cambiar o agregar nombres de dominio adicionales, vea [Incorporación de su nombre de dominio personalizado a Azure Active Directory](add-custom-domain.md).

- Para agregar usuarios, vea [Incorporación o eliminación de un nuevo usuario](add-users-azure-active-directory.md).

- Para agrear grupos y miembros, vea [Creación de un grupo básico y adición de miembros](active-directory-groups-create-azure-portal.md).

- Obtenga información sobre el [acceso basado en rol mediante Privileged Identity Management](../../role-based-access-control/best-practices.md) y el [acceso condicional](../../role-based-access-control/conditional-access-azure-management.md) para ayudar a administrar el acceso a aplicaciones y recursos de su organización.

- Obtenga información sobre Azure AD, incluida la [información de licencia básica, terminología y características asociadas](active-directory-whatis.md).
