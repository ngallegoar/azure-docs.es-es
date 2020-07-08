---
title: Problemas en la configuración del inicio de sesión único federado para una aplicación ajena a la galería | Microsoft Docs
description: Resolver algunos de los problemas comunes que pueden surgir al configurar un inicio de sesión único federado con una aplicación SAML personalizada que se muestre en la galería de aplicaciones de Azure AD
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
ms.date: 07/11/2017
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84a8ee8ca29046d26d200aaf853a3efe25f15768
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763574"
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Problemas en la configuración del inicio de sesión único federado para una aplicación ajena a la galería

Si se produce un problema al configurar una aplicación. Compruebe que ha seguido todos los pasos del artículo [Configuración del inicio de sesión único en aplicaciones que no están en la galería de aplicaciones de Azure Active Directory.](configure-federated-single-sign-on-non-gallery-applications.md)

## <a name="cant-add-another-instance-of-the-application"></a>No se puede agregar otra instancia de la aplicación

Para agregar una segunda instancia de una aplicación, tiene que poder:

-   Configurar un identificador único para la segunda instancia. No podrá configurar el mismo identificador que usó para la primera instancia.

-   Configurar un certificado diferente al utilizado para la primera instancia.

Si la aplicación no admite nada de lo indicado anteriormente, no podrá configurar una segunda instancia.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>¿Dónde se puede establecer el formato de EntityID (Identificador de usuario)?

No podrá seleccionar el formato de EntityID (identificador de usuario) que Azure AD envía a la aplicación en la respuesta a la autenticación de usuario.

Azure AD selecciona el formato del atributo NameID (identificador de usuario) en función del valor seleccionado o del formato que solicite la aplicación en el elemento AuthRequest de SAML. Para más información, visite la sección NameIDPolicy del artículo [Protocolo SAML de inicio de sesión único](../develop/single-sign-on-saml-protocol.md#authnrequest).

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>¿Dónde se obtienen los metadatos de la aplicación o el certificado de Azure AD?

Para descargar el certificado o los metadatos de la aplicación de Azure AD, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global** o **coadministrador.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Haga clic en **Aplicaciones empresariales** en el menú de navegación izquierdo de Azure Active Directory.

5. Haga clic en **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

   * Si no ve la aplicación que desea que aparezca aquí, use el control **Filtro** de la parte superior de la lista **Todas las aplicaciones** y establezca la opción **Mostrar** en **Todas las aplicaciones.**

6. Seleccione la aplicación para la que ha configurado el inicio de sesión único.

7. Cuando se cargue la aplicación, haga clic en **Inicio de sesión único** desde el menú de navegación izquierdo de la aplicación.

8. Vaya a la sección **Certificado de firma de SAML** y haga clic en el valor de la columna **Descargar**. Según lo que necesite la aplicación para configurar el inicio de sesión único, verá la opción para descargar el archivo XML de metadatos o el certificado.

Azure AD no proporciona una dirección URL para obtener los metadatos. Solo se pueden recuperar como un archivo XML.

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a>No sabe cómo personalizar las notificaciones SAML que se han enviado a una aplicación

Para obtener información sobre cómo personalizar las notificaciones de atributo SAML que se han enviado a su aplicación, vea [Asignación de notificaciones en Azure Active Directory](../develop/active-directory-claims-mapping.md).

## <a name="next-steps"></a>Pasos siguientes
[Administración de aplicaciones con Azure Active Directory](what-is-application-management.md)
