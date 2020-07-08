---
title: 'Experiencias de usuario final para aplicaciones: Azure Active Directory'
description: Azure Active Directory (Azure AD) proporciona varias maneras personalizables para implementar aplicaciones para los usuarios finales de su organización.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: kenwith
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c2ec85af2c954c2189f0df1b407a2c40967f096
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763404"
---
# <a name="end-user-experiences-for-applications-in-azure-active-directory"></a>Experiencias de usuario final para aplicaciones en Azure Active Directory

Azure Active Directory (Azure AD) proporciona varias maneras personalizables para implementar aplicaciones para los usuarios finales de su organización:

* Panel de acceso de Azure AD
* Iniciador de aplicaciones de Office 365
* Inicio de sesión directo en aplicaciones federadas
* Vínculos profundos a aplicaciones federadas, con contraseña o existentes

Los métodos que elija implementar en su organización son criterio suyo.

## <a name="azure-ad-access-panel"></a>Panel de acceso de Azure AD

El Panel de acceso de https://myapps.microsoft.com es un portal basado en web que permite que los usuarios finales que tengan cuenta organizativa en Azure Active Directory puedan ver e iniciar aplicaciones a las que el administrador de Azure AD les haya concedido acceso. Si usted es un usuario final con [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), también puede usar las capacidades de autoservicio de administración de grupos a través del Panel de acceso.

![Captura de pantalla que muestra el portal Panel de acceso de Azure AD](media/what-is-single-sign-on/azure-ad-access-panel.png)

De forma predeterminada, todas las aplicaciones se muestran juntas en una sola página. Sin embargo, puede usar colecciones para agrupar aplicaciones relacionadas y presentarlas en una pestaña independiente, lo que facilita su búsqueda. Por ejemplo, puede usar colecciones para crear agrupaciones lógicas de aplicaciones para roles de trabajo específicos, tareas, proyectos, etc. Para obtener información, vea [Cómo usar colecciones de Aplicaciones para personalizar paneles de acceso de usuario](access-panel-collections.md). 

El Panel de acceso es independiente de Azure Portal y no requiere que los usuarios tengan una suscripción de Azure u Office 365.

Para obtener más información sobre el Panel de acceso de Azure AD, consulte la [Introducción al panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="office-365-application-launcher"></a>Iniciador de aplicaciones de Office 365

Para las organizaciones que han implementado Office 365, las aplicaciones asignadas a los usuarios mediante Azure AD también aparecerán en el portal de Office 365, en [https://portal.office.com/myapps](https://portal.office.com/myapps). Esto hace que resulte fácil y cómodo para los usuarios de una organización iniciar sus aplicaciones sin tener que utilizar un segundo portal y es la solución de inicio de aplicaciones recomendada para las organizaciones que usan Office 365.

![Captura de pantalla que muestra el portal de Office 365](./media/end-user-experiences/microsoft-365-portal-office-com.png)

Para obtener más información sobre el iniciador de aplicaciones de Office 365, consulte [Hacer que su aplicación aparezca en el iniciador de aplicaciones de Office 365](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

## <a name="direct-sign-on-to-federated-apps"></a>Inicio de sesión directo en aplicaciones federadas

La mayoría de las aplicaciones federadas que admiten SAML 2.0, WS-Federation u OpenID Connect también tienen la capacidad de que los usuarios inicien la sesión en la aplicación y, después, lo hagan a través Azure AD, ya sea mediante el redireccionamiento automático o haciendo clic en un vínculo para iniciar la sesión. Esto se conoce como inicio de sesión por el proveedor de servicios y la mayoría de las aplicaciones federadas incluidas en la Galería de aplicaciones de Azure AD lo admiten (consulte la documentación vinculada desde el Asistente para la configuración de inicio de sesión único de la aplicación en Azure Portal para obtener más detalles).

![Ejemplo de una página de inicio de sesión de aplicación móvil](./media/end-user-experiences/workdaymobile.png)

## <a name="direct-sign-on-links"></a>Vínculos directos de inicio de sesión

Azure AD también admite vínculos directos de inicio de sesión único a las aplicaciones individuales que admiten el inicio de sesión con contraseña, el inicio de sesión único vinculado y cualquier forma de inicio de sesión único federado.

Estos vínculos son direcciones URL especialmente diseñadas que envían a los usuarios al proceso de inicio de sesión de Azure AD para una aplicación específica sin necesidad de que el usuario la inicie desde el panel de acceso de Azure AD o de Office 365. Estas **direcciones URL de acceso de usuario** pueden encontrarse en las propiedades de las aplicaciones empresariales disponibles. En Azure Portal, seleccione **Azure Active Directory** > **Aplicaciones empresariales**. Seleccione la aplicación y, después, **Propiedades**.

![Ejemplo de la dirección URL de acceso de usuario en las propiedades de Twitter](media/end-user-experiences/direct-sign-on-link.png)

Estos vínculos se pueden copiar y pegar en cualquier sitio donde que desee proporcionar un vínculo de inicio de sesión a la aplicación seleccionada. Podría ser en un mensaje de correo electrónico o en cualquier portal personalizado basado en web que haya configurado para el acceso de los usuarios a la aplicación. Este es un ejemplo de una AD de Azure único inicio de sesión URL directa de Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Similar a direcciones URL específicas de la organización para el panel de acceso, se puede personalizar aún más esta dirección URL mediante la adición de uno de los dominios de activos o comprobados para su directorio tras el dominio myapps.microsoft.com. Esto garantiza que cualquier personalización de marca corporativa se carga inmediatamente en la página de inicio de sesión sin que el usuario tenga que escribir primero su ID de usuario:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Cuando un usuario autorizado hace clic en uno de estos vínculos específicos de aplicaciones, primero ve su página de inicio de sesión organizativa (suponiendo que no ha iniciado todavía la sesión) y después de iniciarla se lo redirigen a su aplicación sin pararse primero en el panel de acceso. Si el usuario no cumple los requisitos previos para tener acceso a la aplicación, como la extensión de explorador para el inicio de sesión único con contraseña, el vínculo le pedirá al usuario que instale la extensión que le falta. La dirección URL del vínculo también permanece constante si cambia la configuración de inicio de sesión única para la aplicación.

Estos vínculos utilizan los mismos mecanismos de control de acceso que el panel de acceso y Office 365, y solo los usuarios o grupos que se han asignado a la aplicación en Azure Portal podrán autenticarse correctamente. Sin embargo, cualquier usuario que no tenga autorización, verá un mensaje que explica que no se le ha concedido acceso y se proporcionan un vínculo para cargar el panel de acceso y ver las aplicaciones disponibles a las que tienen acceso.

## <a name="next-steps"></a>Pasos siguientes

Por planes de implementación, consulte [Planes de implementación de Azure Active Directory](../fundamentals/active-directory-deployment-plans.md)
