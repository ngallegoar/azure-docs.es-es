---
title: Administración de la autenticación | Microsoft Azure Maps
description: Utilice Azure Portal para administrar la autenticación en Microsoft Azure Maps.
author: philmea
ms.author: philmea
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: dfe73971f29ea362fdd0ddd654e705b622ab1866
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335536"
---
# <a name="manage-authentication-in-azure-maps"></a>Administración de la autenticación en Azure Maps

Después de crear una cuenta de Azure Maps, se crean las claves y el identificador de cliente para admitir la autenticación de clave compartida y la autenticación de Azure Active Directory (Azure AD).

## <a name="view-authentication-details"></a>Visualización de los detalles de la autenticación

Después de crear la cuenta de Azure Maps, se generan las claves principal y secundaria. Es recomendable que use la clave principal como clave de suscripción cuando [llame a Azure Maps mediante la autenticación de clave compartida](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication). Puede usar la clave secundaria, por ejemplo, para realizar cambios de clave graduales. Para más información, consulte [Autenticación con Azure Maps](https://aka.ms/amauth).

Puede ver los datos de autenticación en Azure Portal. Una vez allí, vaya a su cuenta y seleccione **Autenticación** en el menú **Configuración**.

![Detalles de la autenticación](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="register-and-configure-an-azure-ad-app"></a>Registro y configuración de una aplicación de Azure AD

1. En Azure Portal, en la lista de servicios de Azure, seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.  

    ![Registro de aplicación](./media/how-to-manage-authentication/app-registration.png)

1. Si ya ha registrado la aplicación, continúe en el siguiente paso. Si no la ha registrado, especifique un **nombre**, seleccione el **tipo de cuenta admitido** y haga clic en **Registrar**.  

    ![Detalles de registro de la aplicación](./media/how-to-manage-authentication/app-create.png)

1. Para asignar permisos de API delegados a Azure Maps, vaya a la aplicación. A continuación, en **Registros de aplicaciones**, seleccione **Permisos de API** > **Add a permission** (Agregar un permiso). En **API usadas en mi organización**, busque y seleccione **Azure Maps**.

    ![Agregar permisos de API a la aplicación](./media/how-to-manage-authentication/app-permissions.png)

1. Active la casilla situada junto a **Access Azure Maps** (Acceder a Azure Maps) y seleccione **Agregar permisos**.

    ![Selección de permisos de API de aplicación](./media/how-to-manage-authentication/select-app-permissions.png)

1. Siga uno de los pasos que se indican a continuación, en función del método de autenticación utilizado. 

    * Si la aplicación utiliza la autenticación basada en tokens de usuario con el SDK web de Azure Maps, habilite `oauth2AllowImplicitFlow`. Para ello, en la sección **Manifiesto** del registro de la aplicación, establezca `oauth2AllowImplicitFlow` en True. 
    
       ![Manifiesto de aplicación](./media/how-to-manage-authentication/app-manifest.png)

    * Si la aplicación utiliza la autenticación del servidor o de la aplicación, en la página de registro de la aplicación, vaya a **Certificates & secrets** (Certificados y secretos). Cargue un certificado de clave pública o cree una contraseña seleccionando **Nuevo secreto de cliente**. 
    
       ![Creación de un secreto de cliente](./media/how-to-manage-authentication/app-keys.png)

        Si crea una contraseña, después de seleccionar **Agregar**, copie la contraseña y guárdela en un lugar seguro. Esta contraseña se utilizará después para obtener los tokens de Azure AD.

       ![Agregar un secreto de cliente](./media/how-to-manage-authentication/add-key.png)


## <a name="grant-role-based-access-control-to-azure-maps"></a>Concesión a Azure Maps del control de acceso basado en roles

Después de asociar una cuenta de Azure Maps a su inquilino de Azure AD, puede conceder el control de acceso. El *control de acceso basado en roles* (RBAC) se concede asignando un usuario, grupo o aplicación a uno o varios roles de control de acceso de Azure Maps. 

1. Vaya a su **cuenta de Azure Maps**. Seleccione **Control de acceso (IAM)**  > **Asignación de roles**.

    ![Concesión de RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

1. En la pestaña **Asignación de roles**, en **Rol**, seleccione **Azure Maps Data Reader (versión preliminar)** . En **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**. Seleccione el usuario o la aplicación. Después, seleccione **Guardar**.

    ![Agregar asignación de roles](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Visualización de los roles de RBAC de Azure Maps disponibles

Para ver los roles RBAC que están disponibles para Azure Maps, vaya a **Control de acceso (IAM)** . Seleccione **Roles** y busque los roles que comienzan con *Azure Maps*. Esos son los roles de Azure Maps a los que puede conceder acceso.

![Visualización de roles disponibles](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Ver RBAC de Azure Maps

RBAC proporciona control de acceso pormenorizado.

Para ver los usuarios y las aplicaciones a los que se han concedido roles RBAC para Azure Maps, vaya a **Control de acceso (IAM)** . Una vez allí, seleccione **Asignaciones de rol** y filtre los resultados por **Azure Maps**.

![Visualización de los usuarios y aplicaciones a los que se les han concedido roles RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Solicitud de tokens de Azure Maps

Después de registrar la aplicación y asociarla a Azure Maps, puede solicitar los tokens de acceso.

Si la aplicación utiliza la autenticación basada en tokens de usuario con el SDK web de Azure Maps, debe configurar la página HTML con el identificador de cliente de Azure Maps y el identificador de aplicación de Azure AD.

Si la aplicación usa la autenticación del servidor o de la aplicación, solicite un token desde el punto de conexión del token de Azure AD `https://login.microsoftonline.com`. Utilice los siguientes datos en la solicitud: 

* Identificador de recurso de Azure AD `https://atlas.microsoft.com/`
* Identificador de cliente de Azure Maps
* Identificador de aplicación de Azure AD
* Contraseña o certificado de registro de la aplicación de Azure AD

| Entorno de Azure   | Punto de conexión del token de Azure AD | Identificador de recurso de Azure |
| --------------------|-------------------------|-------------------|
| Nube pública de Azure        | `https://login.microsoftonline.com` | `https://atlas.microsoft.com/` |
| Nube de Azure Government   | `https://login.microsoftonline.us`  | `https://atlas.microsoft.com/` | 

Para obtener más información sobre la solicitud de tokens de acceso de Azure AD para usuarios y entidades de servicio, consulte el artículo sobre los [escenarios de autenticación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte el artículo sobre el [SDK web de Azure AD y Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Descubra las métricas de uso de API de la cuenta de Azure Maps:
> [!div class="nextstepaction"] 
> [Visualización de métricas de uso](how-to-view-api-usage.md)

Explore ejemplos acerca de cómo integrar Azure AD con Azure Maps:

> [!div class="nextstepaction"]
> [Muestras de autenticación de Azure AD](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
