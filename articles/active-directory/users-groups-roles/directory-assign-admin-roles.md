---
title: 'Permisos y descripciones de roles de administrador: Azure AD | Microsoft Docs'
description: Un rol de administrador puede agregar usuarios, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario o administrar dominios.
services: active-directory
author: curtand
manager: daveba
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: reference
ms.date: 02/28/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro, fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63e561ff1b976b3de993414607b694e621b1d536
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346944"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Permisos de roles de administrador en Azure Active Directory

Con Azure Active Directory (Azure AD), puede designar administradores limitados que administren tareas de identidad en roles con menos privilegios. Los administradores se pueden asignar para realizar tareas como agregar usuarios o cambiarlos, asignar roles administrativos, restablecer contraseñas de usuario, administrar licencias de usuario y administrar nombres de dominio. Los permisos de usuario predeterminados solo se pueden cambiar en la configuración de usuario de Azure AD.

## <a name="limit-the-use-of-global-administrator"></a>Limitación del uso de Administrador global

Los usuarios que tienen asignado el rol Administrador global pueden leer y modificar cada configuración administrativa de la organización de Azure AD. De manera predeterminada, a la persona que se suscribe a Azure se le asigna el rol Administrador global para la organización de Azure AD. Solo los administradores globales y los que tengan un rol con privilegios pueden delegar roles de administrador. Para reducir el riesgo para su negocio, le recomendamos asignar este rol a la menor cantidad posible de personas de su organización.

Como procedimiento recomendado, aconsejamos que se asigne este rol a menos de cinco personas de su organización. Si tiene más de cinco usuarios asignados al rol Administrador global en la organización, estas son algunas maneras de reducir el uso.

### <a name="find-the-role-you-need"></a>Búsqueda del rol que necesita

Si buscar el rol que necesita en una lista de muchos roles le resulta frustrante, Azure AD puede mostrarle subconjuntos de roles según las categorías de rol. Revise nuestro nuevo filtro **Tipo** para [Roles y administradores de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) para mostrarle solo los roles del tipo seleccionado.

### <a name="a-role-exists-now-that-didnt-exist-when-you-assigned-the-global-administrator-role"></a>Ahora existe un rol que no existía cuando asignó el rol Administrador global.

Es posible que uno o varios roles se hayan agregado a Azure AD que proporcionan permisos más específicos que no eran opción cuando elevó algunos usuarios al rol Administrador global. Con el tiempo, se implementan roles adicionales que realizan tareas que antes solo podía realizar el rol Administrador global. Los puede ver reflejados en los [Roles disponibles](#available-roles) siguientes.

## <a name="assign-or-remove-administrator-roles"></a>Asignación o eliminación de roles de administrador

Para obtener información sobre cómo asignar roles administrativos a un usuario en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Roles disponibles

Los roles de administrador disponibles son los siguientes:

### <a name="application-administrator"></a>[Administrador de aplicaciones](#application-administrator-permissions)

los usuarios con este rol pueden crear y administrar todos los aspectos de las aplicaciones empresariales, los registros de aplicaciones y la configuración del proxy de aplicación. Tenga en cuenta que los usuarios asignados a este rol no se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

Los administradores de aplicaciones pueden administrar las credenciales de aplicación que les permite suplantar la aplicación. Por lo tanto, los usuarios asignados a este rol pueden administrar las credenciales de aplicación únicamente de las aplicaciones que no están asignadas a ningún rol de Azure AD ni a los asignados a los roles de administrador siguientes:
* Administrador de aplicaciones
* Desarrollador de aplicaciones
* Administrador de aplicaciones en la nube
* Lectores de directorios

Si se asigna una aplicación a cualquier otro rol que no se menciona anteriormente, el administrador de la aplicación no puede administrar las credenciales de esa aplicación. 
 
Este rol proporciona igualmente la capacidad de _dar el consentimiento_ para permisos delegados y permisos de aplicaciones, con la excepción de los permisos relacionados con Microsoft Graph API.

> [!IMPORTANT]
> Esta excepción significa que puede seguir dando su consentimiento a los permisos para _otras_ aplicaciones (por ejemplo, aplicaciones de terceros que haya registrado), pero no a los permisos propios de Azure AD. Todavía puede _solicitar_ estos permisos como parte del registro de la aplicación, pero _conceder_ estos permisos (es decir, dar su consentimiento) requiere un administrador de Azure AD. Esto significa que un usuario malintencionado no puede elevar fácilmente sus permisos. Por ejemplo, no podría crear y dar su consentimiento a una aplicación que pueda escribir en todo el directorio y a través de los permisos de esa aplicación elevar sus permisos para convertirse en administrador global.

### <a name="application-developer"></a>[Desarrollador de aplicaciones](#application-developer-permissions)

los usuarios con este rol pueden crear registros de aplicaciones cuando la opción "Los usuarios pueden registrar aplicaciones" está establecida en No. Esta función también concede permiso para dar consentimiento en nombre propio cuando la opción "Los usuarios pueden permitir que las aplicaciones accedan a los datos de la compañía en su nombre" está establecida en No. Los usuarios asignados a este rol se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

### <a name="authentication-administrator"></a>[Administrador de autenticación](#authentication-administrator-permissions)

Los usuarios de este rol pueden establecer o restablecer credenciales que no son de contraseña, actualizar contraseñas, requerir que los usuarios vuelvan a registrarse con una credencial existente distinta de contraseña (por ejemplo, MFA o FIDO) y revocar la opción **Recordar MFA en el dispositivo**, por lo que se solicitará la MFA en el siguiente inicio de sesión de todos los usuarios que no sean administradores o que estén asignados a los siguientes roles:
* Administrador de autenticación
* Lectores de directorios
* Invitador de usuarios
* Lector del Centro de mensajes
* Lector de informes

> [!IMPORTANT]
> Los usuarios con este rol pueden cambiar las credenciales de las personas que pueden tener acceso a información confidencial o privada o configuración crítica dentro y fuera de Azure Active Directory. Cambiar las credenciales de un usuario puede significar la capacidad de asumir la identidad y los permisos de ese usuario. Por ejemplo:
>
>- Propietarios de registro de la aplicación y la aplicación de empresa, que pueden administrar las credenciales de las aplicaciones que poseen. Esas aplicaciones pueden tener permisos con privilegios en Azure AD y en otra parte que no se hayan concedido a los administradores de autenticación. Mediante esta ruta de acceso, un administrador de autenticación puede ser capaz de asumir la identidad del propietario de la aplicación y después asumir la identidad de una aplicación con privilegios mediante la actualización de las credenciales de la aplicación.
>- Propietarios de suscripción de Azure, que pueden tener acceso a información confidencial o privada o configuración crítica en Azure.
>- Propietarios del grupo de seguridad y el grupo de Office 365, que pueden administrar la pertenencia a grupos. Dichos grupos pueden conceder acceso a información confidencial o privada o a configuración crítica en Azure AD y en cualquier otra parte.
>- Los administradores de otros servicios fuera de Azure AD, como Exchange Online, Office Security y Compliance Center y sistemas de recursos humanos.
>- Usuarios no administradores como empleados ejecutivos, de asesoramiento jurídico y de recursos humanos que pueden tener acceso a información confidencial o privada.

### <a name="azure-devops-administrator"></a>[Administrador de Azure DevOps](#azure-devops-administrator-permissions)

Los usuarios con este rol pueden administrar la directiva de Azure DevOps para restringir la creación de una nueva organización de Azure DevOps a un conjunto de usuarios o grupos que se puede configurar. Los usuarios de este rol pueden administrar esta directiva en cualquier organización Azure DevOps que esté respaldada por la instancia de Azure AD de la empresa.

Todas las directivas empresariales de Azure DevOps pueden ser administradas por los usuarios de este rol.

### <a name="azure-information-protection-administrator"></a>[Administrador de Azure Information Protection](#azure-information-protection-administrator-permissions)

los usuarios con este rol tienen todos los permisos en el servicio Azure Information Protection. Este rol permite configurar las etiquetas de la directiva de Azure Information Protection, administrar plantillas de protección y activar la protección. Este rol no concede ningún permiso de Identity Protection Center, Privileged Identity Management, Supervisión del estado de mantenimiento del servicio Office 365 y Centro de seguridad y cumplimiento de Office 365.

### <a name="b2c-user-flow-administrator"></a>[Administrador de flujos de usuario B2C](#b2c-user-flow-administrator-permissions)

Los usuarios con este rol pueden crear y administrar flujos de usuario B2C (también conocidos como directivas "integradas") en Azure Portal. Mediante la creación o edición de flujos de usuario, estos usuarios pueden cambiar el contenido de HTML/CSS/Javascript de la experiencia de usuario, los requisitos de MFA por flujo de usuario y las notificaciones en el token, así como ajustar la configuración de las sesiones conforme a todas las directivas del inquilino. Por otro lado, este rol no incluye la posibilidad de revisar los datos de usuario, ni de realizar cambios en los atributos que están incluidos en el esquema de inquilino. Los cambios en las directivas de Identity Experience Framework (también conocida como Custom) también quedan fuera del ámbito de este rol.

### <a name="b2c-user-flow-attribute-administrator"></a>[Administrador de atributos de flujos de usuario B2C](#b2c-user-flow-attribute-administrator-permissions)

los usuarios con este rol agregan o eliminan los atributos personalizados disponibles en todos los flujos de usuario del inquilino. De esta forma, los usuarios con este rol pueden cambiar o agregar nuevos elementos al esquema de usuario final y que ello afecte al comportamiento de todos los flujos de usuario e indirectamente dé como resultado cambios en qué datos se pueden solicitar a los usuarios finales y, finalmente, enviarse como notificaciones a las aplicaciones. Este rol no puede editar flujos de usuario.

### <a name="b2c-ief-keyset-administrator"></a>[Administrador de conjuntos de claves B2C con IEF](#b2c-ief-keyset-administrator-permissions)

El usuario puede crear y administrar claves de directiva y secretos de cifrado de tokens, firmas de tokens y cifrado y descifrado de notificaciones. Al agregar nuevas claves a los contenedores de claves existentes, este administrador limitado puede sustituir secretos según sea necesario sin que ello afecte a las aplicaciones existentes. Este usuario puede ver todo el contenido de estos secretos y sus fechas de expiración incluso después de su creación.

> [!IMPORTANT]
> Se trata de un rol confidencial. El rol Administrador del conjunto de claves se debe auditar y asignar con cuidado durante las fases de preproducción y producción.

### <a name="b2c-ief-policy-administrator"></a>[Administrador de directivas B2C con IEF](#b2c-ief-policy-administrator-permissions)

los usuarios de este rol tienen la posibilidad de crear, leer, actualizar y eliminar todas las directivas personalizadas de Azure AD B2C y, por tanto, tienen control total sobre Identity Experience Framework en el inquilino de Azure AD B2C pertinente. Mediante la edición de directivas, este usuario puede establecer la federación directa con proveedores de identidades externos, cambiar el esquema de directorios, cambiar todo el contenido al que pueden acceder los usuarios (HTML, CSS, JavaScript), cambiar los requisitos para realizar una autenticación, crear nuevos usuarios, enviar datos de usuario a sistemas externos incluyendo migraciones completas, y editar la información de todos los usuarios, incluidos los campos confidenciales como las contraseñas y los números de teléfono. Por el contrario, este rol no puede cambiar las claves de cifrado ni editar los secretos usados para la federación en el inquilino.

> [!IMPORTANT]
> El rol de Administrador de directivas B2C con IEF es un rol delicado que se debería asignar de manera muy limitada para los inquilinos de producción. Las actividades de estos usuarios se deberían auditar estrechamente, especialmente para los inquilinos de producción.

### <a name="billing-administrator"></a>[Administrador de facturación](#billing-administrator-permissions)

hace compras, administra suscripciones, administra incidencias de soporte técnico y supervisa el estado del servicio.

### <a name="cloud-application-administrator"></a>[Administrador de aplicaciones en la nube](#cloud-application-administrator-permissions)

los usuarios con este rol tienen los mismos permisos que el rol de administrador de la aplicación, excluida la capacidad de administrar el proxy de aplicación. Este rol concede la capacidad de crear y administrar todos los aspectos de las aplicaciones empresariales y los registros de aplicaciones. Este rol también proporciona la capacidad de dar el consentimiento para permisos delegados y permisos de aplicaciones, excepto Microsoft Graph API. Los usuarios asignados a este rol no se agregan como propietarios al crear nuevos registros de aplicaciones o aplicaciones empresariales.

Los administradores de aplicaciones en la nube pueden administrar las credenciales de aplicación que les permite suplantar la aplicación. Por lo tanto, los usuarios asignados a este rol pueden administrar las credenciales de aplicación únicamente de las aplicaciones que no están asignadas a ningún rol de Azure AD ni a los asignados a los roles de administrador siguientes:
* Desarrollador de aplicaciones
* Administrador de aplicaciones en la nube
* Lectores de directorios

Si se asigna una aplicación a cualquier otro rol que no se menciona anteriormente, el administrador de la aplicación en la nube no puede administrar las credenciales de esa aplicación.

### <a name="cloud-device-administrator"></a>[Administrador de dispositivos en la nube](#cloud-device-administrator-permissions)

los usuarios de este rol pueden habilitar, deshabilitar y eliminar dispositivos en Azure AD y leer las claves de BitLocker de Windows 10 (si las hay) en Azure Portal. El rol no concede permisos para administrar otras propiedades en el dispositivo.

### <a name="compliance-administrator"></a>[Administrador de cumplimiento](#compliance-administrator-permissions)

Los usuarios con este rol tienen permisos para administrar las características relacionadas con el cumplimiento en el Centro de cumplimiento de Microsoft 365, el Centro de administración de Microsoft 365, Azure y el Centro de seguridad y cumplimiento de Office 365. Los usuarios asignados también pueden crear vales de soporte técnico de Azure y Microsoft 365. Más información disponible en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

En | Puede hacer
----- | ----------
[Centro de cumplimiento de Microsoft 365](https://protection.office.com) | Proteger y administrar los datos de la organización en los servicios de Microsoft 365<br>Administrar las alertas de cumplimiento
[Administrador de cumplimiento](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Controlar, asignar y verificar las actividades de cumplimiento normativo de su organización
[Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Administrar la gobernanza de datos<br>Realizar investigaciones legales y de datos<br>Administrar solicitudes de interesados de datos<br><br>Este rol tiene los mismos permisos que el [grupo de roles Administrador de cumplimiento](https://docs.microsoft.com/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) del control de acceso basado en rol del Centro de seguridad y cumplimiento de Office 365.
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Ver todos los datos de auditoría de Intune
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Tiene permisos de solo lectura y puede administrar alertas<br>Puede crear y modificar las directivas de archivo y permitir acciones de gobernanza de archivos<br>Puede ver todos los informes integrados en Administración de datos

### <a name="compliance-data-administrator"></a>[Administrador de datos de cumplimiento](#compliance-data-administrator-permissions)

Los usuarios con este rol tienen permisos para realizar un seguimiento de los datos del Centro de cumplimiento de Microsoft 365, el Centro de administración de Microsoft 365 y Azure. Los usuarios también pueden hacer un seguimiento de los datos de cumplimiento en el Centro de administración de Exchange, el Administrador de cumplimiento y el Centro de administración de Teams y Skype for Business, así como crear incidencias de soporte técnico para Azure y Microsoft 365.

En | Puede hacer
----- | ----------
[Centro de cumplimiento de Microsoft 365](https://protection.office.com) | Supervisión de las directivas relacionadas con el cumplimiento en servicios de Microsoft 365<br>Administrar las alertas de cumplimiento
[Administrador de cumplimiento](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Controlar, asignar y verificar las actividades de cumplimiento normativo de su organización
[Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Administrar la gobernanza de datos<br>Realizar investigaciones legales y de datos<br>Administrar solicitudes de interesados de datos<br><br>Este rol tiene los mismos permisos que el [grupo de roles Administrador de datos de cumplimiento](https://docs.microsoft.com/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) del control de acceso basado en rol del Centro de seguridad y cumplimiento de Office 365.
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Ver todos los datos de auditoría de Intune
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Tiene permisos de solo lectura y puede administrar alertas<br>Puede crear y modificar las directivas de archivo y permitir acciones de gobernanza de archivos<br>Puede ver todos los informes integrados en Administración de datos

### <a name="conditional-access-administrator"></a>[Administrador de acceso condicional](#conditional-access-administrator-permissions)

los usuarios con este rol tienen la capacidad de administrar la configuración de acceso condicional de Azure Active Directory.
> [!NOTE]
> Para implementar la directiva de acceso condicional de Exchange ActiveSync en Azure, el usuario también debe ser administrador global.

### <a name="customer-lockbox-access-approver"></a>[Aprobador del acceso a la Caja de seguridad del cliente](#customer-lockbox-access-approver-permissions)

Administra las [solicitudes de la Caja de seguridad del cliente](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) de la organización. Reciben notificaciones por correo electrónico para las solicitudes de la Caja de seguridad del cliente y pueden aprobar y rechazar solicitudes del centro de administración de Microsoft 365. También pueden activar o desactivar la característica de la Caja de seguridad del cliente. Recuerde que solo los administradores globales pueden restablecer las contraseñas de las personas asignadas a este rol.

### <a name="desktop-analytics-administrator"></a>[Administrador de Análisis de escritorio](#desktop-analytics-administrator-permissions)


los usuarios con este rol pueden administrar los servicios Análisis de escritorio y de personalización y directivas de Office. En el caso de Análisis de escritorio, esto incluye la posibilidad de ver el inventario de recursos, crear planes de implementación y ver la implementación y el estado de mantenimiento. En el caso del servicio de personalización y directivas de Office, este rol permite a los usuarios administrar las directivas de Office.

### <a name="device-administrator"></a>[Administrador de dispositivos](#device-administrators-permissions)

este rol está disponible solo para la asignación como un administrador local adicional en la [Configuración del dispositivo](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Los usuarios con este rol pasarán a ser administradores del equipo local en todos los dispositivos Windows 10 que estén unidos a Azure Active Directory. No tienen la capacidad de administrar objetos de dispositivos en Azure Active Directory.

### <a name="directory-readers"></a>[Lectores de directorio](#directory-readers-permissions)

Los usuarios que tienen este rol pueden leer la información básica del directorio. Debe utilizarse para:
* Conceder acceso de lectura a un conjunto específico de usuarios invitados, en lugar de hacerlo para todos los usuarios invitados.
* Conceder acceso a Azure Portal a un conjunto específico de usuarios que no sean administradores cuando la opción "Restringir el acceso al portal de administración de Azure AD" esté establecida en "Sí".
* Conceder acceso a las entidades de servicio a aquellos directorios en los que Directory.Read.All no sea una opción.

### <a name="directory-synchronization-accounts"></a>[Cuentas de sincronización de directorios](#directory-synchronization-accounts-permissions)

No debe usarse. Este rol se asigna automáticamente al servicio de Azure AD Connect y no está previsto ni se admite para ningún otro uso.

### <a name="directory-writers"></a>[Escritores de directorios](#directory-writers-permissions)

es un rol heredado que se asigna a las aplicaciones que no admiten el [marco de consentimiento](../develop/quickstart-register-app.md). No se debe asignar a ningún usuario.

### <a name="dynamics-365-administrator--crm-administrator"></a>[Administrador de Dynamics 365/administrador de CRM](#crm-service-administrator-permissions)

los usuarios con este rol tienen permisos globales en Microsoft Dynamics 365 Online, cuando el servicio está presente, así como también la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio. Para obtener más información, consulte [Uso del rol de administrador de servicios para administrar inquilinos](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

> [!NOTE]
> En Microsoft Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de Dynamics 365". En [Azure Portal](https://portal.azure.com) es "Administrador de Dynamics 365".

### <a name="exchange-administrator"></a>[Administrador de Exchange](#exchange-service-administrator-permissions)

los usuarios con este rol tienen permisos globales en Microsoft Exchange Online, cuando el servicio está presente. También se ofrece la capacidad de crear y administrar todos los Grupos de Office 365, administrar las incidencias de soporte técnico y supervisar el estado del servicio. Más información en [Acerca de los roles de administrador de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

> [!NOTE]
> En Microsoft Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de Exchange". En [Azure Portal](https://portal.azure.com) es "Administrador de Exchange". En el [Centro de administración de Exchange](https://go.microsoft.com/fwlink/p/?LinkID=529144) es "Administrador de Exchange Online".

### <a name="external-identity-provider-administrator"></a>[Administrador de proveedor de identidades externo](#external-identity-provider-administrator-permissions)

Este administrador administra la federación entre los inquilinos de Azure Active Directory y los proveedores de identidades externos. Con este rol, los usuarios pueden agregar nuevos proveedores de identidades y configurar todos los valores disponibles (por ejemplo, la ruta de autenticación, el identificador de servicio o los contenedores de claves asignados). Este usuario puede habilitar el inquilino para que confíe en las autenticaciones de los proveedores de identidades externos. El efecto resultante en las experiencias del usuario final depende del tipo de inquilino:

* Inquilinos de Azure Active Directory para empleados y asociados: La adición de una federación (por ejemplo, con Gmail) afectará inmediatamente a todas las invitaciones de invitado que no se hayan canjeado aún. Consulte [Incorporación de Google como proveedor de identidades para los usuarios invitados de B2B](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).
* Inquilinos de Azure Active Directory B2C: La adición de una federación (por ejemplo, con Facebook o con otra organización de Azure AD) no afecta inmediatamente a los flujos de usuario final hasta que se agrega el proveedor de identidades como una opción de un flujo de usuario (también conocido como directiva integrada). Consulte [Configuración de una cuenta Microsoft como proveedor de identidades](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app) para ver un ejemplo. Para cambiar los flujos de usuario, se requiere el rol limitado de "Administrador de flujos de usuario B2C".

### <a name="global-administrator--company-administrator"></a>[Administrador global/administrador de empresa](#company-administrator-permissions)

Los usuarios con este rol tienen acceso a todas las características administrativas en Azure Active Directory, así como también a los servicios que usan las identidades de Azure Active Directory, como el Centro de seguridad de Microsoft 365, el Centro de cumplimiento de Microsoft 365, Exchange Online, SharePoint Online y Skype Empresarial Online. La persona que se suscribe al inquilino de Azure Active Directory se convierte en un administrador global. Puede haber más de un administrador global en su empresa. Los administradores globales pueden restablecer la contraseña de todos los usuarios y de todos los demás administradores.

> [!NOTE]
> En Microsoft Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de la compañía". Es "administrador Global" en [Azure Portal](https://portal.azure.com).
>
>

### <a name="global-reader"></a>[Lector global](#global-reader-permissions)

Los usuarios de este rol pueden leer la configuración y la información administrativa en los servicios de Microsoft 365, pero no pueden llevar a cabo acciones de administración. El rol del lector global es la contrapartida de solo lectura del administrador global. Asigne un lector global en lugar de un administrador global para planeamiento, auditoría o investigación. Use el rol de lector global en combinación con otros roles de administrador limitados, como el de administrador de Exchange, para facilitar que se lleve a cabo el trabajo sin asignar el rol de administrador global. El rol de lector global funciona con el Centro de administración de Microsoft 365, el Centro de administración de Exchange, el Centro de administración de Teams, el Centro de seguridad, el Centro de cumplimiento, el Centro de administración de Azure AD y el Centro de administración de la administración de dispositivos.

> [!NOTE]
> Actualmente, el rol de lector global tiene algunas limitaciones:
>
>- Centro de administración de SharePoint: el Centro de administración de SharePoint no admite el rol de lector global. No verá "SharePoint" en el panel izquierdo debajo de Centros de administración en el [Centro de administración de Microsoft 365](https://admin.microsoft.com/Adminportal/Home#/homepage).
>- [Centro de administración de OneDrive](https://admin.onedrive.com/): el Centro de administración de OneDrive no admite el rol de lector global.
>- [Portal de Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/): el lector global no puede leer el modo de aprovisionamiento de una aplicación empresarial.
>- [Centro de administración de M365](https://admin.microsoft.com/Adminportal/Home#/homepage): el lector global no puede leer las solicitudes de la Caja de seguridad del cliente. En el panel izquierdo del Centro de administración de M365 no aparecerá la pestaña **Solicitudes de la Caja de seguridad del cliente** bajo **Soporte**.
>- [Centro de seguridad de M365](https://security.microsoft.com/homepage): el lector global no puede leer las etiquetas de confidencialidad y retención. No encontrará las pestañas **Etiquetas de confidencialidad**, **Etiquetas de retención** ni **Análisis de etiquetas** en el panel izquierdo del Centro de seguridad de M365.
>- [Centro de seguridad y cumplimiento de Office](https://sip.protection.office.com/homepage): el lector global no puede leer los registros de auditoría de SCC, realizar búsqueda de contenido ni consultar la puntuación de seguridad.
>- [Centro de administración de Teams](https://admin.teams.microsoft.com): el lector global no puede leer **Ciclo de vida de Teams**, **Análisis e informes**, **Administración de dispositivos de teléfono IP** ni **Catálogo de aplicaciones**.
>- [Privileged Access Management (PAM)](https://docs.microsoft.com/office365/securitycompliance/privileged-access-management-overview) no admite el rol de lector global.
>- [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/what-is-information-protection): el lector global solo se admite para la [generación de informes centrales](https://docs.microsoft.com/azure/information-protection/reports-aip) y cuando la organización de Azure AD no forma parte de la [plataforma unificada de etiquetado](https://docs.microsoft.com/azure/information-protection/faqs#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform).
>
> Estas características están actualmente en desarrollo.
>

### <a name="groups-administrator"></a>[Administrador de grupos](#groups-administrator-permissions)

Los usuarios de este rol pueden crear y administrar grupos y su configuración como directivas de nomenclatura y expiración. Es importante comprender que la asignación de un usuario a este rol les permite administrar todos los grupos en el inquilino en varias cargas de trabajo, como Teams, SharePoint, Yammer y Outlook. Además, el usuario podrá administrar la configuración de varios grupos en varios portales de administración, como el centro de administración de Microsoft, Azure Portal, así como los específicos de cargas de trabajo, como Teams y los centros de administración de SharePoint.

### <a name="guest-inviter"></a>[Invitador de usuarios](#guest-inviter-permissions)

los usuarios con este rol pueden administrar las invitaciones de usuarios invitados de Azure Active Directory B2B cuando la configuración de usuario **Los miembros pueden invitar a otras personas** está establecida en No. Más información sobre la colaboración B2B en [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) No incluye otros permisos.

### <a name="helpdesk-administrator"></a>[Administrador del departamento de soporte técnico](#helpdesk-administrator-permissions)

los usuarios con este rol pueden cambiar contraseñas, invalidar tokens de actualización, administrar solicitudes de servicio y supervisar el estado del servicio. Si invalida un token de actualización, obligará al usuario a iniciar sesión de nuevo. Los administradores del departamento de soporte técnico pueden restablecer las contraseñas e invalidar los tokens de actualización de otros usuarios que no sean administradores o asignar los siguientes roles solamente:

* Lectores de directorios
* Invitador de usuarios
* Administrador del departamento de soporte técnico
* Lector del Centro de mensajes
* Lector de informes

> [!IMPORTANT]
> Los usuarios con este rol pueden cambiar las contraseñas de las personas que pueden tener acceso a información confidencial o privada o configuración crítica dentro y fuera de Azure Active Directory. Cambiar la contraseña de un usuario puede significar la capacidad de asumir la identidad y los permisos del usuario. Por ejemplo:
>
>- Propietarios de registro de la aplicación y la aplicación de empresa, que pueden administrar las credenciales de las aplicaciones que poseen. Esas aplicaciones pueden tener permisos con privilegios en Azure AD y en otra parte no concederlos a los administradores del departamento de soporte técnico. Mediante esta ruta de acceso, un administrador del departamento de soporte técnico puede ser capaz de asumir la identidad del propietario de la aplicación y después asumir la identidad de una aplicación con privilegios mediante la actualización de las credenciales de la aplicación.
>- Propietarios de suscripción de Azure, que pueden tener acceso a información confidencial o privada o configuración crítica en Azure.
>- Propietarios del grupo de seguridad y el grupo de Office 365, que pueden administrar la pertenencia a grupos. Dichos grupos pueden conceder acceso a información confidencial o privada o a configuración crítica en Azure AD y en cualquier otra parte.
>- Los administradores de otros servicios fuera de Azure AD, como Exchange Online, Office Security y Compliance Center y sistemas de recursos humanos.
>- Usuarios no administradores como empleados ejecutivos, de asesoramiento jurídico y de recursos humanos que pueden tener acceso a información confidencial o privada.

Delegar permisos administrativos en subconjuntos de usuarios y aplicar directivas a uno de estos subconjuntos es posible con [Unidades administrativas (ahora en versión preliminar pública)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units).

Este rol se llamaba anteriormente "Administrador de contraseñas" en [Azure Portal](https://portal.azure.com/). El nombre "Administrador del departamento de soporte técnico" ahora coincide en Azure AD PowerShell y Microsoft Graph API.

### <a name="intune-administrator"></a>[Administrador de Intune](#intune-service-administrator-permissions)

los usuarios con este rol tienen permisos globales en Microsoft Intune Online, cuando existe el servicio. Además, este rol contiene la capacidad de administrar usuarios y dispositivos para asociar una directiva, así como también para crear y administrar grupos. Para más información, consulte [Control de administración basado en rol (RBAC) con Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control).

Este rol puede crear y administrar todos los grupos de seguridad. Sin embargo, el administrador de Intune no tiene derechos de administrador sobre grupos de Office. Esto significa que el administrador no puede actualizar propietarios o pertenencias de todos los grupos de Office del inquilino. Sin embargo, puede administrar el grupo de Office que crea y que forma parte de sus privilegios de usuario final. Por lo tanto, cualquier grupo de Office (no un grupo de seguridad) que cree debe contar con su cuota de 250.

> [!NOTE]
> En Microsoft Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de Intune". En [Azure Portal](https://portal.azure.com) es "Administrador de Intune".

### <a name="kaizala-administrator"></a>[Administrador de Kaizala](#kaizala-administrator-permissions)

los usuarios con este rol tienen permisos globales para administrar la configuración en Microsoft Kaizala, cuando existe el servicio, así como también la posibilidad de administrar incidencias de soporte técnico y supervisar el mantenimiento del servicio. Además, el usuario puede acceder a informes relacionados con la adopción y el uso de Kaizala por parte de miembros de la organización, y a los informes empresariales que generan las acciones de Kaizala.

### <a name="license-administrator"></a>[Administrador de licencias](#license-administrator-permissions)

los usuarios con este rol pueden agregar, quitar y actualizar las asignaciones de licencias de usuarios y grupos (mediante licencias basadas en grupo) y administrar la ubicación de uso de los usuarios. El rol no otorga la posibilidad de adquirir o administrar suscripciones, crear o administrar grupos o crear o administrar usuarios más allá de la ubicación de uso. Este rol no tiene acceso para ver, crear o administrar incidencias de soporte técnico.

### <a name="message-center-privacy-reader"></a>[Lector de privacidad del Centro de mensajes](#message-center-privacy-reader-permissions)

los usuarios con este rol pueden supervisar todas las notificaciones del Centro de mensajes, incluidos los mensajes de privacidad de datos. Los lectores de privacidad del Centro de mensajes reciben notificaciones por correo electrónico, incluidas las relacionadas con la privacidad de los datos, y pueden cancelar la suscripción mediante las preferencias del Centro de mensajes. Solo el administrador global y el lector de privacidad del Centro de mensajes pueden leer los mensajes de privacidad de los datos. Además, este rol incluye la posibilidad de ver los grupos, dominios y suscripciones. Este rol no tiene permiso para ver, crear o administrar solicitudes de servicio.

### <a name="message-center-reader"></a>[Lector del centro de mensajes](#message-center-reader-permissions)

los usuarios con este rol pueden supervisar las notificaciones y las actualizaciones de mantenimiento de aviso en el [Centro de mensajes de Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) para su organización en los servicios configurados, como Exchange, Intune y Microsoft Teams. Los lectores del centro de mensajes reciben semanal resúmenes de correo electrónico de publicaciones y actualizaciones y pueden compartir entradas del centro de mensajes en Office 365. En Azure AD, los usuarios asignados a este rol solo tendrán acceso de solo lectura en los servicios de Azure AD, como usuarios y grupos. Este rol no tiene acceso para ver, crear o administrar incidencias de soporte técnico.

### <a name="office-apps-administrator"></a>[Administrador de aplicaciones de Office](#office-apps-administrator-permissions)

Los usuarios de este rol pueden administrar la configuración de la nube de las aplicaciones de Office 365. Esto incluye la administración de directivas en la nube, la administración de la descarga de autoservicio y la capacidad de ver el informe relacionado con las aplicaciones de Office. Este rol además ofrece la capacidad de crear y administrar las incidencias de soporte técnico y supervisar el estado del servicio. Los usuarios asignados a este rol también pueden administrar la comunicación de las nuevas características de las aplicaciones de Office. 

### <a name="partner-tier1-support"></a>[Soporte para asociados de nivel 1](#partner-tier1-support-permissions)

No debe usarse. Este rol está en desuso y se quitará de Azure AD en el futuro. Este rol está diseñado para que lo usen un pequeño número de asociados de reventa de Microsoft, no para un uso general.

### <a name="partner-tier2-support"></a>[Soporte para asociados de nivel 2](#partner-tier2-support-permissions)

No debe usarse. Este rol está en desuso y se quitará de Azure AD en el futuro. Este rol está diseñado para que lo usen un pequeño número de asociados de reventa de Microsoft, no para un uso general.

### <a name="password-administrator"></a>[Administrador de contraseñas](#password-administrator-permissions)

los usuarios con este rol tienen una capacidad limitada para administrar las contraseñas. Este rol no concede la capacidad de administrar solicitudes de servicio ni supervisar el estado del servicio. Los administradores de contraseñas pueden restablecer contraseñas de otros usuarios que no son administradores o que solo son miembros de los roles siguientes:

* Lectores de directorios
* Invitador de usuarios
* Administrador de contraseñas

### <a name="power-bi-administrator"></a>[Administrador de Power BI](#power-bi-service-administrator-permissions)

los usuarios con este rol tienen permisos globales en Microsoft Power BI, cuando existe el servicio, así como también la posibilidad de administrar incidencias de soporte técnico y supervisar el mantenimiento del servicio. Puede obtener más información en el artículo [Descripción del rol de administrador de Power BI](https://docs.microsoft.com/power-bi/service-admin-role).

> [!NOTE]
> En Microsoft Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de Power BI". En [Azure Portal](https://portal.azure.com) es "Administrador de Power BI".

### <a name="power-platform-administrator"></a>[Administrador de Power Platform](#power-platform-administrator-permissions)

Los usuarios de este rol pueden crear y administrar todos los aspectos de las directivas de entornos, PowerApps, flujos y prevención de pérdida de datos. Además, los usuarios con este rol tienen la capacidad de administrar incidencias de soporte técnico y supervisar el estado del servicio.

### <a name="privileged-authentication-administrator"></a>[Administrador de autenticación con privilegios](#privileged-authentication-administrator-permissions)

los usuarios con este rol pueden establecer o restablecer credenciales sin contraseña para todos los usuarios, incluidos los administradores globales, y pueden actualizar las contraseñas de todos los usuarios. Los administradores de autenticación con privilegios pueden forzar a los usuarios a que vuelvan a registrarse con una credencial existente distinta a una contraseña (por ejemplo, MFA, FIDO) y revocar la opción "Recordar MFA en el dispositivo", por lo que se solicitará la MFA en el siguiente inicio de sesión de todos los usuarios.

### <a name="privileged-role-administrator"></a>[Administrador de roles con privilegios](#privileged-role-administrator-permissions)

los usuarios con este rol pueden administrar asignaciones de roles en Azure Active Directory, así como en Azure AD Privileged Identity Management. Además, este rol permite administrar todos los aspectos de Privileged Identity Management y de las unidades administrativas.

> [!IMPORTANT]
> Este rol concede la capacidad de administrar las asignaciones de todos los roles de Azure AD, incluido el rol de administrador global. Este rol no incluye ninguna otra capacidad con privilegios en Azure AD, como crear o actualizar los usuarios. Sin embargo, los usuarios asignados a este rol pueden concederse a sí mismos o a otros usuarios privilegios adicionales mediante la asignación de roles adicionales.

### <a name="reports-reader"></a>[Lector de informes](#reports-reader-permissions)

los usuarios con este rol pueden ver datos de informes de uso y el panel de informes en el centro de administración de Microsoft 365 y el paquete del contexto de adopción en Power BI. Además, el rol proporciona acceso a informes de inicio de sesión y a actividad de Azure AD y a los datos devueltos por la API de informes de Microsoft Graph. Un usuario asignado al rol de lector de informes puede acceder solo a métricas de uso y adopción pertinentes. No tienen permisos de administrador para configurar valores ni acceder a los centros de administración específicos de productos como Exchange. Este rol no tiene acceso para ver, crear o administrar incidencias de soporte técnico.

### <a name="search-administrator"></a>[Administrador de búsqueda](#search-administrator-permissions)

los usuarios con este rol tienen acceso total a las características de administración de Búsqueda de Microsoft en el centro de administración de Microsoft 365. Los administradores de búsqueda pueden delegar los roles de Administrador de búsqueda y Editor de búsqueda en los usuarios, y pueden crear y administrar contenido como marcadores, preguntas y respuestas y ubicaciones. Además, estos usuarios pueden ver el Centro de mensajes, supervisar el estado del servicio y crear solicitudes de servicio.

### <a name="search-editor"></a>[Editor de búsqueda](#search-editor-permissions)

los usuarios con este rol pueden crear, administrar y eliminar contenido de Búsqueda de Microsoft en el centro de administración de Microsoft 365, incluidos los marcadores, las preguntas y respuestas y las ubicaciones.

### <a name="security-administrator"></a>[Administrador de seguridad](#security-administrator-permissions)

Los usuarios con este rol tienen permisos para administrar las características relacionadas con la seguridad en el Centro de seguridad de Microsoft 365, Azure Active Directory Identity Protection, Azure Information Protection y el Centro de seguridad y cumplimiento de Office 365. Encuentre más información disponible sobre los permisos de Office 365 en [Permisos del Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

En | Puede hacer
--- | ---
[Centro de seguridad de Microsoft 365](https://protection.office.com) | Supervisar las directivas relacionadas con la seguridad en servicios de Microsoft 365<br>Administrar alertas y amenazas de seguridad<br>Ver informes
Identity Protection Center | Todos los permisos del rol Lector de seguridad<br>Además, la posibilidad de realizar todas las operaciones de Identity Protection Center, excepto la de restablecer contraseñas
[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Todos los permisos del rol Lector de seguridad<br>**No puede** administrar la configuración ni la asignación de roles de Azure AD
[Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Administrar directivas de seguridad<br>Ver e investigar amenazas de seguridad y responder a ellas<br>Ver informes
Azure Advanced Threat Protection | Supervisar la actividad sospechosa de seguridad y responder a ella
EDR y ATP de Windows Defender | Asignación de roles<br>Administración de grupos de máquinas<br>Configurar la detección de amenazas de punto de conexión y la corrección automatizada<br>Ver e investigar alertas y responder a ellas
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Ver información sobre usuarios, dispositivos, inscripciones, configuración y aplicaciones<br>No se pueden realizar cambios en Intune
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Agregar administradores, agregar directivas y configuraciones, cargar registros y realizar acciones de gobernanza
[Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | puede ver las directivas de seguridad, los estados de seguridad, editar las directivas de seguridad, ver alertas y recomendaciones, y descartar alertas y recomendaciones.
[Estado del servicio de Office 365](https://docs.microsoft.com/office365/enterprise/view-service-health) | Ver el estado de los servicios de Office 365

### <a name="security-operator"></a>[Operador de seguridad](#security-operator-permissions)

Los usuarios con este rol pueden administrar alertas y tienen acceso global de solo lectura en características relacionadas con la seguridad, incluida toda la información del Centro de seguridad de Microsoft 365, Azure Active Directory, Identity Protection, Privileged Identity Management y el Centro de seguridad y cumplimiento de Office 365. Encuentre más información disponible sobre los permisos de Office 365 en [Permisos del Centro de seguridad y cumplimiento de Office 365](https://docs.microsoft.com/office365/securitycompliance/permissions-in-the-security-and-compliance-center).

En | Puede hacer
--- | ---
[Centro de seguridad de Microsoft 365](https://protection.office.com) | Todos los permisos del rol Lector de seguridad<br>Ver e investigar amenazas de seguridad y responder a ellas
Identity Protection Center | Todos los permisos del rol Lector de seguridad<br>Además, la posibilidad de realizar todas las operaciones de Identity Protection Center, excepto la de restablecer contraseñas
[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Todos los permisos del rol Lector de seguridad
[Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Todos los permisos del rol Lector de seguridad<br>Ver e investigar amenazas de seguridad y responder a ellas
EDR y ATP de Windows Defender | Todos los permisos del rol Lector de seguridad<br>Ver e investigar amenazas de seguridad y responder a ellas
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Todos los permisos del rol Lector de seguridad
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Todos los permisos del rol Lector de seguridad
[Estado del servicio de Office 365](https://docs.microsoft.com/office365/enterprise/view-service-health) | Ver el estado de los servicios de Office 365

### <a name="security-reader"></a>[Lector de seguridad](#security-reader-permissions)

Los usuarios con este rol tienen acceso global de solo lectura en la característica relacionada con la seguridad, incluida toda la información del Centro de seguridad de Microsoft 365, Azure Active Directory, Identity Protection, Privileged Identity Management, así como también la capacidad de leer registros de auditoría e informes de inicio de sesión de Azure Active Directory, y del Centro de seguridad y cumplimiento de Office 365. Encuentre más información disponible sobre los permisos de Office 365 en [Permisos del Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

En | Puede hacer
--- | ---
[Centro de seguridad de Microsoft 365](https://protection.office.com) | Ver las directivas relacionadas con la seguridad en los servicios de Microsoft 365<br>Ver las alertas y amenazas de seguridad<br>Ver informes
Identity Protection Center | Leer todos los informes de seguridad y la información de configuración de las características de seguridad<br><ul><li>Filtro de correo no deseado<li>Cifrado<li>Prevención de la pérdida de datos<li>Antimalware<li>Protección contra amenazas avanzada<li>Protección contra suplantación de identidad (anti-phishing)<li>Reglas de flujo de correo
[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Tiene acceso de solo lectura a toda la información que aparece en Azure AD Privileged Identity Management: Directivas e informes para las asignaciones de roles y revisiones de seguridad de Azure AD.<br>**No se puede** suscribir a Azure AD Privileged Identity Management ni realizar cambios en él. En el portal de Privileged Identity Management o mediante PowerShell, alguien con este rol puede activar roles adicionales (por ejemplo, Administrador global o Administrador de rol con privilegios), si el usuario es apto para ellos.
[Centro de seguridad y cumplimiento de Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Visualización de directivas de seguridad<br>Ver e investigar las amenazas de seguridad<br>Ver informes
EDR y ATP de Windows Defender | Ver e investigar alertas. Al activar el control de acceso basado en rol en ATP de Windows Defender, los usuarios con permisos de solo lectura como el rol de lector de seguridad Azure AD pierden el acceso hasta que se asignan a un rol de ATP de Windows Defender.
[Intune](https://docs.microsoft.com/intune/role-based-access-control) | Ver información sobre usuarios, dispositivos, inscripciones, configuración y aplicaciones No se pueden realizar cambios en Intune
[Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Tiene permisos de solo lectura y puede administrar alertas
[Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | puede ver las recomendaciones y alertas, ver las directivas de seguridad y ver los estados de seguridad, pero no puede realizar cambios
[Estado del servicio de Office 365](https://docs.microsoft.com/office365/enterprise/view-service-health) | Ver el estado de los servicios de Office 365

### <a name="service-support-administrator"></a>[Administrador de soporte técnico de servicios](#service-support-administrator-permissions)

los usuarios con este rol pueden abrir incidencias de soporte técnico con Microsoft para servicios de Azure y Office 365, y consultar el centro de mensajes y el panel de servicios de [Azure Portal](https://portal.azure.com) y el [centro de administración de Microsoft 365](https://admin.microsoft.com). Más información en [Acerca de los roles de administrador](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

> [!NOTE]
> Antes, este rol se llamaba "Administrador de servicios" en [Azure Portal](https://portal.azure.com) y en el [Centro de administración de Microsoft 365](https://admin.microsoft.com). Ahora, se denomina "Administrador de soporte técnico del servicio" para que coincida con el nombre ya existente en Microsoft Graph API, Graph API de Azure AD y Azure AD PowerShell.

### <a name="sharepoint-administrator"></a>[Administrador de SharePoint](#sharepoint-service-administrator-permissions)

los usuarios con este rol tienen permisos globales en Microsoft SharePoint Online, cuando existe el servicio, así como también la posibilidad de crear y administrar todos los grupos de Office 365, administrar las incidencias de soporte técnico y supervisar el mantenimiento del servicio. Más información en [Acerca de los roles de administrador](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

> [!NOTE]
> En Microsoft Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de SharePoint". En [Azure Portal](https://portal.azure.com) es "Administrador de SharePoint".

### <a name="skype-for-business--lync-administrator"></a>[Administrador de Skype Empresarial o Lync](#lync-service-administrator-permissions)

los usuarios con este rol tienen permisos globales en Microsoft Skype Empresarial, cuando el servicio está presente, así como la posibilidad de administrar atributos de usuario específicos de Skype en Azure Active Directory. Además, este rol concede la posibilidad de administrar incidencias de soporte técnico, supervisar el estado del servicio y acceder al centro de administración de Teams y de Skype Empresarial. La cuenta también debe tener licencia para Teams o no podrá ejecutar los cmdlets de PowerShell de Teams. Para más información, visite [Acerca del rol de administrador de Skype Empresarial](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) y para ver la información de licencias de Teams, consulte [Licencias complementarias de Skype Empresarial y Microsoft Teams](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

> [!NOTE]
> En Microsoft Graph API y Azure AD PowerShell, este rol se identifica como "Administrador de servicios de Lync". En [Azure Portal](https://portal.azure.com/) es "Administrador de Skype Empresarial".

### <a name="teams-communications-administrator"></a>[Administrador de comunicaciones de Teams](#teams-communications-administrator-permissions)

los usuarios con este rol pueden administrar aspectos de la carga de trabajo de Microsoft Teams relacionados con la voz y la telefonía. Esto incluye las herramientas de administración para la asignación de números de teléfono, directivas de voz y reunión, y acceso total al conjunto de herramientas de análisis de llamada.

### <a name="teams-communications-support-engineer"></a>[Ingeniero de soporte técnico de comunicaciones de Teams](#teams-communications-support-engineer-permissions)

los usuarios con este rol pueden solucionar problemas de comunicación de Microsoft Teams y Skype Empresarial mediante las herramientas de solución de problemas de llamadas del usuario del centro de administración de Microsoft Teams y Skype Empresarial. Los usuarios con este rol pueden ver la información completa de registro de llamadas de todos los participantes implicados. Este rol no tiene acceso para ver, crear o administrar incidencias de soporte técnico.

### <a name="teams-communications-support-specialist"></a>[Especialista de soporte técnico de comunicaciones de Teams](#teams-communications-support-specialist-permissions)

los usuarios con este rol pueden solucionar problemas de comunicación de Microsoft Teams y Skype Empresarial mediante las herramientas de solución de problemas de llamadas del usuario del centro de administración de Microsoft Teams y Skype Empresarial. Los usuarios con este rol solo pueden ver los detalles del usuario en la llamada al usuario específico que han buscado. Este rol no tiene acceso para ver, crear o administrar incidencias de soporte técnico.

### <a name="teams-service-administrator"></a>[Administrador del servicio Teams](#teams-service-administrator-permissions)

los usuarios con este rol pueden administrar todos los aspectos de la carga de trabajo de Microsoft Teams a través del centro de administración de Microsoft Teams y Skype Empresarial y los módulos de PowerShell correspondientes. Esto incluye, entre otras áreas, todas las herramientas de administración relacionadas con telefonía, mensajería, reuniones y los propios equipos. Este rol además ofrece la capacidad de crear y administrar todos los Grupos de Office 365, administrar las incidencias de soporte técnico y supervisar el estado del servicio.

### <a name="user-administrator"></a>[Administrador de usuarios](#user-administrator-permissions)

los usuarios con este rol pueden crear y administrar todos los aspectos de los usuarios con algunas restricciones (véase la información siguiente) y pueden actualizar las directivas de expiración de las contraseñas. Además, los usuarios con este rol pueden crear y administrar todos los grupos. Este rol también incluye la capacidad de crear y administrar vistas de usuarios, administrar las incidencias de soporte técnico y supervisar el estado del servicio. Los administradores de usuarios no tienen permiso para administrar algunas propiedades de usuario en la mayoría de los roles de administrador. El usuario con este rol no tiene los permisos para administrar MFA. En la tabla siguiente se indican los roles que son excepciones a esta restricción.

| | |
| --- | --- |
|Permisos globales|<p>Creación de usuarios y grupos</p><p>Crear y administrar vistas de usuario</p><p>Administrar incidencias de soporte técnico de Office<p>Actualización de directivas de expiración de contraseñas|
|<p>En todos los usuarios, incluidos todos los administradores</p>|<p>Administrar licencias</p><p>Administrar todas las propiedades de usuario, excepto el nombre principal de usuario</p>
|Solo en los usuarios que no son administradores o en cualquiera de los siguientes roles de administrador limitados:<ul><li>Lectores de directorios<li>Invitador de usuarios<li>Administrador del departamento de soporte técnico<li>Lector del Centro de mensajes<li>Lector de informes<li>Administrador de usuarios|<p>Eliminar y restaurar</p><p>Deshabilitar y habilitar</p><p>Invalidar tokens de actualización</p><p>Administrar todas las propiedades de usuario, incluido el nombre principal de usuario</p><p>Restablecimiento de contraseña</p><p>Actualizar las claves de dispositivo (FIDO)</p>|

> [!IMPORTANT]
> Los usuarios con este rol pueden cambiar las contraseñas de las personas que pueden tener acceso a información confidencial o privada o configuración crítica dentro y fuera de Azure Active Directory. Cambiar la contraseña de un usuario puede significar la capacidad de asumir la identidad y los permisos del usuario. Por ejemplo:
>
>- Propietarios de registro de la aplicación y la aplicación de empresa, que pueden administrar las credenciales de las aplicaciones que poseen. Esas aplicaciones pueden tener permisos con privilegios en Azure AD y en otra parte no concederlos a los administradores de usuarios. Mediante esta ruta de acceso, un administrador de usuarios puede ser capaz de asumir la identidad del propietario de la aplicación y después asumir la identidad de una aplicación con privilegios mediante la actualización de las credenciales de la aplicación.
>- Propietarios de suscripción de Azure, que pueden tener acceso a información confidencial o privada o configuración crítica en Azure.
>- Propietarios del grupo de seguridad y el grupo de Office 365, que pueden administrar la pertenencia a grupos. Dichos grupos pueden conceder acceso a información confidencial o privada o a configuración crítica en Azure AD y en cualquier otra parte.
>- Los administradores de otros servicios fuera de Azure AD, como Exchange Online, Office Security y Compliance Center y sistemas de recursos humanos.
>- Usuarios no administradores como empleados ejecutivos, de asesoramiento jurídico y de recursos humanos que pueden tener acceso a información confidencial o privada.

## <a name="role-permissions"></a>Permisos de los roles

En las tablas siguientes se describen los permisos específicos concedidos a cada rol en Azure Active Directory. Algunos roles pueden tener permisos adicionales en los servicios de Microsoft fuera de Azure Active Directory.

### <a name="application-administrator-permissions"></a>Permisos de administrador de la aplicación

Puede crear y administrar todos los aspectos de los registros de aplicaciones y de las aplicaciones empresariales.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/Application/appProxyAuthentication/update | Actualice las propiedades de autenticación del proxy de aplicación en las entidades de servicio de Azure Active Directory. |
| microsoft.directory/Application/appProxyUrlSettings/update | Actualice las direcciones URL internas y externas del proxy de aplicación en Azure Active Directory. |
| microsoft.directory/applications/applicationProxy/read | Lea todas las propiedades del proxy de aplicación. |
| microsoft.directory/applications/applicationProxy/update | Actualice todas las propiedades del proxy de aplicación. |
| microsoft.directory/applications/audience/update | Actualiza la propiedad applications.audience en Azure Active Directory. |
| microsoft.directory/applications/authentication/update | Actualiza la propiedad applications.authentication en Azure Active Directory. |
| microsoft.directory/applications/basic/update | Actualiza las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/create | Crea aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/credentials/update | Actualiza la propiedad applications.credentials en Azure Active Directory. |
| microsoft.directory/applications/delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/owners/update | Actualiza la propiedad applications.owners en Azure Active Directory. |
| microsoft.directory/applications/permissions/update | Actualiza la propiedad applications.permissions en Azure Active Directory. |
| microsoft.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/read | Lee el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/update | Actualiza el elemento appRoleAssignments en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/delete | Elimina elementos appRoleAssignments en Azure Active Directory. |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/connectorGroups/everything/read | Lea las propiedades del grupo de conectores del proxy de aplicación en Azure Active Directory. |
| microsoft.directory/connectorGroups/everything/update | Actualice todas las propiedades del grupo de conectores del proxy de aplicación en Azure Active Directory. |
| microsoft.directory/connectorGroups/create | Cree los grupos de conectores del proxy de aplicación en Azure Active Directory. |
| microsoft.directory/connectorGroups/delete | Elimine los grupos de conectores del proxy de aplicación en Azure Active Directory. |
| microsoft.directory/connectors/everything/read | Lea todas las propiedades de los conectores del proxy de aplicación en Azure Active Directory. |
| microsoft.directory/connectors/create | Cree los conectores del proxy de aplicación en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/basic/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/basic/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/create | Crea directivas en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/delete | Elimina directivas en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/owners/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/owners/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/policyAppliedTo/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/servicePrincipals/audience/update | Actualiza la propiedad servicePrincipals.audience en Azure Active Directory. |
| microsoft.directory/servicePrincipals/authentication/update | Actualiza la propiedad servicePrincipals.authentication en Azure Active Directory. |
| microsoft.directory/servicePrincipals/basic/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/create | Crea elementos sevicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/credentials/update | Actualiza la propiedad servicePrincipals.credentials en Azure Active Directory. |
| microsoft.directory/servicePrincipals/delete | Elimina servicePrincipals in Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/permissions/update | Actualiza la propiedad servicePrincipals.permissions en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="application-developer-permissions"></a>Permisos de desarrollador de la aplicación

Puede crear registros de aplicación independientemente de la opción de configuración "Los usuarios pueden registrar aplicaciones".

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/applications/createAsOwner | Crea aplicaciones en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/appRoleAssignments/createAsOwner | Crea el elemento AppRoleAssignments en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/oAuth2PermissionGrants/createAsOwner | Crea el elemento oAuth2PermissionGrants en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/servicePrincipals/createAsOwner | Crea elementos sevicePrincipals en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |

### <a name="authentication-administrator-permissions"></a>Permisos de administrador de autenticación

Puede ver, configurar y restablecer la información de los métodos de autenticación de cualquier usuario que no sea administrador.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.directory/users/strongAuthentication/update | Actualizar las propiedades de autenticación sólida, como la información de las credenciales de MFA. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.directory/users/password/update | Actualice las contraseñas de todos los usuarios de la organización de Office 365. Para obtener más información, consulte la documentación en línea. |

### <a name="azure-devops-administrator-permissions"></a>Permisos de administrador de Azure DevOps

Puede administrar la configuración y la directiva de la organización de Azure DevOps.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la [descripción del rol](#azure-devops-administrator) anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.devOps/allEntities/allTasks | Lea y configure Azure DevOps. |

### <a name="azure-information-protection-administrator-permissions"></a>Permisos de administrador de Azure Information Protection

Puede administrar todos los aspectos del servicio Azure Information Protection.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la [descripción del rol](#) anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Administra todos los aspectos de Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="b2c-user-flow-administrator-permissions"></a>Permisos de administrador de flujos de usuario B2C

Cree y administre todos los aspectos de los flujos de usuarios.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.b2c/userFlows/allTasks | Permite leer y configurar los flujos de usuario en Azure Active Directory B2C. |

### <a name="b2c-user-flow-attribute-administrator-permissions"></a>Permisos de administrador de atributos de flujos de usuario B2C

Cree y administre el esquema de atributos disponible para todos los flujos de usuario.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.b2c/userAttributes/allTasks | Permite leer y configurar atributos de usuario en Azure Active Directory B2C. |

### <a name="b2c-ief-keyset-administrator-permissions"></a>Permisos de administrador de conjuntos de claves B2C con IEF

Administre los secretos de la federación y el cifrado en Identity Experience Framework.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/keySets/allTasks | Permite leer y configurar los conjuntos de claves en Azure Active Directory B2C. |

### <a name="b2c-ief-policy-administrator-permissions"></a>Permisos de administrador de directivas B2C con IEF

Cree y administre las directivas del marco de confianza en Identity Experience Framework.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/policies/allTasks | Permite leer y configurar las directivas personalizadas en Azure Active Directory B2C. |

### <a name="billing-administrator-permissions"></a>Permisos de administrador de facturación

Puede realizar tareas comunes relacionadas con la facturación como actualizar la información de pago.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/organization/basic/update | Actualiza las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Administra todos los aspectos de la facturación de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="cloud-application-administrator-permissions"></a>Permisos de administrador de la aplicación en la nube

Puede crear y administrar todos los aspectos de los registros de aplicaciones y de las aplicaciones empresariales, excepto el proxy de aplicación.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/applications/audience/update | Actualiza la propiedad applications.audience en Azure Active Directory. |
| microsoft.directory/applications/authentication/update | Actualiza la propiedad applications.authentication en Azure Active Directory. |
| microsoft.directory/applications/basic/update | Actualiza las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/create | Crea aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/credentials/update | Actualiza la propiedad applications.credentials en Azure Active Directory. |
| microsoft.directory/applications/delete | Elimina aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/owners/update | Actualiza la propiedad applications.owners en Azure Active Directory. |
| microsoft.directory/applications/permissions/update | Actualiza la propiedad applications.permissions en Azure Active Directory. |
| microsoft.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/update | Actualiza el elemento appRoleAssignments en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/delete | Elimina elementos appRoleAssignments en Azure Active Directory. |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/create | Crea directivas en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/basic/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/basic/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/delete | Elimina directivas en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/owners/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/owners/update | Actualiza la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/policies/applicationConfiguration/policyAppliedTo/read | Lee la propiedad policies.applicationConfiguration en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/servicePrincipals/audience/update | Actualiza la propiedad servicePrincipals.audience en Azure Active Directory. |
| microsoft.directory/servicePrincipals/authentication/update | Actualiza la propiedad servicePrincipals.authentication en Azure Active Directory. |
| microsoft.directory/servicePrincipals/basic/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/create | Crea elementos sevicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/credentials/update | Actualiza la propiedad servicePrincipals.credentials en Azure Active Directory. |
| microsoft.directory/servicePrincipals/delete | Elimina servicePrincipals in Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/permissions/update | Actualiza la propiedad servicePrincipals.permissions en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="cloud-device-administrator-permissions"></a>Permisos de administrador del dispositivo en la nube

Acceso total para administrar los dispositivos de Azure AD.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Lee la propiedad devices.bitLockerRecoveryKeys en Azure Active Directory. |
| microsoft.directory/devices/delete | Elimina dispositivos en Azure Active Directory. |
| microsoft.directory/devices/disable | Deshabilita dispositivos en Azure Active Directory. |
| microsoft.directory/devices/enable | Habilita dispositivos en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="company-administrator-permissions"></a>Permisos de administrador de empresa

Puede administrar todos los aspectos de los servicios de Azure AD y Microsoft que usan identidades de Azure AD. Este rol también se conoce como el rol de administrador global. 

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.aad.cloudAppSecurity. |
| microsoft.directory/administrativeUnits/allProperties/allTasks | Crea y elimina elementos administrativeUnits, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/applications/allProperties/allTasks | Crea y elimina aplicaciones, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/allProperties/allTasks | Crea y elimina elementos appRoleAssignments, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/contacts/allProperties/allTasks | Crea y elimina contactos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/contracts/allProperties/allTasks | Crea y elimina contratos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/devices/allProperties/allTasks | Crea y elimina dispositivos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/directoryRoles/allProperties/allTasks | Crea y elimina elementos directoryRoles, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/directoryRoleTemplates/allProperties/allTasks | Crea y elimina elementos directoryRoleTemplates, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/domains/allProperties/allTasks | Crea y elimina dominios, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/groups/allProperties/allTasks | Crea y elimina grupos, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/groupSettings/allProperties/allTasks | Crea y elimina groupSettings, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/groupSettingTemplates/allProperties/allTasks | Crea y elimina groupSettingTemplates, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/loginTenantBranding/allProperties/allTasks | Crea y elimina loginTenantBranding, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Crea y elimina oAuth2PermissionGrants, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/organization/allProperties/allTasks | Crea y elimina el elemento organization, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/policies/allProperties/allTasks | Crea y elimina directivas, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/roleAssignments/allProperties/allTasks | Crea y elimina elementos roleAssignments, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/roleDefinitions/allProperties/allTasks | Crea y elimina elementos RoleDefinitions, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/scopedRoleMemberships/allProperties/allTasks | Crea y elimina elementos scopedRoleMemberships, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/serviceAction/activateService | Puede realizar la acción de servicio Activateservice en Azure Active Directory. |
| microsoft.directory/serviceAction/disableDirectoryFeature | Puede realizar la acción de servicio Disabledirectoryfeature en Azure Active Directory. |
| microsoft.directory/serviceAction/enableDirectoryFeature | Puede realizar la acción de servicio Enabledirectoryfeature en Azure Active Directory. |
| microsoft.directory/serviceAction/getAvailableExtentionProperties | Puede realizar la acción de servicio Getavailableextentionproperties en Azure Active Directory. |
| microsoft.directory/servicePrincipals/allProperties/allTasks | Crea y elimina servicePrincipals, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.directory/subscribedSkus/allProperties/allTasks | Crea y elimina elementos subscribedSku, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directory/users/allProperties/allTasks | Crea y elimina usuarios, y lee y actualiza todas las propiedades en Azure Active Directory. |
| microsoft.directorySync/allEntities/allTasks | Realiza todas las acciones en Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lee todos los recursos de microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Lee todos los recursos en microsoft.azure.advancedThreatProtection. |
| microsoft.azure.informationProtection/allEntities/allTasks | Administra todos los aspectos de Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Administra todos los aspectos de la facturación de Office 365. |
| microsoft.intune/allEntities/allTasks | Administra todos los aspectos de Intune. |
| microsoft.office365.complianceManager/allEntities/allTasks | Administra todos los aspectos del Administrador de cumplimiento de Office 365 |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Administrar todos los aspectos de análisis de escritorio. |
| microsoft.office365.exchange/allEntities/allTasks | Administra todos los aspectos de Exchange Online. |
| microsoft.office365.lockbox/allEntities/allTasks | Administra todos los aspectos de la Caja de seguridad del cliente de Office 365. |
| microsoft.office365.messageCenter/messages/read | Leer mensajes en microsoft.office365.messageCenter. |
| microsoft.office365.messageCenter/securityMessages/read | Leer mensajes de seguridad en microsoft.office365.messageCenter. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Administra todos los aspectos del Centro de protección de Office 365. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.office365.securityComplianceCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.sharepoint/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.office365.sharepoint. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Administra todos los aspectos de Skype Empresarial Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Administra todos los aspectos de Dynamics 365. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Administra todos los aspectos de Power BI. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Lee todos los recursos en microsoft.windows.defenderAdvancedThreatProtection. |

### <a name="compliance-administrator-permissions"></a>Permisos de administrador de cumplimiento

Puede leer y administrar los informes y la configuración de cumplimiento en Azure AD y Office 365.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.complianceManager/allEntities/allTasks | Administra todos los aspectos del Administrador de cumplimiento de Office 365 |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="compliance-data-administrator-permissions"></a>Permisos de administrador de datos de cumplimiento

Crea y administra el contenido de cumplimiento.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Permite leer y configurar Microsoft Cloud App Security. |
| microsoft.azure.informationProtection/allEntities/allTasks | Administra todos los aspectos de Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.complianceManager/allEntities/allTasks | Administra todos los aspectos del Administrador de cumplimiento de Office 365 |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="conditional-access-administrator-permissions"></a>Permisos de administrador de acceso condicional

Puede administrar las funcionalidades de acceso condicional.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/policies/conditionalAccess/basic/read | Lee la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/basic/update | Actualiza la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/create | Crea directivas en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/delete | Elimina directivas en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/owners/read | Lee la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/owners/update | Actualiza la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/policiesAppliedTo/read | Lee la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/tenantDefault/update | Actualiza la propiedad policies.conditionalAccess en Azure Active Directory. |

### <a name="crm-service-administrator-permissions"></a>Permisos de administrador de servicios de CRM

Puede administrar todos los aspectos del producto Dynamics 365.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Administra todos los aspectos de Dynamics 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="customer-lockbox-access-approver-permissions"></a>Permisos de aprobador del acceso a la Caja de seguridad del cliente

Puede aprobar solicitudes de soporte técnico de Microsoft para acceder a datos de la organización del cliente.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.lockbox/allEntities/allTasks | Administra todos los aspectos de la Caja de seguridad del cliente de Office 365. |

### <a name="desktop-analytics-administrator-permissions"></a>Permisos de administrador de análisis de escritorio

Puede administrar los servicios Análisis de escritorio y de personalización y directivas de Office. En el caso de Análisis de escritorio, esto incluye la posibilidad de ver el inventario de recursos, crear planes de implementación y ver la implementación y el estado de mantenimiento. En el caso del servicio de personalización y directivas de Office, este rol permite a los usuarios administrar las directivas de Office.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Administrar todos los aspectos de análisis de escritorio. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="device-administrators-permissions"></a>Permisos de administradores de dispositivos

Los usuarios que se asignan a este rol se agregan al grupo de administradores locales en dispositivos unidos a Azure AD.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/groupSettings/basic/read | Lee las propiedades básicas de groupSettings en Azure Active Directory. |
| microsoft.directory/groupSettingTemplates/basic/read | Lee las propiedades básicas de groupSettingTemplates en Azure Active Directory. |

### <a name="directory-readers-permissions"></a>Permisos de lectores de directorios
Puede leer la información básica del directorio. Para conceder acceso a aplicaciones; no pensado para los usuarios.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/administrativeUnits/basic/read | Lee las propiedades básicas de administrativeUnits en Azure Active Directory. |
| microsoft.directory/administrativeUnits/members/read | Lee la propiedad administrativeUnits.members en Azure Active Directory. |
| microsoft.directory/applications/basic/read | Lee las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/owners/read | Lee la propiedad applications.owners en Azure Active Directory. |
| microsoft.directory/applications/policies/read | Leer la propiedad applications.policies en Azure Active Directory. |
| microsoft.directory/contacts/basic/read | Lee las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.directory/contacts/memberOf/read | Lee la propiedad contacts.memberOf en Azure Active Directory. |
| microsoft.directory/contracts/basic/read | Lee las propiedades básicas en los contratos de Azure Active Directory. |
| microsoft.directory/devices/basic/read | Lee las propiedades básicas en los dispositivos en Azure Active Directory. |
| microsoft.directory/devices/memberOf/read | Lee la propiedad devices.memberOf en Azure Active Directory. |
| microsoft.directory/devices/registeredOwners/read | Lee la propiedad devices.registeredOwners en Azure Active Directory. |
| microsoft.directory/devices/registeredUsers/read | Lee la propiedad devices.registeredUsers en Azure Active Directory. |
| microsoft.directory/directoryRoles/basic/read | Lee las propiedades básicas de directoryRoles en Azure Active Directory. |
| microsoft.directory/directoryRoles/eligibleMembers/read | Lee la propiedad directoryRoles.eligibleMembers en Azure Active Directory. |
| microsoft.directory/directoryRoles/members/read | Lee la propiedad directoryRoles.members en Azure Active Directory. |
| microsoft.directory/domains/basic/read | Lee las propiedades básicas de los dominios de Azure Active Directory. |
| microsoft.directory/groups/appRoleAssignments/read | Lee la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/groups/basic/read | Lee las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.directory/groups/memberOf/read | Lee la propiedad groups.memberOf en Azure Active Directory. |
| microsoft.directory/groups/members/read | Lee la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/read | Lee la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/groups/settings/read | Lee la propiedad groups.settings en Azure Active Directory. |
| microsoft.directory/groupSettings/basic/read | Lee las propiedades básicas de groupSettings en Azure Active Directory. |
| microsoft.directory/groupSettingTemplates/basic/read | Lee las propiedades básicas de groupSettingTemplates en Azure Active Directory. |
| microsoft.directory/oAuth2PermissionGrants/basic/read | Lee las propiedades básicas de oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/organization/basic/read | Lee las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.directory/organization/trustedCAsForPasswordlessAuth/read | Lee organizations.trustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.directory/roleAssignments/basic/read | Leer las propiedades básicas de roleAssignments en Azure Active Directory. |
| microsoft.directory/roleDefinitions/basic/read | Leer las propiedades básicas de roleDefinitions en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/read | Lee la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignments/read | Lee la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/servicePrincipals/basic/read | Lee las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/memberOf/read | Lee la propiedad servicePrincipals.memberOf en Azure Active Directory. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lee la propiedad servicePrincipals.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/servicePrincipals/ownedObjects/read | Lee la propiedad servicePrincipals.ownedObjects en Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/read | Lee la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/read | Lee la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directory/subscribedSkus/basic/read | Lee las propiedades básicas de subscribedSkus en Azure Active Directory. |
| microsoft.directory/users/appRoleAssignments/read | Lee la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/basic/read | Lee las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/directReports/read | Lee la propiedad users.directReports en Azure Active Directory. |
| microsoft.directory/users/manager/read | Lee la propiedad users.manager en Azure Active Directory. |
| microsoft.directory/users/memberOf/read | Lee la propiedad users.memberOf en Azure Active Directory. |
| microsoft.directory/users/oAuth2PermissionGrants/basic/read | Lee la propiedad users.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/users/ownedDevices/read | Lee la propiedad users.ownedDevices en Azure Active Directory. |
| microsoft.directory/users/ownedObjects/read | Lee la propiedad users.ownedObjects en Azure Active Directory. |
| microsoft.directory/users/registeredDevices/read | Lee la propiedad users.registeredDevices en Azure Active Directory. |

### <a name="directory-synchronization-accounts-permissions"></a>Permisos de cuentas de sincronización de directorios

Solo las usa el servicio de Azure AD Connect.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/organization/dirSync/update | Actualiza la propiedad organization.dirSync en Azure Active Directory. |
| microsoft.directory/policies/create | Crea directivas en Azure Active Directory. |
| microsoft.directory/policies/delete | Elimina directivas en Azure Active Directory. |
| microsoft.directory/policies/basic/read | Lee las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.directory/policies/basic/update | Actualiza las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.directory/policies/owners/read | Lee la propiedad policies.owners en Azure Active Directory. |
| microsoft.directory/policies/owners/update | Actualiza la propiedad policies.owners en Azure Active Directory. |
| microsoft.directory/policies/policiesAppliedTo/read | Lee la propiedad policies.policiesAppliedTo en Azure Active Directory. |
| microsoft.directory/policies/tenantDefault/update | Actualiza la propiedad policies.tenantDefault en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/read | Lee la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Actualiza la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignments/read | Lee la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Actualiza la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/servicePrincipals/audience/update | Actualiza la propiedad servicePrincipals.audience en Azure Active Directory. |
| microsoft.directory/servicePrincipals/authentication/update | Actualiza la propiedad servicePrincipals.authentication en Azure Active Directory. |
| microsoft.directory/servicePrincipals/basic/read | Lee las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/basic/update | Actualiza las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/create | Crea elementos sevicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/credentials/update | Actualiza la propiedad servicePrincipals.credentials en Azure Active Directory. |
| microsoft.directory/servicePrincipals/memberOf/read | Lee la propiedad servicePrincipals.memberOf en Azure Active Directory. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lee la propiedad servicePrincipals.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/read | Lee la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/update | Actualiza la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/ownedObjects/read | Lee la propiedad servicePrincipals.ownedObjects en Azure Active Directory. |
| microsoft.directory/servicePrincipals/permissions/update | Actualiza la propiedad servicePrincipals.permissions en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/read | Lee la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directorySync/allEntities/allTasks | Realiza todas las acciones en Azure AD Connect. |

### <a name="directory-writers-permissions"></a>Permisos de escritores de directorios

Puede leer y escribir información básica del directorio. Para conceder acceso a aplicaciones; no pensado para los usuarios.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/groups/basic/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.directory/groupSettings/basic/update | Actualiza las propiedades básicas de groupSettings en Azure Active Directory. |
| microsoft.directory/groupSettings/create | Crea elementos groupSettings en Azure Active Directory. |
| microsoft.directory/groupSettings/delete | Elimina elementos groupSettings en Azure Active Directory. |
| microsoft.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.directory/users/basic/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |

### <a name="exchange-service-administrator-permissions"></a>Permisos de administrador de servicios de Exchange

Puede administrar todos los aspectos del producto Exchange.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.directory/groups/unified/appRoleAssignments/update | Actualiza la propiedad groups.unified en Azure Active Directory. |
| microsoft.directory/groups/unified/basic/update | Actualizar las propiedades básicas de los Grupos de Office 365. |
| microsoft.directory/groups/unified/create | Crear Grupos de Office 365. |
| microsoft.directory/groups/unified/delete | Eliminar Grupos de Office 365. |
| microsoft.directory/groups/unified/members/update | Actualizar pertenencia de Grupos de Office 365. |
| microsoft.directory/groups/unified/owners/update | Actualizar propiedad de Grupos de Office 365. |
| microsoft.office365.exchange/allEntities/allTasks | Administra todos los aspectos de Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |

### <a name="external-identity-provider-administrator-permissions"></a>Permisos de administrador de proveedor de identidades externo

Configure proveedores de identidades para su uso en la federación directa.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.b2c/identityProviders/allTasks | Permite leer y configurar los proveedores de identidades en Azure Active Directory B2C. |

### <a name="global-reader-permissions"></a>Permisos del lector global
Puede leer los mismos elementos que un administrador global, pero no puede editar nada.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la [descripción del rol](#global-reader) anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.commerce.billing/allEntities/read    | Lee todos los aspectos de la facturación de Office 365. |
| microsoft.directory/administrativeUnits/basic/read    | Lee las propiedades básicas de administrativeUnits en Azure Active Directory. |
| microsoft.directory/administrativeUnits/members/read    | Lee la propiedad administrativeUnits.members en Azure Active Directory. |
| microsoft.directory/applications/basic/read    | Lee las propiedades básicas de las aplicaciones en Azure Active Directory. |
| microsoft.directory/applications/owners/read    | Lee la propiedad applications.owners en Azure Active Directory. |
| microsoft.directory/applications/policies/read    | Leer la propiedad applications.policies en Azure Active Directory. |
| microsoft.directory/contacts/basic/read    | Lee las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.directory/contacts/memberOf/read    | Lee la propiedad contacts.memberOf en Azure Active Directory. |
| microsoft.directory/contracts/basic/read    | Lee las propiedades básicas en los contratos de Azure Active Directory. |
| microsoft.directory/devices/basic/read    | Lee las propiedades básicas en los dispositivos en Azure Active Directory. |
| microsoft.directory/devices/memberOf/read    | Lee la propiedad devices.memberOf en Azure Active Directory. |
| microsoft.directory/devices/registeredOwners/read    | Lee la propiedad devices.registeredOwners en Azure Active Directory. |
| microsoft.directory/devices/registeredUsers/read    | Lee la propiedad devices.registeredUsers en Azure Active Directory. |
| microsoft.directory/directoryRoles/basic/read    | Lee las propiedades básicas de directoryRoles en Azure Active Directory. |
| microsoft.directory/directoryRoles/eligibleMembers/read    | Lee la propiedad directoryRoles.eligibleMembers en Azure Active Directory. |
| microsoft.directory/directoryRoles/members/read    | Lee la propiedad directoryRoles.members en Azure Active Directory. |
| microsoft.directory/domains/basic/read    | Lee las propiedades básicas de los dominios de Azure Active Directory. |
| microsoft.directory/groups/appRoleAssignments/read    | Lee la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/groups/basic/read    | Lee las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.directory/groups/hiddenMembers/read    | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.directory/groups/memberOf/read    | Lee la propiedad groups.memberOf en Azure Active Directory. |
| microsoft.directory/groups/members/read    | Lee la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/read    | Lee la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/groups/settings/read    | Lee la propiedad groups.settings en Azure Active Directory. |
| microsoft.directory/groupSettings/basic/read    | Lee las propiedades básicas de groupSettings en Azure Active Directory. |
| microsoft.directory/groupSettingTemplates/basic/read    | Lee las propiedades básicas de groupSettingTemplates en Azure Active Directory. |
| microsoft.directory/oAuth2PermissionGrants/basic/read    | Lee las propiedades básicas de oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/organization/basic/read    | Lee las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.directory/organization/trustedCAsForPasswordlessAuth/read    | Lee organizations.trustedCAsForPasswordlessAuth en Azure Active Directory. |
| microsoft.directory/policies/standard/read    | Lee las directivas estándar en Azure Active Directory. |
| microsoft.directory/roleAssignments/basic/read    | Leer las propiedades básicas de roleAssignments en Azure Active Directory. |
| microsoft.directory/roleDefinitions/basic/read    | Leer las propiedades básicas de roleDefinitions en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/read    | Lee la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/appRoleAssignments/read    | Lee la propiedad servicePrincipals.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/servicePrincipals/basic/read    | Lee las propiedades básicas de servicePrincipals en Azure Active Directory. |
| microsoft.directory/servicePrincipals/memberOf/read    | Lee la propiedad servicePrincipals.memberOf en Azure Active Directory. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/basic/read    | Lee la propiedad servicePrincipals.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/servicePrincipals/ownedObjects/read    | Lee la propiedad servicePrincipals.ownedObjects en Azure Active Directory. |
| microsoft.directory/servicePrincipals/owners/read    | Lee la propiedad servicePrincipals.owners en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/read    | Lee la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read    | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.directory/subscribedSkus/basic/read    | Lee las propiedades básicas de subscribedSkus en Azure Active Directory. |
| microsoft.directory/users/appRoleAssignments/read    | Lee la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/basic/read    | Lee las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/directReports/read    | Lee la propiedad users.directReports en Azure Active Directory. |
| microsoft.directory/users/manager/read    | Lee la propiedad users.manager en Azure Active Directory. |
| microsoft.directory/users/memberOf/read    | Lee la propiedad users.memberOf en Azure Active Directory. |
| microsoft.directory/users/oAuth2PermissionGrants/basic/read    | Lee la propiedad users.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/users/ownedDevices/read    | Lee la propiedad users.ownedDevices en Azure Active Directory. |
| microsoft.directory/users/ownedObjects/read    | Lee la propiedad users.ownedObjects en Azure Active Directory. |
| microsoft.directory/users/registeredDevices/read    | Lee la propiedad users.registeredDevices en Azure Active Directory. |
| microsoft.directory/users/strongAuthentication/read    | Lee las propiedades de autenticación sólida, como la información de las credenciales de MFA. |
| microsoft.office365.exchange/allEntities/read    | Lee todos los aspectos de Exchange Online. |
| microsoft.office365.messageCenter/messages/read    | Leer mensajes en microsoft.office365.messageCenter. |
| microsoft.office365.messageCenter/securityMessages/read    | Leer mensajes de seguridad en microsoft.office365.messageCenter. |
| microsoft.office365.protectionCenter/allEntities/read    | Lee todos los aspectos del Centro de protección de Office 365. |
| microsoft.office365.securityComplianceCenter/allEntities/read    | Lee todas las propiedades estándar de microsoft.office365.securityComplianceCenter. |
| microsoft.office365.usageReports/allEntities/read    | Lee los informes de uso de Office 365. |
| microsoft.office365.webPortal/allEntities/standard/read    | Lee las propiedades estándar de todos los recursos de microsoft.office365.webPortal. |

### <a name="groups-administrator-permissions"></a>Permisos de administrador de grupos
Puede administrar todos los aspectos de los grupos y las configuraciones de grupos, como las directivas de nomenclatura y expiración.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/groups/basic/read | Lee las propiedades estándar del elemento Groups en Azure Active Directory.  |
| microsoft.directory/groups/basic/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.directory/groups/hiddenMembers/read | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.messageCenter/messages/read | Leer mensajes en microsoft.office365.messageCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="guest-inviter-permissions"></a>Permisos de invitador de usuarios invitados
Puede invitar a usuarios independientemente de la configuración "Members can invite guests" (Los miembros pueden invitar a usuarios).

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/users/appRoleAssignments/read | Lee la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/basic/read | Lee las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/directReports/read | Lee la propiedad users.directReports en Azure Active Directory. |
| microsoft.directory/users/inviteGuest | Invita usuarios en Azure Active Directory. |
| microsoft.directory/users/manager/read | Lee la propiedad users.manager en Azure Active Directory. |
| microsoft.directory/users/memberOf/read | Lee la propiedad users.memberOf en Azure Active Directory. |
| microsoft.directory/users/oAuth2PermissionGrants/basic/read | Lee la propiedad users.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/users/ownedDevices/read | Lee la propiedad users.ownedDevices en Azure Active Directory. |
| microsoft.directory/users/ownedObjects/read | Lee la propiedad users.ownedObjects en Azure Active Directory. |
| microsoft.directory/users/registeredDevices/read | Lee la propiedad users.registeredDevices en Azure Active Directory. |

### <a name="helpdesk-administrator-permissions"></a>Permisos de administrador del departamento de soporte técnico

Puede restablecer contraseñas de usuarios que no son administradores y de administradores del departamento de soporte técnico.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Lee la propiedad devices.bitLockerRecoveryKeys en Azure Active Directory. |
| microsoft.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="intune-service-administrator-permissions"></a>Permisos de administrador de servicios de Intune

Puede administrar todos los aspectos del producto Intune.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/contacts/basic/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.directory/devices/basic/update | Actualiza las propiedades básicas en los dispositivos de Azure Active Directory. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Lee la propiedad devices.bitLockerRecoveryKeys en Azure Active Directory. |
| microsoft.directory/devices/create | Crea dispositivos en Azure Active Directory. |
| microsoft.directory/devices/delete | Elimina dispositivos en Azure Active Directory. |
| microsoft.directory/devices/registeredOwners/update | Actualiza la propiedad devices.registeredOwners en Azure Active Directory. |
| microsoft.directory/devices/registeredUsers/update | Actualiza la propiedad devices.registeredUsers en Azure Active Directory. |
| microsoft.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/groups/basic/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.directory/groups/hiddenMembers/read | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/basic/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.intune/allEntities/allTasks | Administra todos los aspectos de Intune. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |

### <a name="kaizala-administrator-permissions"></a>Permisos de administrador de Kaizala

Puede administrar la configuración de Microsoft Kaizala.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee en el centro de administración de Office 365. |

### <a name="license-administrator-permissions"></a>Permisos de administrador de licencias

Puede administrar licencias de producto de usuarios y grupos.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.directory/users/usageLocation/update | Actualizar la propiedad users.UsageLocation en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="lync-service-administrator-permissions"></a>Permisos de administrador de servicios de Lync

Puede administrar todos los aspectos del producto Skype Empresarial.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Administra todos los aspectos de Skype Empresarial Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read    | Lee los informes de uso de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |


### <a name="message-center-privacy-reader-permissions"></a>Permisos de lector de privacidad del Centro de mensajes

Puede leer entradas del Centro de mensajes, mensajes de privacidad de datos, grupos, dominios y suscripciones.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.messageCenter/messages/read | Leer mensajes en microsoft.office365.messageCenter. |
| microsoft.office365.messageCenter/securityMessages/read | Leer mensajes de seguridad en microsoft.office365.messageCenter. |

### <a name="message-center-reader-permissions"></a>Permisos de lector del Centro de mensajes
Puede leer los mensajes y las actualizaciones para su organización solo en el Centro de mensajes de Office 365. 

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.messageCenter/messages/read | Leer mensajes en microsoft.office365.messageCenter. |

### <a name="office-apps-administrator-permissions"></a>Permisos de administrador de aplicaciones de Office
Puede administrar los servicios en la nube de las aplicaciones de Office, incluida la administración de directivas y de la configuración. También puede administrar la posibilidad de seleccionar y publicar contenido relativo a la característica "novedades", además de anular la selección de dicho contenido, para los dispositivos del usuario final.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.messageCenter/messages/read | Leer mensajes en microsoft.office365.messageCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.userCommunication/allEntities/allTasks | Lea y actualice la visibilidad de los mensajes sobre novedades. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |

### <a name="partner-tier1-support-permissions"></a>Permisos de soporte técnico de nivel 1 para asociados

No lo use. No está pensado para el uso general.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/contacts/basic/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.directory/users/basic/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/delete | Elimina usuarios en Azure Active Directory. |
| microsoft.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.directory/users/restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="partner-tier2-support-permissions"></a>Permisos de soporte técnico de nivel 2 para asociados

No lo use. No está pensado para el uso general.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/contacts/basic/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.directory/domains/allTasks | Crea y elimina dominios, y lee y actualiza las propiedades estándar en Azure Active Directory. |
| microsoft.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.directory/organization/basic/update | Actualiza las propiedades básicas de la organización en Azure Active Directory. |
| microsoft.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.directory/users/basic/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/delete | Elimina usuarios en Azure Active Directory. |
| microsoft.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.directory/users/restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="password-administrator-permissions"></a>Permisos de administrador de contraseñas

Puede restablecer contraseñas para usuarios que no son administradores y para administradores de contraseñas.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |

### <a name="power-bi-service-administrator-permissions"></a>Permisos de administrador de servicios de Power BI

Puede administrar todos los aspectos del producto Power BI.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>
| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Administra todos los aspectos de Power BI. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |


### <a name="power-platform-administrator-permissions"></a>Permisos de administrador de Power Platform

Puede crear y administrar todos los aspectos de Microsoft Dynamics 365, PowerApps y Microsoft Flow. 

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>
| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.dynamics365/allEntities/allTasks | Administra todos los aspectos de Dynamics 365. |
| microsoft.flow/allEntities/allTasks | Administra todos los aspectos de Microsoft Flow. |
| microsoft.powerApps/allEntities/allTasks | Administra todos los aspectos de PowerApps. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="privileged-authentication-administrator-permissions"></a>Permisos de administrador de autenticación con privilegios

Puede ver, configurar y restablecer la información de los métodos de autenticación de cualquier usuario (administrador o no).

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.directory/users/strongAuthentication/update | Actualizar las propiedades de autenticación sólida, como la información de las credenciales de MFA. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.directory/users/password/update | Actualice las contraseñas de todos los usuarios de la organización de Office 365. Para obtener más información, consulte la documentación en línea. |

### <a name="privileged-role-administrator-permissions"></a>Permisos de administrador de roles con privilegios

Puede administrar las asignaciones de roles en Azure AD y todos los aspectos de Privileged Identity Management.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.aad.privilegedIdentityManagement. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/allTasks | Lee y configura la propiedad servicePrincipals.appRoleAssignedTo en Azure Active Directory. |
| microsoft.directory/servicePrincipals/oAuth2PermissionGrants/allTasks | Lee y configura la propiedad servicePrincipals.oAuth2PermissionGrants en Azure Active Directory. |
| microsoft.directory/administrativeUnits/allProperties/allTasks | Crea y administra unidades administrativas (incluidos los miembros). |
| microsoft.directory/roleAssignments/allProperties/allTasks | Crea y administra las asignaciones de roles. |
| microsoft.directory/roleDefinitions/allProperties/allTasks | Crea y administra las definiciones de roles. |

### <a name="reports-reader-permissions"></a>Permisos del lector de informes

Puede leer los informes de inicio de sesión y auditoría.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |

### <a name="search-administrator-permissions"></a>Permisos de administrador de búsqueda

Puede crear y administrar todos los aspectos de la configuración de Búsqueda de Microsoft.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Leer mensajes en microsoft.office365.messageCenter. |
| microsoft.office365.search/allEntities/allProperties/allTasks | Crea y elimina todos los recursos, y lee y actualiza todas las propiedades en microsoft.office365.search. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |

### <a name="search-editor-permissions"></a>Permisos de editor de búsqueda

Puede crear y administrar contenido editorial como marcadores, preguntas y respuestas, ubicaciones y plano de planta.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Leer mensajes en microsoft.office365.messageCenter. |
| microsoft.office365.search/content/allProperties/allTasks | Crea y elimina el contenido, y lee y actualiza todas las propiedades en microsoft.office365.search. |

### <a name="security-administrator-permissions"></a>Permisos de administrador de seguridad

Puede leer la información y los informes de seguridad, así como administrar la configuración, en Azure AD y Office 365.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/applications/policies/update | Actualiza la propiedad applications.policies en Azure Active Directory. |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Lee la propiedad devices.bitLockerRecoveryKeys en Azure Active Directory. |
| microsoft.directory/policies/basic/update | Actualiza las propiedades básicas en las directivas de Azure Active Directory. |
| microsoft.directory/policies/create | Crea directivas en Azure Active Directory. |
| microsoft.directory/policies/delete | Elimina directivas en Azure Active Directory. |
| microsoft.directory/policies/owners/update | Actualiza la propiedad policies.owners en Azure Active Directory. |
| microsoft.directory/policies/tenantDefault/update | Actualiza la propiedad policies.tenantDefault en Azure Active Directory. |
| microsoft.directory/servicePrincipals/policies/update | Actualiza la propiedad servicePrincipals.policies en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Lee todos los recursos en microsoft.aad.identityProtection. |
| microsoft.aad.identityProtection/allEntities/update | Actualiza todos los recursos en microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lee todos los recursos de microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.protectionCenter/allEntities/read | Lee todos los aspectos del Centro de protección de Office 365. |
| microsoft.office365.protectionCenter/allEntities/update | Actualiza todos los recursos en microsoft.office365.protectionCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="security-operator-permissions"></a>Permisos de operador de seguridad

Crea y administra los eventos de seguridad.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Permite leer y configurar Microsoft Cloud App Security. |
| microsoft.aad.identityProtection/allEntities/read | Lee todos los recursos en microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lee todos los recursos de microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Lee y configura Azure AD Advanced Threat Protection. |
| microsoft.intune/allEntities/allTasks | Administra todos los aspectos de Intune. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Lee y configura el Centro de seguridad y cumplimiento. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Lee y configura Advanced Threat Protection para Windows Defender. |

### <a name="security-reader-permissions"></a>Permisos de lector de seguridad

Puede leer la información y los informes de seguridad de Azure AD y Office 365.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en auditLogs en Azure Active Directory. |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Lee la propiedad devices.bitLockerRecoveryKeys en Azure Active Directory. |
| microsoft.directory/policies/conditionalAccess/basic/read | Lee la propiedad policies.conditionalAccess en Azure Active Directory. |
| microsoft.directory/signInReports/allProperties/read | Lee todas las propiedades (incluidas las propiedades con privilegios) en signInReports en Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Lee todos los recursos en microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lee todos los recursos de microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.protectionCenter/allEntities/read | Lee todos los aspectos del Centro de protección de Office 365. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="service-support-administrator-permissions"></a>Permisos de administrador de soporte técnico de servicios

Puede leer la información de estado del servicio y administrar las incidencias de soporte técnico.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

### <a name="sharepoint-service-administrator-permissions"></a>Permisos de administrador de servicios de SharePoint

Puede administrar todos los aspectos del servicio SharePoint.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.directory/groups/unified/appRoleAssignments/update | Actualiza la propiedad groups.unified en Azure Active Directory. |
| microsoft.directory/groups/unified/basic/update | Actualizar las propiedades básicas de los Grupos de Office 365. |
| microsoft.directory/groups/unified/create | Crear Grupos de Office 365. |
| microsoft.directory/groups/unified/delete | Eliminar Grupos de Office 365. |
| microsoft.directory/groups/unified/members/update | Actualizar pertenencia de Grupos de Office 365. |
| microsoft.directory/groups/unified/owners/update | Actualizar propiedad de Grupos de Office 365. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.sharepoint/allEntities/allTasks | Crea y elimina todos los recursos, y lee y actualiza las propiedades estándar en microsoft.office365.sharepoint. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read    | Lee los informes de uso de Office 365. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |

### <a name="teams-communications-administrator-permissions"></a>Permisos de administrador de comunicaciones de Teams

Puede administrar las características de llamadas y reuniones del servicio Microsoft Teams.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |

### <a name="teams-communications-support-engineer-permissions"></a>Permisos de ingeniero de soporte técnico de comunicaciones de Teams

Puede solucionar los problemas de comunicaciones dentro de Teams mediante herramientas avanzadas.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="teams-communications-support-specialist-permissions"></a>Permisos de especialista de soporte técnico de comunicaciones de Teams

Puede solucionar los problemas de comunicaciones dentro de Teams mediante herramientas básicas.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |

### <a name="teams-service-administrator-permissions"></a>Permisos de administrador de servicios de Teams

Puede administrar el servicio Microsoft Teams.

> [!NOTE]
> Este rol tiene permisos adicionales fuera de Azure Active Directory. Para más información, vea la descripción del rol anterior.
>
>

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/groups/hiddenMembers/read | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.directory/groups/unified/appRoleAssignments/update | Actualiza la propiedad groups.unified en Azure Active Directory. |
| microsoft.directory/groups/unified/basic/update | Actualizar las propiedades básicas de los Grupos de Office 365. |
| microsoft.directory/groups/unified/create | Crear Grupos de Office 365. |
| microsoft.directory/groups/unified/delete | Eliminar Grupos de Office 365. |
| microsoft.directory/groups/unified/members/update | Actualizar pertenencia de Grupos de Office 365. |
| microsoft.directory/groups/unified/owners/update | Actualizar propiedad de Grupos de Office 365. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |
| microsoft.office365.usageReports/allEntities/read | Lee los informes de uso de Office 365. |

### <a name="user-administrator-permissions"></a>Permisos de administrador de usuarios
Puede administrar todos los aspectos de usuarios y grupos, incluido el restablecimiento de contraseñas para administradores limitados.

| **Acciones** | **Descripción** |
| --- | --- |
| microsoft.directory/appRoleAssignments/create | Crea el elemento AppRoleAssignments en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/delete | Elimina elementos appRoleAssignments en Azure Active Directory. |
| microsoft.directory/appRoleAssignments/update | Actualiza el elemento appRoleAssignments en Azure Active Directory. |
| microsoft.directory/contacts/basic/update | Actualiza las propiedades básicas de los contactos en Azure Active Directory. |
| microsoft.directory/contacts/create | Crea contactos en Azure Active Directory. |
| microsoft.directory/contacts/delete | Elimina los contactos en Azure Active Directory. |
| microsoft.directory/groups/appRoleAssignments/update | Actualiza la propiedad groups.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/groups/basic/update | Actualiza las propiedades básicas de los grupos en Azure Active Directory. |
| microsoft.directory/groups/create | Crea grupos en Azure Active Directory. |
| microsoft.directory/groups/createAsOwner | Crea grupos en Azure Active Directory. El creador se agrega como primer propietario y el objeto creado cuenta en la cuota de 250 objetos que crea. |
| microsoft.directory/groups/delete | Elimina grupos en Azure Active Directory. |
| microsoft.directory/groups/hiddenMembers/read | Lee la propiedad groups.hiddenMembers en Azure Active Directory. |
| microsoft.directory/groups/members/update | Actualiza la propiedad groups.members en Azure Active Directory. |
| microsoft.directory/groups/owners/update | Actualiza la propiedad groups.owners en Azure Active Directory. |
| microsoft.directory/groups/restore | Restaura grupos en Azure Active Directory. |
| microsoft.directory/groups/settings/update | Actualiza la propiedad groups.settings en Azure Active Directory. |
| microsoft.directory/users/appRoleAssignments/update | Actualiza la propiedad users.appRoleAssignments en Azure Active Directory. |
| microsoft.directory/users/assignLicense | Administrar licencias de usuarios de Azure Active Directory. |
| microsoft.directory/users/basic/update | Actualiza las propiedades básicas de los usuarios en Azure Active Directory. |
| microsoft.directory/users/create | Crea usuarios en Azure Active Directory. |
| microsoft.directory/users/delete | Elimina usuarios en Azure Active Directory. |
| microsoft.directory/users/invalidateAllRefreshTokens | Invalida todos los tokens de actualización de usuario en Azure Active Directory. |
| microsoft.directory/users/manager/update | Actualiza la propiedad users.manager en Azure Active Directory. |
| microsoft.directory/users/password/update | Actualiza las contraseñas de todos los usuarios en Azure Active Directory. Para obtener más información, consulte la documentación en línea. |
| microsoft.directory/users/restore | Restaura usuarios eliminados en Azure Active Directory. |
| microsoft.directory/users/userPrincipalName/update | Actualiza la propiedad users.userPrincipalName en Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Leer y configurar Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Azure. |
| microsoft.office365.webPortal/allEntities/basic/read | Lee las propiedades básicas de todos los recursos en microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lee y configura el estado de mantenimiento del servicio Office 365. |
| microsoft.office365.supportTickets/allEntities/allTasks | Crea y administra incidencias de soporte técnico de Office 365. |

## <a name="role-template-ids"></a>Identificadores de plantilla de rol

Los identificadores de plantilla de rol los usan principalmente los usuarios de Microsoft Graph API o PowerShell.

Nombre para mostrar de Graph | Nombre para mostrar de Azure Portal | directoryRoleTemplateId
----------------- | ------------------------- | -------------------------
Administrador de aplicaciones | Administrador de aplicaciones | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Desarrollador de aplicaciones | Desarrollador de aplicaciones | CF1C38E5-3621-4004-A7CB-879624DCED7C
Administrador de autenticación | Administrador de autenticación | c4e39bd9-1100-46d3-8c65-fb160da0071f
Administrador de Azure DevOps | Administrador de Azure DevOps | e3973bdf-4987-49ae-837a-ba8e231c7286
Administrador de Azure Information Protection | Administrador de Azure Information Protection | 7495fdc4-34c4-4d15-a289-98788ce399fd
Administrador de flujos de usuario B2C | Administrador de flujos de usuario B2C | 6e591065-9bad-43ed-90f3-e9424366d2f0
Administrador de atributos de flujos de usuario B2C | Administrador de atributos de flujos de usuario B2C | 0f971eea-41eb-4569-a71e-57bb8a3eff1e
Administrador de conjuntos de claves B2C con IEF | Administrador de conjuntos de claves B2C con IEF | aaf43236-0c0d-4d5f-883a-6955382ac081
Administrador de directivas B2C con IEF | Administrador de directivas B2C con IEF | 3edaf663-341e-4475-9f94-5c398ef6c070
Administrador de facturación | Administrador de facturación | b0f54661-2d74-4c50-afa3-1ec803f12efe
Administrador de aplicaciones en la nube | Administrador de aplicaciones en la nube | 158c047a-c907-4556-b7ef-446551a6b5f7
Administrador de dispositivos en la nube | Administrador de dispositivos en la nube | 7698a772-787b-4ac8-901f-60d6b08affd2
Administrador de la compañía | Administrador global | 62e90394-69f5-4237-9190-012177145e10
Administrador de cumplimiento | Administrador de cumplimiento | 17315797-102d-40b4-93e0-432062caca18
Administrador de datos de cumplimiento | Administrador de datos de cumplimiento | e6d1a23a-da11-4be4-9570-befc86d067a7
Administrador de acceso condicional | Administrador de acceso condicional | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
Administrador de servicios de CRM | Administrador de Dynamics 365 | 44367163-eba1-44c3-98af-f5787879f96a
Aprobador del acceso a la Caja de seguridad del cliente | Aprobador del acceso a la Caja de seguridad del cliente | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
Administrador de análisis de escritorio | Administrador de análisis de escritorio | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
Administradores de dispositivos | Administradores de dispositivo | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Combinación de dispositivos | Combinación de dispositivos | 9c094953-4995-41c8-84c8-3ebb9b32c93f
Administradores de dispositivos | Administradores de dispositivos | 2b499bcd-da44-4968-8aec-78e1674fa64d
Usuarios de dispositivos | Usuarios de dispositivos | d405c6df-0af8-4e3b-95e4-4d06e542189e
Lectores de directorios | Lectores de directorios | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Cuentas de sincronización de directorios | Cuentas de sincronización de directorios | d29b2b05-8046-44ba-8758-1e26182fcf32
Escritores de directorios | Escritores de directorios | 9360feb5-f418-4baa-8175-e2a00bac4301
Administrador de servicios de Exchange | Administrador de Exchange | 29232cdf-9323-42fd-ade2-1d097af3e4de
Administrador de proveedor de identidades externo | Administrador de proveedor de identidades externo | be2f45a1-457d-42af-a067-6ec1fa63bc45
Lector global | Lector global | f2ef992c-3afb-46b9-b7cf-a126ee74c451
Administrador de grupos | Administrador de grupos | fdd7a751-b60b-444a-984c-02652fe8fa1c 
Invitador de usuarios | Invitador de usuarios | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Administrador del departamento de soporte técnico | Administrador del departamento de soporte técnico | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Administrador de servicios de Intune | Administrador de Intune | 3a2c62db-5318-420d-8d74-23affee5d9d5
Administrador de Kaizala | Administrador de Kaizala | 74ef975b-6605-40af-a5d2-b9539d836353
Administrador de licencias | Administrador de licencias | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Administrador de servicios de Lync | Administrador de Skype Empresarial | 75941009-915a-4869-abe7-691bff18279e
Lector de privacidad del Centro de mensajes | Lector de privacidad del Centro de mensajes | ac16e43d-7b2d-40e0-ac05-243ff356ab5b
Lector del Centro de mensajes | Lector del centro de mensajes | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Administrador de aplicaciones de Office | Administrador de aplicaciones de Office | 2b745bdf-0803-4d80-aa65-822c4493daac
Soporte para asociados de nivel 1 | Soporte para asociados de nivel 1 | 4ba39ca4-527c-499a-b93d-d9b492c50246
Soporte para asociados de nivel 2 | Soporte para asociados de nivel 2 | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Administrador de contraseñas | Administrador de contraseñas | 966707d0-3269-4727-9be2-8c3a10f19b9d
Administrador de servicios de Power BI | Administrador de Power BI | a9ea8996-122f-4c74-9520-8edcd192826c
Administrador de Power Platform | Administrador de Power Platform | 11648597-926c-4cf3-9c36-bcebb0ba8dcc
Administrador de autenticación con privilegios | Administrador de autenticación con privilegios | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Administrador de roles con privilegios | Administrador de roles con privilegios | e8611ab8-c189-46e8-94e1-60213ab1f814
Lector de informes | Lector de informes | 4a5d8f65-41da-4de4-8968-e035b65339cf
Administrador de búsqueda | Administrador de búsqueda | 0964bb5e-9bdb-4d7b-ac29-58e794862a40
Editor de búsqueda | Editor de búsqueda | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9
Administrador de seguridad | Administrador de seguridad | 194ae4cb-b126-40b2-bd5b-6091b380977d
Operador de seguridad | Operador de seguridad | 5f2222b1-57c3-48ba-8ad5-d4759f1fde6f
Lector de seguridad | Lector de seguridad | 5d6b6bb7-de71-4623-b4af-96380a352509
Administrador del soporte técnico del servicio | Administrador del soporte técnico del servicio | f023fd81-a637-4b56-95fd-791ac0226033
Administrador de servicios de SharePoint | Administrador de SharePoint | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Administrador de comunicaciones de Teams | Administrador de comunicaciones de Teams | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Ingeniero de soporte técnico de comunicaciones de Teams | Ingeniero de soporte técnico de comunicaciones de Teams | f70938a0-fc10-4177-9e90-2178f8765737
Especialista de soporte técnico de comunicaciones de Teams | Especialista de soporte técnico de comunicaciones de Teams | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Administrador de servicios de Teams | Administrador de servicios de Teams | 69091246-20e8-4a56-aa4d-066075b2a7a8
Usuario | Usuario | a0b1b346-4d3e-4e8b-98f8-753987be4970
Administrador de cuenta de usuario | Administrador de usuarios | fe930be7-5e62-47db-91af-98c3a49a38b1
Combinación de dispositivos de área de trabajo | Combinación de dispositivos de área de trabajo | c34f683f-4d5a-4403-affd-6615e00e3a7f

## <a name="deprecated-roles"></a>Roles en desuso

Los siguientes roles no deben usarse. Están en desuso y se eliminarán de Azure AD más adelante.

* Administrador de licencias ad hoc
* Combinación de dispositivos
* Administradores de dispositivos
* Usuarios de dispositivos
* Creador de usuarios comprobados de correo electrónico
* Administrador de buzones de correo
* Combinación de dispositivos de área de trabajo

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de cómo asignar un usuario como administrador de una suscripción de Azure, consulte [Administración del acceso mediante RBAC y Azure Portal](../../role-based-access-control/role-assignments-portal.md)
* Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para más información acerca de cómo se relaciona Azure Active Directory con la suscripción de Azure, consulte [Asociación de las suscripciones de Azure con Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
