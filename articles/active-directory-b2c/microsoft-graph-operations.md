---
title: Operaciones de Microsoft Graph admitidas
titleSuffix: Azure AD B2C
description: Índice de las operaciones de Microsoft Graph compatibles con la administración de recursos de Azure AD B2C, incluidos usuarios, flujos de usuarios, proveedores de identidades, directivas personalizadas, claves de directivas, etc.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 27fe1a41365d96a4179f8c659b63dc22c7b9fc93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184255"
---
# <a name="microsoft-graph-operations-available-for-azure-ad-b2c"></a>Operaciones de Microsoft Graph disponibles para Azure AD B2C

Se admiten las siguientes operaciones de Microsoft Graph API para la administración de recursos de Azure AD B2C, incluidos usuarios, proveedores de identidades, flujos de usuarios, directivas personalizadas y claves de directivas.

Cada vínculo de las secciones siguientes se dirige a la página correspondiente dentro de la referencia de Microsoft Graph API para esa operación.

## <a name="user-management"></a>Administración de usuarios

- [Enumerar usuarios](https://docs.microsoft.com/graph/api/user-list)
- [Crear usuario](https://docs.microsoft.com/graph/api/user-post-users)
- [Obtener un usuario](https://docs.microsoft.com/graph/api/user-get)
- [Actualizar usuario](https://docs.microsoft.com/graph/api/user-update)
- [Eliminar un usuario](https://docs.microsoft.com/graph/api/user-delete)

Para más información sobre la administración de cuentas de usuario de Azure AD B2C, consulte [Azure AD B2C: Usar Graph API de Azure AD](manage-user-accounts-graph-api.md).

## <a name="identity-providers-user-flow"></a>Proveedores de identidades (flujo de usuarios)

Administre los proveedores de identidades disponibles para los flujos de usuarios en el inquilino de Azure AD B2C.

- [Enumerar proveedores de identidades en el inquilino de Azure AD B2C](https://docs.microsoft.com/graph/api/identityprovider-list)
- [Crear proveedores de identidades](https://docs.microsoft.com/graph/api/identityprovider-post-identityproviders)
- [Obtener proveedores de identidades](https://docs.microsoft.com/graph/api/identityprovider-get)
- [Actualizar proveedores de identidades](https://docs.microsoft.com/graph/api/identityprovider-update)
- [Eliminar proveedores de identidades](https://docs.microsoft.com/graph/api/identityprovider-delete)

## <a name="user-flow"></a>Flujo de usuario

Configure las directivas predefinidas para el registro, el inicio de sesión, el registro y el inicio de sesión combinados, el restablecimiento de contraseña y la actualización del perfil.

- [Enumerar flujos de usuario](https://docs.microsoft.com/graph/api/identityuserflow-list)
- [Crear flujos de usuario](https://docs.microsoft.com/graph/api/identityuserflow-post-userflows)
- [Obtener flujos de usuario](https://docs.microsoft.com/graph/api/identityuserflow-get)
- [Eliminar flujos de usuario](https://docs.microsoft.com/graph/api/identityuserflow-delete)

## <a name="custom-policies"></a>Directivas personalizadas

Las siguientes operaciones permiten administrar las directivas del marco de confianza de Azure AD B2C, conocidas como [directivas personalizadas](custom-policy-overview.md).

- [Enumerar directivas de marco de confianza](https://docs.microsoft.com/graph/api/trustframework-list-trustframeworkpolicies)
- [Crear directivas de marco de confianza](https://docs.microsoft.com/graph/api/trustframework-post-trustframeworkpolicy)
- [Obtener directivas de marco de confianza](https://docs.microsoft.com/graph/api/trustframeworkpolicy-get)
- [Actualizar o crear directivas de marco de confianza](https://docs.microsoft.com/graph/api/trustframework-put-trustframeworkpolicy)
- [Eliminar directivas de marco de confianza](https://docs.microsoft.com/graph/api/trustframeworkpolicy-delete)

## <a name="policy-keys"></a>Claves de directiva

Identity Experience Framework almacena los secretos a los que se hace referencia en una directiva personalizada para establecer la confianza entre los componentes. Estos secretos pueden ser claves/valores simétricos o asimétricos. En Azure Portal, estas entidades se muestran como **claves de directiva**.

El recurso de nivel superior para las claves de directiva de Microsoft Graph API es el [conjunto de claves del marco de confianza](https://docs.microsoft.com/graph/api/resources/trustframeworkkeyset). Cada **conjunto de claves** contiene al menos una **clave**. Para crear una clave, primero debe crear un conjunto de claves vacío y, a continuación, generar una clave en el conjunto de claves. Puede crear un secreto manual, cargar un certificado o una clave PKCS12. La clave puede ser un secreto generado, una cadena que defina (como el secreto de aplicación de Facebook) o un certificado que cargue. Si un conjunto de claves tiene varias claves, solo una de las claves está activa.

### <a name="trust-framework-policy-keyset"></a>Conjunto de claves de directiva de marco de confianza

- [Enumerar conjuntos de claves de confianza](https://docs.microsoft.com/graph/api/trustframework-list-keysets)
- [Crear conjuntos de claves de confianza](https://docs.microsoft.com/graph/api/trustframework-post-keysets)
- [Obtener conjuntos de claves de confianza](https://docs.microsoft.com/graph/api/trustframeworkkeyset-get)
- [Actualizar conjuntos de claves de confianza](https://docs.microsoft.com/graph/api/trustframeworkkeyset-update)
- [Eliminar conjuntos de claves de confianza](https://docs.microsoft.com/graph/api/trustframeworkkeyset-delete)

### <a name="trust-framework-policy-key"></a>Clave de directiva de marco de confianza

- [Obtener clave activa](https://docs.microsoft.com/graph/api/trustframeworkkeyset-getactivekey)
- [Generar clave](https://docs.microsoft.com/graph/api/trustframeworkkeyset-generatekey)
- [Cargar secreto](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadsecret)
- [Cargar certificado X.509](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadcertificate)
- [Cargar certificado PKCS12](https://docs.microsoft.com/graph/api/trustframeworkkeyset-uploadpkcs12)

## <a name="applications"></a>APLICACIONES

- [Lista de aplicaciones](https://docs.microsoft.com/graph/api/application-list)
- [Crear aplicación](https://docs.microsoft.com/graph/api/resources/application)
- [Actualizar aplicación](https://docs.microsoft.com/graph/api/application-update)
- [Tipo de recurso servicePrincipal](https://docs.microsoft.com/graph/api/resources/serviceprincipal)
- [Tipo de recurso oAuth2PermissionGrant](https://docs.microsoft.com/graph/api/resources/oauth2permissiongrant)
- [Eliminar aplicación](https://docs.microsoft.com/graph/api/application-delete)

## <a name="application-extension-properties"></a>Propiedades de extensión de aplicación

- [Enumerar propiedades de extensión](https://docs.microsoft.com/graph/api/application-list-extensionproperty)

Azure AD B2C proporciona un directorio que puede contener 100 atributos personalizados por usuario. En el caso de los flujos de usuarios, estas propiedades de extensión se [administran mediante Azure Portal](custom-policy-custom-attributes.md). En el caso de las directivas personalizadas, Azure AD B2C crea la propiedad automáticamente la primera vez que la directiva escribe un valor en la propiedad de extensión.

## <a name="audit-logs"></a>Registros de auditoría

- [Enumerar auditorías de auditorio](https://docs.microsoft.com/graph/api/directoryaudit-list)

Para más información sobre cómo obtener acceso a los registros de auditoría de Azure AD B2C con Microsoft Graph API, consulte [Acceso a los registros de auditoría de Azure AD B2C](view-audit-logs.md).
