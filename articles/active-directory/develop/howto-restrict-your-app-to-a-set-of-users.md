---
title: Restricción de la aplicación de Azure AD a un conjunto de usuarios | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a restringir el acceso a las aplicaciones registradas en Azure AD a un conjunto de usuarios seleccionado.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: cccd2df334828c0b8103e4da2ffcd8549673b69c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76697003"
---
# <a name="how-to-restrict-your-azure-ad-app-to-a-set-of-users"></a>Procedimientos: Restricción de la aplicación de Azure AD a un conjunto de usuarios

Las aplicaciones registradas en un inquilino de Azure Active Directory (Azure AD) están disponibles de forma predeterminada para todos los usuarios del inquilino que se autentica correctamente.

Igualmente, en el caso de una aplicación [multiinquilino](howto-convert-app-to-be-multi-tenant.md), todos los usuarios del inquilino de Azure AD donde se aprovisiona esta aplicación podrán acceder a ella una vez que se autentiquen correctamente en sus respectivos inquilinos.

Los desarrolladores y administradores de inquilinos tienen con frecuencia el requisito de restringir una aplicación a un determinado conjunto de usuarios. Los desarrolladores pueden conseguir lo mismo mediante patrones de autorización conocidos, como el control de acceso basado en rol (RBAC), pero este enfoque exige una importante cantidad de trabajo por parte del desarrollador.

Azure AD permite a los administradores y desarrolladores de inquilinos restringir una aplicación a un conjunto específico de usuarios o a los grupos de seguridad del inquilino.

## <a name="supported-app-configurations"></a>Configuraciones de aplicación admitidas

La opción para restringir una aplicación a un conjunto específico de usuarios o a los grupos de seguridad de un inquilino funciona con los siguientes tipos de aplicaciones:

- Aplicaciones configuradas para el inicio de sesión federado con autenticación basada en SAML
- Aplicaciones de proxy de aplicación que usan la autenticación previa de Azure AD
- Aplicaciones integradas directamente en la plataforma de aplicaciones de Azure AD que usan la autenticación de OAuth 2.0 u OpenID Connect después de que un usuario o un administrador han dado su consentimiento a esa aplicación.

     > [!NOTE]
     > Esta característica está disponible únicamente para aplicaciones web o API web y aplicaciones empresariales. Las aplicaciones que se registran como [nativas](quickstart-v1-integrate-apps-with-azure-ad.md) no se pueden restringir a un conjunto de usuarios o a los grupos de seguridad del inquilino.

## <a name="update-the-app-to-enable-user-assignment"></a>Actualización de la aplicación para permitir la asignación de usuarios

Hay dos maneras de crear una aplicación con la asignación de usuarios habilitada. Una necesita el rol **Administrador global**, la otra no.

### <a name="enterprise-applications-requires-the-global-administrator-role"></a>Aplicaciones empresariales (necesita el rol Administrador global)

1. Vaya a [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **Administrador global**.
1. En la barra superior, seleccione la cuenta con sesión iniciada. 
1. En **Directory**, seleccione el inquilino de Azure AD donde se registrará la aplicación.
1. En el panel de navegación izquierdo, seleccione **Azure Active Directory**. Si Azure Active Directory no está disponible en el panel de navegación, siga estos pasos:

    1. Seleccione **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.
    1. Escriba **Azure Active Directory** en el cuadro de búsqueda de filtro y seleccione el elemento **Azure Active Directory** en el resultado.

1. En el panel **Azure Active Directory**, seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo **Azure Active Directory**.
1. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

     Si no ve la aplicación que desea, utilice los distintos filtros de la parte superior de la lista **Registros de aplicaciones** para restringir la lista o desplácese hacia abajo en la lista para buscar la aplicación.

1. Seleccione la aplicación que desea asignar a un usuario o a un grupo de seguridad.
1. En la página **Información general** de la aplicación, seleccione **Propiedades** en el menú de navegación izquierdo de la aplicación.
1. Busque el valor **¿Asignación de usuarios?** y establézcalo en **Sí**. Cuando esta opción se establece en **Sí**, los usuarios primero se deben asignar a esta aplicación antes de poder acceder a ella.
1. Para guardar este cambio de configuración, seleccione **Guardar**.

### <a name="app-registration"></a>Registro de aplicación

1. Vaya a [**Azure Portal**](https://portal.azure.com/).
1. En la barra superior, seleccione la cuenta con sesión iniciada. 
1. En **Directory**, seleccione el inquilino de Azure AD donde se registrará la aplicación.
1. En el panel de navegación izquierdo, seleccione **Azure Active Directory**.
1. En el panel **Azure Active Directory**, seleccione **Registros de aplicaciones** en el menú de navegación de la izquierda de **Azure Active Directory**.
1. Cree o seleccione la aplicación que quiere administrar. Debe ser **Propietario** de este registro de aplicación.
1. En la página **Información general** de la aplicación, siga el vínculo **Aplicación administrada en el directorio local** en los elementos fundamentales que aparecen en la parte superior de la página. Esto lo llevará a la _aplicación empresarial administrada_ de su registro de aplicación.
1. En la hoja de navegación de la izquierda, seleccione **Propiedades**.
1. Busque el valor **¿Asignación de usuarios?** y establézcalo en **Sí**. Cuando esta opción se establece en **Sí**, los usuarios primero se deben asignar a esta aplicación antes de poder acceder a ella.
1. Para guardar este cambio de configuración, seleccione **Guardar**.

## <a name="assign-users-and-groups-to-the-app"></a>Asignación de usuarios y grupos a la aplicación

Una vez que ha configurado la aplicación para permitir la asignación de usuarios, puede seguir adelante y asignar usuarios y grupos a la aplicación.

1. Seleccione el panel **Usuarios y grupos** en el menú de navegación izquierdo de la aplicación.
1. En la parte superior de la lista **Usuarios y grupos**, seleccione el botón **Agregar usuario** para abrir el panel **Agregar asignación**.
1. Elija el selector **Usuarios** en el panel **Agregar asignación**. 

     Se mostrará una lista de usuarios y grupos de seguridad junto con un cuadro de texto para buscar un usuario o grupo determinado. Esta pantalla permite seleccionar varios usuarios y grupos en una sola operación.

1. Cuando haya terminado la selección de los usuarios y grupos, presione el botón **Seleccionar** en la parte inferior para desplazarse al apartado siguiente.
1. Presione el botón **Asignar** en la parte inferior para finalizar las asignaciones de usuarios y grupos a la aplicación. 
1. Confirme que los usuarios y grupos que agregó se muestran en la lista **Usuarios y grupos** actualizada.

