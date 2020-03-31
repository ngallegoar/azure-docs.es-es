---
title: Creación de una unidad organizativa (OU) en Azure AD Domain Services | Microsoft Docs
description: Aprenda a crear y administrar una unidad organizativa personalizada en un dominio administrado de Azure AD Domain Services.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: iainfou
ms.openlocfilehash: 7abbdf03e85f425f65a45e6640b82529c2b9c84f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614061"
---
# <a name="create-an-organizational-unit-ou-in-an-azure-ad-domain-services-managed-domain"></a>Creación de una unidad organizativa en un dominio administrado de Azure AD Domain Services

Las unidades organizativas de Active Directory Domain Services (AD DS) le permiten agrupar lógicamente objetos como cuentas de usuario, cuentas de servicio o cuentas de equipo. Después, puede asignar administradores a unidades organizativas específicas y aplicar la directiva de grupo para aplicar los valores de configuración de destino.

Los dominios administrados de Azure AD DS incluyen dos unidades organizativas integradas *AADDC Computers* y *AADDC Users*. La unidad organizativa *AADDC Computers* contiene objetos de equipo para todos los equipos que están unidos al dominio administrado. La unidad organizativa *AADDC Users* incluye los usuarios y grupos sincronizados desde el inquilino de Azure AD. A medida que crea y ejecuta cargas de trabajo que usan Azure AD DS, puede que necesite crear cuentas de servicio para que las aplicaciones se autentiquen a sí mismas. Para organizar estas cuentas de servicio, normalmente crea una unidad organizativa personalizada en el dominio administrado de Azure AD DS y, a continuación, crea cuentas de servicio dentro de esa unidad organizativa.

En un entorno híbrido, las unidades organizativas creadas en un entorno de AD DS local no se sincronizan con Azure AD DS. Los dominios administrados de Azure AD DS tienen una estructura plana de unidad organizativa. Todas las cuentas de usuario y los grupos se almacenan en el contenedor *Usuarios de AADDC*, aunque se sincronicen desde dominios o bosques locales distintos, incluso si se ha configurado una estructura jerárquica de unidad organizativa ahí.

En este artículo se muestra cómo crear una unidad organizativa en el dominio administrado de Azure AD DS.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Antes de empezar

Para completar este artículo, necesitará los siguientes recursos y privilegios:

* Una suscripción de Azure activa.
    * Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un inquilino de Azure Active Directory asociado a su suscripción, ya sea sincronizado con un directorio en el entorno local o con un directorio solo en la nube.
    * Si es necesario, [cree un inquilino de Azure Active Directory][create-azure-ad-tenant] o [asocie una suscripción a Azure con su cuenta][associate-azure-ad-tenant].
* Un dominio administrado de Azure Active Directory Domain Services habilitado y configurado en su inquilino de Azure AD.
    * Si es necesario, complete el tutorial para [crear y configurar una instancia de Azure Active Directory Domain Services][create-azure-ad-ds-instance].
* Una máquina virtual de administración de Windows Server que esté unida al dominio administrado de Azure AD DS.
    * Si es necesario, complete el tutorial para [crear una máquina virtual de administración][tutorial-create-management-vm].
* Una cuenta de usuario que sea miembro del grupo de *administradores de Azure AD DC* en el inquilino de Azure AD.

## <a name="custom-ou-considerations-and-limitations"></a>Consideraciones y limitaciones de las unidades organizativas personalizadas

Al crear unidades organizativas personalizadas en un dominio administrado de Azure AD DS, obtiene flexibilidad de administración adicional para la administración de usuarios y la aplicación de la directiva de grupo. En comparación con un entorno de AD DS local, hay algunas limitaciones y consideraciones a la hora de crear y administrar una estructura de unidad organizativa personalizada en Azure AD DS:

* Para crear unidades organizativas personalizadas, los usuarios deben ser miembros del grupo *Administradores de controladores de dominio de AAD*.
* A un usuario que crea una unidad organizativa personalizada se le conceden privilegios de administración (control total) sobre esa unidad organizativa y es el propietario del recurso.
    * De forma predeterminada, el grupo *Administradores de controladores de dominio de AAD* también tiene control total sobre la unidad organizativa personalizada.
* Se crea una unidad organizativa predeterminada para los *usuarios de AADDC* que contiene todas las cuentas de usuario sincronizadas del inquilino de Azure AD.
    * No podrá mover usuarios o grupos de la unidad organizativa *AADDC Users* a las unidades organizativas personalizadas que cree. Solo las cuentas de usuario o los recursos creados en el dominio administrado de Azure AD DS se pueden mover a unidades organizativas personalizadas.
* Las cuentas de usuario, los grupos, las cuentas de servicio y los objetos de equipo que se creen en unidades organizativas personalizadas no estarán disponibles en el inquilino de Azure AD.
    * Estos objetos no se muestran mediante Microsoft Graph API ni aparecerán en la interfaz de usuario de Azure AD; solo están disponibles en el dominio administrado de Azure AD DS.

## <a name="create-a-custom-ou"></a>Creación de una unidad organizativa personalizada

Para crear una unidad organizativa personalizada, use las herramientas administrativas de Active Directory desde una máquina virtual unida a un dominio. El Centro de administración de Active Directory le permite ver, editar y crear recursos en un dominio administrado de Azure AD DS, incluidas las unidades organizativas.

> [!NOTE]
> Para crear una unidad organizativa personalizada en un dominio administrado de Azure AD DS, debe haber iniciado sesión en una cuenta de usuario que sea miembro del grupo *Administradores del controlador de dominio de AAD*.

1. Inicie sesión en la máquina virtual de administración. Si quiere conocer los pasos para conectarse mediante Azure Portal, consulte [Conexión a una máquina virtual de Windows Server][connect-windows-server-vm].
1. En la pantalla Inicio, seleccione **Herramientas administrativas**. Se muestra una lista de las herramientas de administración disponibles que se instalaron en el tutorial para [crear una máquina virtual de administración][tutorial-create-management-vm].
1. Para crear y administrar unidades organizativas, seleccione **Centro de administración de Active Directory** de la lista de herramientas administrativas.
1. En el panel izquierdo, elija el dominio administrado de Azure AD DS como, por ejemplo, *aaddscontoso.com*. Aparecerá una lista de las unidades organizativas y los recursos existentes:

    ![Selección del dominio administrado de Azure AD DS en el Centro de administración de Active Directory](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

1. El panel **Tareas** aparece en el lado derecho del Centro de administración de Active Directory. En el dominio, por ejemplo, *aaddscontoso.com*, seleccione **Nueva > Unidad organizativa**.

    ![Selección de la opción para crear una nueva unidad organizativa en el Centro de administración de Active Directory](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

1. En el cuadro de diálogo **Create Organizational Unit** (Crear unidad organizativa), especifique un **Nombre** para la nueva unidad organizativa como, por ejemplo *MyCustomOu*. Proporcione una descripción breve de la unidad organizativa como *Unidad organizativa personalizada para las cuentas de servicio*. Si lo desea, también puede establecer el campo **Administrado por** de la unidad organizativa. Seleccione **Aceptar**para crear la unidad organizativa personalizada.

    ![Creación de una unidad organizativa personalizada desde el Centro de administración de Active Directory](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

1. De nuevo en el Centro de administración de Active Directory, la unidad organizativa personalizada aparece ahora en la lista y está disponible para su uso:

    ![Unidad organizativa personalizada disponible para su uso en el Centro de administración de Active Directory](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de las herramientas administrativas o la creación y el uso de cuentas de servicio, consulte los siguientes artículos:

* [Centro de administración de Active Directory: Introducción](https://technet.microsoft.com/library/dd560651.aspx)
* [Guía paso a paso de las cuentas de servicio](https://technet.microsoft.com/library/dd548356.aspx)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm
