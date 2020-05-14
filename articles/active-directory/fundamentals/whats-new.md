---
title: Novedades Notas de la versión de Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de las novedades de Azure Active Directory, como, por ejemplo, las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: active-directory
author: msmimart
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c58c99faa83d6f04ac37d03789202e111f1b06d
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611167"
---
# <a name="whats-new-in-azure-active-directory"></a>¿Cuáles son las novedades de Azure Active Directory?

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue la dirección URL `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` en el ![icono del lector de fuentes icono RSS](./media/whats-new/feed-icon-16x16.png) lector de fuentes.

En Azure AD se realizan mejoras de forma continua. Para mantenerse al día de los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas
- Planes de cambios

Esta página se actualiza mensualmente, por lo que se recomienda visitarla con frecuencia. Si busca elementos que tengan más de 6 meses, puede encontrarlos en el [Archivo de novedades de Azure Active Directory](whats-new-archive.md).

---

## <a name="april-2020"></a>Abril de 2020

### <a name="combined-security-info-registration-experience-is-now-generally-available"></a>La experiencia combinada del registro de la información de seguridad ya está disponible con carácter general.

**Tipo:** Nueva característica

**Categoría del servicio:** Autenticaciones (inicios de sesión)

**Funcionalidad del producto:** Seguridad y protección de la identidad

La experiencia de registro combinada con autenticación multifactor (MFA) y autoservicio de restablecimiento de contraseña (SSPR) ahora están disponibles con carácter general. Esta nueva experiencia de registro permite a los usuarios registrarse en MFA y SSPR en un único proceso paso a paso. Al implementar la nueva experiencia para la organización, los usuarios pueden registrarse en menos tiempo y con menos complicaciones. Consulte la entrada de blog [aquí](https://bit.ly/3etiRyQ).

---

### <a name="continuous-access-evaluation"></a>Evaluación continua de acceso

**Tipo:** Nueva característica

**Categoría del servicio:** Autenticaciones (inicios de sesión)

**Funcionalidad del producto:** Seguridad y protección de la identidad

Evaluación continua de acceso es una nueva característica de seguridad que permite el cumplimiento casi en tiempo real de las directivas en los usuarios de confianza que consumen tokens de acceso de Azure AD cuando se producen eventos en Azure AD (por ejemplo, la eliminación de cuentas de usuario). Esta característica la vamos a implementar en primer lugar para clientes de Teams y Outlook. Para más información, consulte el [blog](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933) y la [documentación](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-continuous-access-evaluation).

---

### <a name="sms-sign-in-firstline-workers-can-sign-in-to-azure-ad-backed-applications-with-their-phone-number-and-no-password"></a>Inicio de sesión por SMS: Los trabajadores de primera línea pueden iniciar sesión en las aplicaciones respaldadas por Azure AD con su número de teléfono y sin contraseña.

**Tipo:** Nueva característica

**Categoría del servicio:** Autenticaciones (inicios de sesión)

**Funcionalidad del producto:** Autenticación de usuarios

Office está lanzando una serie de aplicaciones empresariales orientadas a móviles dirigidas a organizaciones no tradicionales y a empleados de grandes organizaciones que no usan el correo electrónico como método de comunicación principal. Estas aplicaciones se dirigen a empleados de primera línea, trabajadores sin escritorio, agentes de campo o empleados de comercios minoristas que podrían no recibir una dirección de correo electrónico de su empleador, tener acceso a un equipo o a recursos de TI. Este proyecto permitirá a estos empleados iniciar sesión en las aplicaciones empresariales solo con un número de teléfono y la devolución de un código. Para más información, consulte la [documentación del administrador](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-sms-signin) y la [documentación del usuario final](https://docs.microsoft.com/azure/active-directory/user-help/sms-sign-in-explainer).

---

### <a name="invite-internal-users-to-use-b2b-collaboration"></a>Invitar a usuarios internos a usar la colaboración B2B

**Tipo:** Nueva característica

**Categoría del servicio:** B2B

**Funcionalidad del producto:**

Vamos a expandir la funcionalidad de invitación B2B para que se pueda invitar a las cuentas internas existentes a usar las credenciales de colaboración B2B en adelante. Esto se consigue pasando el objeto de usuario a la API de invitación, además de los parámetros típicos, como la dirección de correo electrónico invitada. El identificador de objeto del usuario, su UPN, la pertenencia a grupos, la asignación de aplicaciones, entre otros, permanecen intactos, pero en adelante usarán B2B para autenticarse con las credenciales de su inquilino particular en lugar de con las credenciales internas que usaban antes de la invitación. Para más información, consulte la [documentación](https://docs.microsoft.com/azure/active-directory/b2b/invite-internal-users).

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>El modo Solo informe para el Acceso condicional ya está disponible con carácter general.

**Tipo:** Nueva característica

**Categoría del servicio:** Acceso condicional

**Funcionalidad del producto:** Seguridad y protección de la identidad

El [modo Solo informe para el Acceso condicional de Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-report-only) le permite evaluar el resultado de una directiva sin necesidad de aplicar controles de acceso. Puede probar las directivas del modo Solo informe en toda la organización y comprender su impacto antes de habilitarlas, lo que hará que la implementación sea más segura y fácil. Durante los últimos meses, hemos visto una importante adopción del modo Solo informe, con más de 26 millones de usuarios. Con este anuncio, se crearán nuevas directivas de acceso condicional de Azure AD en modo Solo informe de forma predeterminada. Esto significa que puede supervisar el impacto de las directivas desde el momento en el que se crean. Además, para aquellos usuarios que utilizan Microsoft Graph API, también puede [administrar directivas de Solo informe mediante programación](https://docs.microsoft.com/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta). 

---

### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>El libro Conclusiones e informes de Acceso condicional está disponible con carácter general.

**Tipo:** Nueva característica

**Categoría del servicio:** Acceso condicional

**Funcionalidad del producto:** Seguridad y protección de la identidad

El [libro Conclusiones e informes](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-insights-reporting) de Acceso condicional proporciona a los administradores una vista resumida del acceso condicional de Azure AD en el inquilino. Con la funcionalidad de seleccionar una directiva individual, los administradores pueden comprender mejor lo que hace cada directiva y supervisar los cambios en tiempo real. El libro transmite los datos almacenados en Azure Monitor, que puede configurar en unos minutos [según estas instrucciones](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics). Para encontrar el panel con más facilidad, lo hemos trasladado a la nueva pestaña Conclusiones e informes del menú Acceso condicional de Azure AD.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>La hoja Detalles de la directiva de Acceso condicional está en versión preliminar pública.

**Tipo:** Nueva característica

**Categoría del servicio:** Acceso condicional

**Funcionalidad del producto:** Seguridad y protección de la identidad

La nueva [hoja Detalles de la directiva](https://docs.microsoft.com/azure/active-directory/conditional-access/troubleshoot-conditional-access) muestra qué asignaciones, condiciones y controles se cumplieron durante la evaluación de la directiva de acceso condicional. Para acceder a la hoja, seleccione una fila en las pestañas **Acceso condicional** o **Solo informe** de los detalles de inicio de sesión.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (abril de 2020)

**Tipo:** Nueva característica

**Categoría del servicio:** Aplicaciones empresariales

**Funcionalidad del producto:** Integración de terceros

En abril de 2020, hemos agregado a la galería de aplicaciones estas 31 nuevas aplicaciones que admiten federación: 

[SincroPool Apps](https://www.sincropool.com/), [SmartDB](https://hibiki.dreamarts.co.jp/smartdb/trial/), [Float](https://docs.microsoft.com/azure/active-directory/saas-apps/float-tutorial), [LMS365](https://lms.365.systems/), [IWT Procurement Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/iwt-procurement-suite-tutorial), [Lunni](https://lunni.fi/), [EasySSO for Jira](https://docs.microsoft.com/azure/active-directory/saas-apps/easysso-for-jira-tutorial), [Virtual Training Academy](https://vta.c3p.ca/app/en/openid?authenticate_with=microsoft), [Meraki Dashboard](https://docs.microsoft.com/azure/active-directory/saas-apps/meraki-dashboard-tutorial), [Office 365 Mover](https://app.mover.io/login), [Speaker Engage](https://speakerengage.com/login.php), [Honestly](https://docs.microsoft.com/azure/active-directory/saas-apps/honestly-tutorial), [Ally](https://docs.microsoft.com/azure/active-directory/saas-apps/ally-tutorial), [DutyFlow](https://app.dutyflow.nl/), [AlertMedia](https://docs.microsoft.com/azure/active-directory/saas-apps/alertmedia-tutorial), [gr8 People](https://docs.microsoft.com/azure/active-directory/saas-apps/gr8-people-tutorial), [Pendo](https://docs.microsoft.com/azure/active-directory/saas-apps/pendo-tutorial), [HighGround](https://docs.microsoft.com/azure/active-directory/saas-apps/highground-tutorial), [Harmony](https://docs.microsoft.com/azure/active-directory/saas-apps/harmony-tutorial), [Timetabling Solutions](https://docs.microsoft.com/azure/active-directory/saas-apps/timetabling-solutions-tutorial), [SynchroNet CLICK](https://docs.microsoft.com/azure/active-directory/saas-apps/synchronet-click-tutorial), [empower](https://www.made-in-office.com/en/), [Fortes Change Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/fortes-change-cloud-tutorial), [Litmus](https://docs.microsoft.com/azure/active-directory/saas-apps/litmus-tutorial), [GroupTalk](https://recorder.grouptalk.com/), [Frontify](https://docs.microsoft.com/azure/active-directory/saas-apps/frontify-tutorial), [MongoDB Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/mongodb-cloud-tutorial), [TickitLMS Learn](https://docs.microsoft.com/azure/active-directory/saas-apps/tickitlms-learn-tutorial), [COCO](https://hexaware.com/partnerships-and-alliances/digital-transformation-using-microsoft-azure/), [Nitro Productivity Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/nitro-productivity-suite-tutorial), [Trend Micro Web Security(TMWS)](https://review.docs.microsoft.com/azure/active-directory/saas-apps/trend-micro-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="microsoft-graph-delta-query-support-for-oauth2permissiongrant-available-for-public-preview"></a>La compatibilidad con las consultas delta de Microsoft Graph para oAuth2PermissionGrant está disponible para la versión preliminar pública.

**Tipo:** Nueva característica

**Categoría del servicio:** MS Graph

**Funcionalidad del producto:** Experiencia para el desarrollador

Las consultas delta para oAuth2PermissionGrant están disponibles en versión preliminar pública. Ahora puede realizar el seguimiento de los cambios sin necesidad de sondear Microsoft Graph continuamente. [Más información.](https://docs.microsoft.com/graph/api/oAuth2PermissionGrant-delta?view=graph-rest-beta&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-organizational-contact-generally-available"></a>La compatibilidad con las consultas delta de Microsoft Graph para contactos de la organización está disponible con carácter general.

**Tipo:** Nueva característica

**Categoría del servicio:** MS Graph

**Funcionalidad del producto:** Experiencia para el desarrollador

Las consultas delta para contactos de la organización están disponibles con carácter general. Ahora puede realizar el seguimiento de los cambios en las aplicaciones de producción sin necesidad de sondear Microsoft Graph continuamente. Reemplace el código existente que ahora sondea continuamente los datos de orgContact por una consulta delta para mejorar significativamente el rendimiento. [Más información.](https://docs.microsoft.com/graph/api/orgcontact-delta?view=graph-rest-1.0&tabs=http)

---

### <a name="microsoft-graph-delta-query-support-for-application-generally-available"></a>La compatibilidad con las consultas delta de Microsoft Graph para aplicaciones está disponible con carácter general.

**Tipo:** Nueva característica

**Categoría del servicio:** MS Graph

**Funcionalidad del producto:** Experiencia para el desarrollador

Las consultas delta para aplicaciones están disponibles con carácter general. Ahora puede realizar el seguimiento de los cambios en las aplicaciones de producción sin necesidad de sondear Microsoft Graph continuamente. Reemplace el código existente que ahora sondea continuamente los datos de la aplicación por una consulta delta para mejorar significativamente el rendimiento. [Más información.](https://docs.microsoft.com/graph/api/application-delta?view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-administrative-units-available-for-public-preview"></a>La compatibilidad con las consultas delta de Microsoft Graph para unidades administrativas está disponible en versión preliminar pública.

**Tipo:** Nueva característica

**Categoría del servicio:** MS Graph

**Funcionalidad del producto:** Las consultas delta de la experiencia del desarrollador para unidades administrativas están disponibles en versión preliminar pública. Ahora puede realizar el seguimiento de los cambios sin necesidad de sondear Microsoft Graph continuamente. [Más información.](https://docs.microsoft.com/graph/api/administrativeunit-delta?view=graph-rest-beta&tabs=http)

---

### <a name="manage-authentication-phone-numbers-and-more-in-new-microsoft-graph-beta-apis"></a>Administre los números de teléfono de autenticación y mucho más en las nuevas Microsoft Graph API en versión beta.

**Tipo:** Nueva característica

**Categoría del servicio:** MS Graph

**Funcionalidad del producto:** Experiencia para el desarrollador

Estas API son una herramienta fundamental para administrar los métodos de autenticación de los usuarios. Ahora puede realizar un registro previo y administrar mediante programación los autenticadores usados para MFA y el autoservicio de restablecimiento de contraseña (SSPR). Esta ha sido una de las características más solicitadas en las áreas de Azure MFA, SSPR y Microsoft Graph. Las nuevas API que hemos publicado en esta oleada le ofrecen la posibilidad de:

- Leer, agregar, actualizar y eliminar los teléfonos de autenticación de un usuario
- Restablecer la contraseña de un usuario
- Activar y desactivar el inicio de sesión por SMS

Para más información, consulte [Introducción a la API de métodos de autenticación de Azure AD](https://docs.microsoft.com/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

---

### <a name="administrative-units-public-preview"></a>Versión preliminar de las unidades administrativas

**Tipo:** Nueva característica

**Categoría del servicio:** RBAC

**Funcionalidad del producto:** Control de acceso

Las unidades administrativas permiten conceder permisos de administrador que están restringidos a un departamento, a una región o a otro segmento de la organización que se defina. Puede usar las unidades administrativas para delegar permisos en los administradores regionales o establecer directivas de forma pormenorizada. Por ejemplo, un administrador de cuentas de usuario podría actualizar la información del perfil, restablecer las contraseñas y asignar las licencias para los usuarios solo en su unidad administrativa.

Mediante el uso de unidades administrativas, un administrador central podría:

- Crear una unidad administrativa para la administración descentralizada de los recursos.
- Asignar un rol con permisos administrativos solo sobre los usuarios de Azure AD de una unidad administrativa.
- Rellenar las unidades administrativas con usuarios y grupos según sea necesario.

Para más información, consulte [Administración de unidades administrativas en Azure Active Directory (versión preliminar)](https://aka.ms/AdminUnitsDocs).

---

### <a name="printer-administrator-and-printer-technician-built-in-roles"></a>Roles integrados Administrador de impresoras y Técnico de impresoras

**Tipo:** Nueva característica

**Categoría del servicio:** RBAC

**Funcionalidad del producto:** Control de acceso

**Administrador de impresoras**: los usuarios con este rol pueden registrar impresoras y administrar todos los aspectos de todas las configuraciones de impresora en la solución de impresión universal de Microsoft, incluida la configuración del conector de impresión universal. Pueden dar su consentimiento a todas las solicitudes de permiso de impresión delegada. Los administradores de impresoras también tienen acceso a los informes de impresión. 

**Técnico de impresoras**: los usuarios con este rol pueden registrar impresoras y administrar el estado de la impresora en la solución de impresión universal de Microsoft. También pueden leer toda la información del conector. Una tarea clave que un Técnico de impresoras no puede realizar es establecer permisos de usuario en las impresoras ni compartir impresoras. [Más información.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#printer-administrator)

---

### <a name="hybrid-identity-admin-built-in-role"></a>Rol integrado Administrador de identidades híbridas

**Tipo:** Nueva característica

**Categoría del servicio:** RBAC

**Funcionalidad del producto:** Control de acceso

Los usuarios de este rol pueden habilitar, configurar y administrar servicios y configuraciones relacionados con la habilitación de la identidad híbrida en Azure AD. Este rol concede la capacidad de configurar Azure AD en uno de los tres métodos de autenticación admitidos: la sincronización de hash de contraseña (PHS), la autenticación de paso a través (PTA) o la federación (AD FS o proveedor de federación de terceros), así como implementar la infraestructura local relacionada para habilitarlos. La infraestructura local incluye agentes de aprovisionamiento y PTA. Este rol concede la capacidad de habilitar el inicio de sesión único de conexión directa (S-SSO) para habilitar la autenticación directa en dispositivos que no son de Windows 10 o equipos que no son de Windows Server 2016. Además, este rol concede la capacidad de ver los registros de inicio de sesión y el acceso a los datos de mantenimiento y análisis para la supervisión y la solución de problemas. [Más información.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#hybrid-identity-administrator)

---

### <a name="network-administrator-built-in-role"></a>Rol integrado Administrador de red

**Tipo:** Nueva característica

**Categoría del servicio:** RBAC

**Funcionalidad del producto:** Control de acceso

Los usuarios con este rol pueden revisar las recomendaciones de la arquitectura de perímetro de red de Microsoft que se basan en la telemetría de red desde sus ubicaciones de usuario. El rendimiento de red para Office 365 se basa en una arquitectura de perímetro de red de cliente empresarial rigurosa que suele ser específica de la ubicación del usuario. Este rol permite la edición de ubicaciones de usuario detectadas y la configuración de parámetros de red para esas ubicaciones con el fin de facilitar la mejora de las medidas de telemetría y las recomendaciones de diseño. [Más información.](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#network-administrator)

---

### <a name="bulk-activity-and-downloads-in-the-azure-ad-admin-portal-experience"></a>Actividad y descargas masivas en la experiencia del portal de administración de Azure AD

**Tipo:** Nueva característica

**Categoría del servicio:** User Management

**Funcionalidad del producto:** Directorio

Ahora puede realizar actividades masivas con usuarios y grupos de Azure AD mediante la carga de un archivo CSV en la experiencia del portal de administración de Azure AD. Puede crear usuarios o eliminar usuarios, e invitar a usuarios. También puede agregar y quitar miembros de un grupo.

Además, puede descargar listas de recursos de Azure AD desde la experiencia del portal de administración de Azure AD. Puede descargar la lista de usuarios del directorio, la lista de grupos del directorio y los miembros de un grupo determinado.

Para más información, consulte lo siguiente:

- [Crear usuarios](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-add) o [invitar a usuarios ](https://docs.microsoft.com/azure/active-directory/b2b/tutorial-bulk-invite).
- [Eliminar usuarios](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-delete) o [restaurar usuarios eliminados](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-restore).
- [Descargar la lista de usuarios](https://docs.microsoft.com/azure/active-directory/users-groups-roles/users-bulk-download) o [descargar la lista de grupos](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download)
- [Agregar (importar) miembros](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), [eliminar miembros](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members) o [descargar la lista de miembros](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members) de un grupo.

---

### <a name="my-staff-delegated-user-management"></a>Administración delegada de usuarios Mi personal

**Tipo:** Nueva característica

**Categoría del servicio:** User Management

**Funcionalidad del producto:**

Mi personal permite a los administradores de primera línea, como el administrador de una tienda, garantizar que los miembros del personal puedan acceder a sus cuentas de Azure AD. En lugar de depender de un departamento de soporte técnico central, las organizaciones pueden delegar tareas comunes, como el restablecimiento de contraseñas o el cambio de números de teléfono, en un administrador de primera línea. Con Mi personal, un usuario que no puede acceder a su cuenta puede recuperar el acceso con tan solo un par de clics, sin necesidad de ayuda del personal de TI o el departamento de soporte técnico. Para más información, consulte [Administración de usuarios con Mi personal (versión preliminar)](https://aka.ms/MyStaffAdminDocs) y [Delegación de la administración de usuarios con Mi personal (versión preliminar)](https://aka.ms/MyStaffUserDocs).

---

### <a name="an-upgraded-end-user-experience-in-access-reviews"></a>Una experiencia de usuario final actualizada en las revisiones de acceso

**Tipo:** Característica modificada

**Categoría del servicio:** Revisiones de acceso

**Funcionalidad del producto:** Identity Governance

Hemos actualizado la experiencia del revisor para las revisiones de acceso de Azure AD en el portal Aplicaciones. A finales de abril, los revisores que han iniciado sesión en la experiencia del revisor de las revisiones de acceso de Azure AD, verán un banner que les permitirá probar la experiencia actualizada en Mi acceso. Tenga en cuenta que la experiencia de revisiones de acceso actualizada ofrece la misma funcionalidad que la experiencia actual, pero con una interfaz de usuario mejorada sobre nuevas funcionalidades para permitir que los usuarios sean productivos. [Aquí encontrará más información sobre la experiencia actualizada](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review). Esta versión preliminar pública durará hasta finales de julio de 2020. A finales de julio, los revisores que no hayan participado en la experiencia en versión preliminar serán redirigidos automáticamente a Mi acceso para realizar las revisiones de acceso. Si desea que los revisores cambien permanentemente a la experiencia en versión preliminar en Mi acceso ahora, [realice una solicitud aquí](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u).

---

### <a name="workday-inbound-user-provisioning-and-writeback-apps-now-support-the-latest-versions-of-workday-web-services-api"></a>Las aplicaciones de aprovisionamiento y escritura diferida de usuarios de entrada de WorkDay ahora admiten las versiones más recientes de la API Workday Web Services

**Tipo:** Característica modificada

**Categoría del servicio:** Aprovisionamiento de aplicaciones

**Funcionalidad del producto:** 

Hemos tenido en cuenta los comentarios de los clientes y hemos actualizado las aplicaciones de aprovisionamiento y escritura diferida de usuarios de entrada de WorkDay en la galería de aplicaciones empresariales para admitir las versiones más recientes de la API Workday Web Services (WWS). Con este cambio, los clientes pueden especificar la versión de la API WWS que desean usar en la cadena de conexión. Esto proporciona a los clientes la capacidad de recuperar más atributos de recursos humanos disponibles en las versiones de WorkDay. La aplicación de escritura diferida de WorkDay ahora usa el servicio web de WorkDay recomendado, Change_Work_Contact_Info, para superar las limitaciones de Maintain_Contact_Info.

De forma predeterminada, si no se especifica ninguna versión en la cadena de conexión, las aplicaciones de aprovisionamiento de entrada de WorkDay seguirán usando WWS v21.1. Para cambiar a las versiones más recientes de la API Workday Web Services para el aprovisionamiento de usuarios de entrada, los clientes deben actualizar la cadena de conexión tal y como se documenta [en el tutorial](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles), y también actualizar los valores de XPATH utilizados para los atributos de WorkDay, tal y como se describe en [Guía de referencia de atributos de Workday](https://docs.microsoft.com/azure/active-directory/app-provisioning/workday-attribute-reference#xpath-values-for-workday-web-services-wws-api-v30). 

Para usar la nueva API para la escritura diferida, no se requieren cambios en la aplicación de aprovisionamiento de escritura diferida de WorkDay. En WorkDay, asegúrese de que la cuenta de usuario del sistema de integración (ISU) de WorkDay tiene permisos para invocar el proceso empresarial Change_Work_Contact, tal y como se describe en la sección del tutorial [Configuración de los permisos de la directiva de seguridad de procesos empresariales](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial#configuring-business-process-security-policy-permissions). 

Hemos actualizado la [guía de tutoriales](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) para reflejar la compatibilidad con la nueva versión de la API.

---

### <a name="users-with-default-access-role-are-now-in-scope-for-provisioning"></a>Los usuarios con el rol de acceso predeterminado ahora están dentro del ámbito del aprovisionamiento

**Tipo:** Característica modificada

**Categoría del servicio:** Aprovisionamiento de aplicaciones

**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Históricamente, los usuarios con el rol de acceso predeterminado han estado fuera del ámbito del aprovisionamiento. Hemos recibido comentarios acerca de que los clientes quieren que los usuarios con este rol estén dentro del ámbito del aprovisionamiento. A partir del 16 de abril de 2020, todas las nuevas configuraciones de aprovisionamiento permiten aprovisionar a los usuarios con el rol de acceso predeterminado. Cambiaremos gradualmente el comportamiento de las configuraciones de aprovisionamiento existentes para admitir el aprovisionamiento de usuarios con este rol. [Más información.](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned)

---

### <a name="updated-provisioning-ui"></a>Actualización de la interfaz de usuario de aprovisionamiento

**Tipo:** Característica modificada

**Categoría del servicio:** Aprovisionamiento de aplicaciones

**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Hemos actualizado la experiencia de aprovisionamiento para crear una vista de administración más centrada. Cuando vaya a la hoja de aprovisionamiento de una aplicación empresarial que ya se ha configurado, podrá supervisar fácilmente el progreso del aprovisionamiento y administrar acciones como el inicio, la detención y el reinicio del aprovisionamiento. [Más información.](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

---

### <a name="dynamic-group-rule-validation-is-now-available-for-public-preview"></a>La validación de reglas de grupos dinámicos ahora está disponible en versión preliminar pública

**Tipo:** Característica modificada

**Categoría del servicio:** Administración de grupos

**Funcionalidad del producto:** Colaboración

Azure Active Directory (Azure AD) ahora ofrece los medios para validar las reglas de grupos dinámicos. En la pestaña **Validación de las reglas**, puede validar la regla dinámica con los miembros del grupo de ejemplo, para confirmar que la regla funciona según lo previsto. Al crear o actualizar reglas de grupos dinámicos, los administradores quieren saber si un usuario o un dispositivo será miembro del grupo. Esto ayuda a evaluar si el usuario o el dispositivo cumplen los criterios de la regla y ayuda a solucionar problemas cuando no se espera la pertenencia.

Para más información, consulte [Validación de una regla de pertenencia dinámica a grupos (versión preliminar)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-rule-validation).

---

### <a name="identity-secure-score---security-defaults-and-mfa-improvement-action-updates"></a>Puntuación de seguridad de la identidad: valores predeterminados de seguridad y actualizaciones de las acciones de mejora de MFA

**Tipo:** Característica modificada

**Categoría del servicio:** N/D

**Funcionalidad del producto:** Seguridad y protección de la identidad

**Compatibilidad con los valores predeterminados de seguridad para las acciones de mejora de Azure AD:** La Puntuación de seguridad de Microsoft se actualizará con acciones de mejora para admitir [valores predeterminados de seguridad en Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults), lo que facilita la protección de la organización con una configuración de seguridad preconfigurada para ataques comunes. Esto afectará a las siguientes acciones de mejora:

- Garantizar que todos los usuarios pueden completar la autenticación multifactor para el acceso seguro
- Requerir MFA para roles administrativos
- Habilitar la directiva para bloquear la autenticación heredada
 
**Actualizaciones de las acciones de mejora de MFA:** Para reflejar la necesidad de las empresas de garantizar la mayor seguridad al aplicar directivas que funcionen con su negocio, la Puntuación de seguridad de Microsoft ha eliminado tres acciones de mejora centradas en la autenticación multifactor y ha agregado otras dos.

Acciones de mejora eliminadas:

- Registrar a todos los usuarios para la autenticación multifactor
- Exigir autenticación multifactor para todos los usuarios
- Requerir MFA para roles con privilegios Azure AD

Acciones de mejora agregadas:

- Garantizar que todos los usuarios pueden completar la autenticación multifactor para el acceso seguro
- Requerir MFA para roles administrativos

Estas nuevas acciones de mejora requerirán el registro de usuarios o administradores para la autenticación multifactor (MFA) en todo el directorio y el establecimiento del conjunto adecuado de directivas que se adapten a las necesidades de la organización. El objetivo principal es tener flexibilidad, asegurando al mismo tiempo que todos los usuarios y administradores puedan autenticarse con varios factores o solicitudes de comprobación de identidad basadas en riesgos. Se puede hacer con varias directivas que apliquen decisiones de ámbito o estableciendo valores predeterminados de seguridad (a partir del 16 de marzo) que permitan a Microsoft decidir cuándo desafiar a los usuarios con MFA. [Más información sobre las novedades en la Puntuación de seguridad de Microsoft](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide#whats-new).

---

## <a name="march-2020"></a>Marzo de 2020

### <a name="unmanaged-azure-active-directory-accounts-in-b2b-update-for-march-2021"></a>Cuentas de Azure Active Directory no administradas en la actualización B2B de marzo de 2021

**Tipo:** Plan de cambio  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C
 
**A partir del 31 de marzo de 2021**, Microsoft dejará de admitir el canje de invitaciones mediante la creación de cuentas de Azure Active Directory (Azure AD) no administradas e inquilinos para escenarios de colaboración B2B. Como preparación para esto, le recomendamos que opte por la [autenticación de código de acceso de un solo uso por correo electrónico](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode).

---

### <a name="users-with-the-default-access-role-will-be-in-scope-for-provisioning"></a>Los usuarios con el rol de acceso predeterminado estarán dentro del ámbito del aprovisionamiento.

**Tipo:** Plan de cambio  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
Históricamente, los usuarios con el rol de acceso predeterminado han estado fuera del ámbito del aprovisionamiento. Hemos recibido comentarios acerca de que los clientes quieren que los usuarios con este rol estén dentro del ámbito del aprovisionamiento. Estamos trabajando en la implementación de un cambio para que todas las nuevas configuraciones de aprovisionamiento permitan aprovisionar a los usuarios con el rol de acceso predeterminado. Gradualmente, cambiaremos el comportamiento de las configuraciones de aprovisionamiento existentes para admitir el aprovisionamiento de usuarios con este rol. No se requiere ninguna acción del cliente. Publicaremos una actualización en la [documentación](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned) una vez que este cambio esté en vigor.

---

### <a name="azure-ad-b2b-collaboration-will-be-available-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet-tenants"></a>La colaboración B2B de Azure AD estará disponible en Microsoft Azure controlado por inquilinos de 21Vianet (Azure China 21Vianet).

**Tipo:** Plan de cambio  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C
 
Las funcionalidades de colaboración B2B de Azure AD estarán disponibles en Microsoft Azure controlado por los inquilinos de 21Vianet (Azure China 21Vianet), lo que permite a los usuarios de un inquilino de Azure China 21Vianet colaborar sin problemas con los usuarios de otros inquilinos de Azure China 21Vianet. [Más información sobre la colaboración B2B de Azure AD](https://docs.microsoft.com/azure/active-directory/b2b/).

---
 
### <a name="azure-ad-b2b-collaboration-invitation-email-redesign"></a>Rediseño del correo electrónico de invitación de colaboración B2B de Azure AD

**Tipo:** Plan de cambio  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C
 
Los [correos electrónicos](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) enviados por el servicio de invitación de colaboración B2B de Azure AD para invitar a los usuarios al directorio se rediseñarán para que la información de la invitación y los siguientes pasos del usuario sean más claros.

---

### <a name="homerealmdiscovery-policy-changes-will-appear-in-the-audit-logs"></a>Los cambios de la directiva HomeRealmDiscovery aparecerán en los registros de auditoría

**Tipo:** Corregido  
**Categoría del servicio:** Auditoría  
**Funcionalidad del producto:** Supervisión e informes
 
Se ha corregido un error por el que los cambios en la [directiva HomeRealmDiscovery](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) no se incluían en los registros de auditoría. Ahora podrá ver cuándo y cómo ha cambiado la directiva, y quién la cambió. 

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (marzo de 2020)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En marzo de 2020, hemos agregado estas 51 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones: 

[Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect), [Zoho One China](https://docs.microsoft.com/azure/active-directory/saas-apps/zoho-one-china-tutorial), [PlusPlus](https://test.plusplus.app/auth/login/azuread-outlook/), [Profit.co SAML App](https://docs.microsoft.com/azure/active-directory/saas-apps/profitco-saml-app-tutorial), [iPoint Service Provider](https://docs.microsoft.com/azure/active-directory/saas-apps/ipoint-service-provider-tutorial), [contexxt.ai SPHERE](https://contexxt-sphere.com/login), [Wisdom By Invictus](https://docs.microsoft.com/azure/active-directory/saas-apps/wisdom-by-invictus-tutorial), [Flare Digital Signage](https://spark-dev.pixelnebula.com/login), [Logz.io - Cloud Observability for Engineers](https://docs.microsoft.com/azure/active-directory/saas-apps/logzio-cloud-observability-for-engineers-tutorial), [SpectrumU](https://docs.microsoft.com/azure/active-directory/saas-apps/spectrumu-tutorial), [BizzContact](https://bizzcontact.app/), [Elqano SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/elqano-sso-tutorial), [MarketSignShare](http://www.signshare.com/), [CrossKnowledge Learning Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/crossknowledge-learning-suite-tutorial), [Netvision Compas](https://docs.microsoft.com/azure/active-directory/saas-apps/netvision-compas-tutorial), [FCM HUB](https://docs.microsoft.com/azure/active-directory/saas-apps/fcm-hub-tutorial), [RIB A/S Byggeweb Mobile](https://apps.apple.com/us/app/docia/id529058757), [GoLinks](https://docs.microsoft.com/azure/active-directory/saas-apps/golinks-tutorial), [Datadog](https://docs.microsoft.com/azure/active-directory/saas-apps/datadog-tutorial), [Zscaler B2B User Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-b2b-user-portal-tutorial), [LIFT](https://docs.microsoft.com/azure/active-directory/saas-apps/lift-tutorial), [Planview Enterprise One](https://docs.microsoft.com/azure/active-directory/saas-apps/planview-enterprise-one-tutorial), [WatchTeams](https://www.devfinition.com/), [Aster](https://demo.asterapp.io/login), [Skills Workflow](https://docs.microsoft.com/azure/active-directory/saas-apps/skills-workflow-tutorial), [Node Insight](https://admin.nodeinsight.com/AADLogin.aspx), [IP Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/ip-platform-tutorial), [InVision](https://docs.microsoft.com/azure/active-directory/saas-apps/invision-tutorial), [Pipedrive](https://docs.microsoft.com/azure/active-directory/saas-apps/pipedrive-tutorial), [Showcase Workshop](https://app.showcaseworkshop.com/), [Greenlight Integration Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-integration-platform-tutorial), [Greenlight Compliant Access Management](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-compliant-access-management-tutorial), [Grok Learning](https://docs.microsoft.com/azure/active-directory/saas-apps/grok-learning-tutorial), [Miradore Online](https://login.online.miradore.com/), [Khoros Care](https://docs.microsoft.com/azure/active-directory/saas-apps/khoros-care-tutorial), [AskYourTeam](https://docs.microsoft.com/azure/active-directory/saas-apps/askyourteam-tutorial), [TruNarrative](https://docs.microsoft.com/azure/active-directory/saas-apps/trunarrative-tutorial), [Smartwaiver](https://www.smartwaiver.com/m/user/sw_login.php?wms_login), [Bizagi Studio for Digital Process Automation](https://docs.microsoft.com/azure/active-directory/saas-apps/bizagi-studio-for-digital-process-automation-tutorial), [insuiteX](https://www.insuite.jp/), [sybo](https://www.systexsoftware.com.tw/), [Britive](https://docs.microsoft.com/azure/active-directory/saas-apps/britive-tutorial), [WhosOffice](https://docs.microsoft.com/azure/active-directory/saas-apps/whosoffice-tutorial), [E-days](https://docs.microsoft.com/azure/active-directory/saas-apps/e-days-tutorial), [Kollective SDN](https://portal.kollective.app/login), [Witivio](https://app.witivio.com/), [Playvox](https://my.playvox.com/login), [Korn Ferry 360](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-360-tutorial), [Campus Café](https://docs.microsoft.com/azure/active-directory/saas-apps/campus-cafe-tutorial), [Catchpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/catchpoint-tutorial), [Code42](https://docs.microsoft.com/azure/active-directory/saas-apps/code42-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-b2b-collaboration-available-in-azure-government-tenants"></a>Está disponible la colaboración B2B de Azure AD en inquilinos de Azure Government

**Tipo:** Nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C
 
Las características de colaboración B2B de Azure AD ahora están disponibles entre algunos inquilinos de Azure Government.  Para averiguar si su inquilino es capaz de usar estas funcionalidades, siga las instrucciones que se indican en [¿Cómo puedo saber si la colaboración B2B está disponible en mi inquilino de Azure US Government?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant)

---

### <a name="azure-monitor-integration-for-azure-logs-is-now-available-in-azure-government"></a>La integración de Azure Monitor para los registros de Azure ahora está disponible en Azure Government

**Tipo:** Nueva característica  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes
 
La integración de Azure Monitor en los registros de Azure AD ahora está disponible en Azure Government. Puede enrutar los registros de Azure AD (registros de auditoría e inicio de sesión) a una cuenta de almacenamiento, un centro de eventos y Log Analytics. Consulte la [documentación detallada](https://aka.ms/aadlogsinamd), así como los [planes de implementación para la elaboración de informes y la supervisión](https://docs.microsoft.com/azure/active-directory/reports-monitoring/plan-monitoring-and-reporting) para escenarios de Azure AD.

---

### <a name="identity-protection-refresh-in-azure-government"></a>Actualización de Identity Protection en Azure Government

**Tipo:** Nueva característica  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Nos complace compartir que ahora hemos implementado la experiencia actualizada de [Azure AD Identity Protection](https://aka.ms/IdentityProtectionDocs) en el [portal de Microsoft Azure Government](https://portal.azure.us/). Para obtener más información, consulte nuestra [entrada de blog de anuncio](https://techcommunity.microsoft.com/t5/public-sector-blog/identity-protection-refresh-in-microsoft-azure-government/ba-p/1223667).

---

### <a name="disaster-recovery-download-and-store-your-provisioning-configuration"></a>Recuperación ante desastres: Descarga y almacenamiento de la configuración de aprovisionamiento

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad
 
El servicio de aprovisionamiento de Azure AD proporciona un amplio conjunto de funcionalidades de configuración. Los clientes deben poder guardar su configuración para poder consultarla más adelante o revertir a una versión válida conocida. Hemos agregado la capacidad de descargar la configuración de aprovisionamiento como archivo JSON y cargarlo cuando sea necesario. [Más información](https://docs.microsoft.com/azure/active-directory/app-provisioning/export-import-provisioning-configuration).

---
 
### <a name="sspr-self-service-password-reset-now-requires-two-gates-for-admins-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>SSPR (autoservicio de restablecimiento de contraseña) ahora requiere dos puertas para los administradores en Microsoft Azure controlado por 21Vianet (Azure China 21Vianet) 

**Tipo:** Característica modificada  
**Categoría del servicio:** Restablecimiento de contraseña de autoservicio  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Anteriormente, en Microsoft Azure controlado por 21Vianet (Azure China 21Vianet), los administradores que usaban el autoservicio de restablecimiento de contraseña (SSPR) para restablecer sus propias contraseñas solo necesitaban una "puerta" (desafío) para demostrar su identidad. En las nubes públicas y en otras nubes nacionales, los administradores generalmente deben usar dos puertas para demostrar su identidad al usar SSPR. No obstante, dado que no se admiten las llamadas telefónicas ni SMS en Azure China 21Vianet, hemos permitido que los administradores restablezcan la contraseña con una sola puerta.

Estamos creando la paridad de características de SSPR entre Azure China 21Vianet y la nube pública. En el futuro, los administradores deberán usar dos puertas al usar SSPR. Se admitirán SMS, llamadas telefónicas, y códigos y notificaciones de aplicación de autenticador. [Más información](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#administrator-reset-policy-differences).

---

### <a name="password-length-is-limited-to-256-characters"></a>La longitud de las contraseñas está limitada a 256 caracteres

**Tipo:** Característica modificada  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios
 
Para garantizar la confiabilidad del servicio de Azure AD, las contraseñas de usuario ahora tienen una longitud máxima de 256 caracteres. A los usuarios con contraseñas más largas que esta se les pedirá que cambien su contraseña en el inicio de sesión subsiguiente, ya sea poniéndose en contacto con su administrador o mediante la característica de autoservicio de restablecimiento de contraseña.

Este cambio se habilitó el 13 de marzo de 2020, a las 10:00 PST (18:00 UTC), y el error es AADSTS 50052, InvalidPasswordExceedsMaxLength. Para obtener más información, consulte el [aviso de cambio importante](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#user-passwords-will-be-restricted-to-256-characters).

---

### <a name="azure-ad-sign-in-logs-are-now-available-for-all-free-tenants-through-the-azure-portal"></a>Los registros de inicio de sesión de Azure AD ahora están disponibles para todos los inquilinos gratuitos a través de Azure Portal

**Tipo:** Característica modificada  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes
 
A partir de ahora, los clientes que dispongan de inquilinos gratuitos pueden tener acceso a los [registros de inicio de sesión de Azure AD desde Azure Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) para un máximo de 7 días. Anteriormente, los registros de inicio de sesión solo estaban disponibles para los clientes con licencias de Azure Active Directory Premium. Con este cambio, todos los inquilinos pueden tener acceso a estos registros a través del portal.

> [!NOTE]
> Los clientes todavía necesitan una licencia Premium (Azure Active Directory Premium P1 o P2) para tener acceso a los registros de inicio de sesión a través de Microsoft Graph API y Azure Monitor.

---

### <a name="deprecation-of-directory-wide-groups-option-from-groups-general-settings-on-azure-portal"></a>Opción en desuso de grupos de todo el directorio de la configuración general de grupos en Azure Portal

**Tipo:** Obsoleto  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

Para ofrecer a los clientes una manera más flexible de crear grupos de todo el directorio que se adapte mejor a sus necesidades, hemos reemplazado la opción **Grupos de todo el directorio** de la opción **Grupos** > **General** en Azure Portal por un vínculo a la [documentación de grupos dinámicos](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership). Hemos mejorado la documentación para incluir más instrucciones para que los administradores puedan crear grupos de todos los usuarios que incluyan o excluyan usuarios invitados.

---

## <a name="february-2020"></a>Febrero de 2020

### <a name="upcoming-changes-to-custom-controls"></a>Próximos cambios en los controles personalizados

**Tipo:** Plan de cambio  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Tenemos previsto reemplazar la versión preliminar actual de los controles personalizados por un enfoque que permita que las funcionalidades de autenticación proporcionadas por el partner funcionen sin problemas con las experiencias de administrador y usuario final de Azure Active Directory. Hoy en día, las soluciones de MFA de los partners enfrentan las siguientes limitaciones: solo funcionan después de haber escrito una contraseña; no sirven como MFA para la autenticación de nivel superior en otros escenarios clave; y no se integran con las funciones de administración de credenciales administrativas ni de usuario final. La nueva implementación permitirá que los factores de autenticación proporcionados por los partners funcionen junto con factores integrados para escenarios clave, como el registro, el uso, las notificaciones de MFA, la autenticación de nivel superior, la creación de informes y el registro. 

Los controles personalizados se seguirán admitiendo en la versión preliminar junto con el nuevo diseño hasta que alcancen la disponibilidad general. En ese momento, daremos tiempo a los clientes para migrar al nuevo diseño. Debido a las limitaciones del enfoque actual, no se incorporarán nuevos proveedores hasta que el nuevo diseño esté disponible. Estamos trabajando en estrecha colaboración con los clientes y proveedores, y comunicaremos los plazos a medida que nos acerquemos. [Más información](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/upcoming-changes-to-custom-controls/ba-p/1144696#).

---

### <a name="identity-secure-score---mfa-improvement-action-updates"></a>Puntuación de seguridad de la identidad: actualizaciones de acciones de mejora de MFA

**Tipo:** Plan de cambio  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Para reflejar la necesidad de las empresas de garantizar la mayor seguridad al aplicar directivas que funcionen con su negocio, la puntuación segura de Microsoft va a quitar tres acciones de mejora centradas en la autenticación multifactor (MFA) y va a añadir otras dos.

Se quitarán las siguientes acciones de mejora:

- Registrar todos los usuarios de MFA
- Exigir autenticación multifactor para todos los usuarios
- Requerir MFA para roles con privilegios Azure AD

Se agregarán las siguientes acciones de mejora:

- Asegurarse de que todos los usuarios pueden completar MFA para el acceso seguro
- Requerir MFA para roles administrativos

Estas nuevas acciones de mejora requerirán el registro de usuarios o administradores para MFA en todo el directorio y el establecimiento del conjunto adecuado de directivas que se adapten a las necesidades de la organización. El objetivo principal es tener flexibilidad, asegurando al mismo tiempo que todos los usuarios y administradores puedan autenticarse con varios factores o solicitudes de comprobación de identidad basadas en riesgos. Esto puede traducirse en la configuración de valores predeterminados de seguridad que permitan a Microsoft decidir cuándo desafiar a los usuarios para MFA o el mantenimiento de varias directivas que apliquen decisiones de ámbito. Como parte de estas actualizaciones de acciones de mejora, las directivas de protección de línea de base ya no se incluirán en los cálculos de puntuación. [Obtenga más información sobre lo próximo de la puntuación segura de Microsoft](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score-whats-coming?view=o365-worldwide).

---

### <a name="azure-ad-domain-services-sku-selection"></a>Selección de SKU Azure AD Domain Services

**Tipo:** Nueva característica  
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services
 
Hemos escuchado los comentarios de los clientes de Azure AD Domain Services que desean más flexibilidad a la hora de seleccionar los niveles de rendimiento de sus instancias. A partir del 1 de febrero de 2020, cambiamos de un modelo dinámico (donde Azure AD determina el rendimiento y el plan de tarifa en función del número de objetos) a un modelo de selección automática. Ahora los clientes pueden elegir un nivel de rendimiento que se adapte a su entorno. Este cambio también nos permite habilitar nuevos escenarios, como los bosques de recursos, y características Premium, como las copias de seguridad diarias. El número de objetos es ahora ilimitado para todas las SKU, pero seguiremos ofreciendo sugerencias de cantidades de objetos para cada nivel.

**No se requiere ninguna acción inmediata del cliente.** Para los clientes actuales, el nivel dinámico que estaba en uso el 1 de febrero de 2020 determina el nuevo nivel predeterminado. El resultado de este cambio no afecta al precio ni al rendimiento. A partir de entonces, los clientes de Azure AD DS deberán evaluar los requisitos de rendimiento a medida que cambien sus características de carga de trabajo y el tamaño del directorio. El cambio entre los niveles de servicio seguirá siendo una operación sin tiempo de inactividad, y ya no se moverá automáticamente a los clientes a nuevos niveles en función del crecimiento de su directorio. Además, no habrá ningún aumento del precio y los nuevos precios se alinearán con nuestro modelo de facturación actual. Para obtener más información, consulte la [documentación de las SKU de Azure AD DS](https://docs.microsoft.com/azure/active-directory-domain-services/administration-concepts#azure-ad-ds-skus) y la [página de precios de Azure AD Domain Services](https://azure.microsoft.com/pricing/details/active-directory-ds/).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (febrero de 2020)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En febrero de 2020, hemos agregado estas 31 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones: 

[IamIP Patent Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/iamip-patent-platform-tutorial), [Experience Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/experience-cloud-tutorial), [NS1 SSO For Azure](https://docs.microsoft.com/azure/active-directory/saas-apps/ns1-sso-azure-tutorial), [Barracuda Email Security Service](https://ess.barracudanetworks.com/sso/azure), [ABa Reporting](https://myaba.co.uk/client-access/signin/auth/msad), [In Case of Crisis - Online Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-online-portal-tutorial), [BIC Cloud Design](https://docs.microsoft.com/azure/active-directory/saas-apps/bic-cloud-design-tutorial), [Beekeeper Azure AD Data Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/beekeeper-azure-ad-data-connector-tutorial), [Korn Ferry Assessments](https://www.kornferry.com/solutions/kf-digital/kf-assess), [Verkada Command](https://docs.microsoft.com/azure/active-directory/saas-apps/verkada-command-tutorial), [Splashtop](https://docs.microsoft.com/azure/active-directory/saas-apps/splashtop-tutorial), [Syxsense](https://docs.microsoft.com/azure/active-directory/saas-apps/syxsense-tutorial), [EAB Navigate](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-tutorial), [New Relic (Limited Release)](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial), [Thulium](https://admin.thulium.com/login/instance), [Ticket Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/ticketmanager-tutorial), [Template Chooser for Teams](https://links.officeatwork.com/templatechooser-download-teams), [Beesy](https://www.beesy.me/index.php/site/login), [Health Support System](https://docs.microsoft.com/azure/active-directory/saas-apps/health-support-system-tutorial), [MURAL](https://app.mural.co/signup), [Hive](https://docs.microsoft.com/azure/active-directory/saas-apps/hive-tutorial), [LavaDo](https://appsource.microsoft.com/product/web-apps/lavaloon.lavado_standard?tab=Overview), [Wakelet](https://wakelet.com/login), [Firmex VDR](https://docs.microsoft.com/azure/active-directory/saas-apps/firmex-vdr-tutorial), [ThingLink for Teachers and Schools](https://www.thinglink.com/), [Coda](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial), [NearpodApp](https://nearpod.com/signup/?oc=Microsoft&utm_campaign=Microsoft&utm_medium=site&utm_source=product), [WEDO](https://docs.microsoft.com/azure/active-directory/saas-apps/wedo-tutorial), [InvitePeople](https://invitepeople.com/login), [Reprints Desk - Article Galaxy](https://docs.microsoft.com/azure/active-directory/saas-apps/reprints-desk-article-galaxy-tutorial), [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-tutorial)

 
Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---february-2020"></a>Nuevos conectores de aprovisionamiento en la galería de aplicaciones de Azure AD (febrero de 2020)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Mixpanel](https://docs.microsoft.com/azure/active-directory/saas-apps/mixpanel-provisioning-tutorial)
- [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-provisioning-tutorial)
- [Azure Databricks](https://docs.microsoft.com/azure/active-directory/saas-apps/azure-databricks-scim-connector-provisioning-tutorial)
- [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-provisioning-tutorial)
- [Zapier](https://docs.microsoft.com/azure/active-directory/saas-apps/zapier-provisioning-tutorial)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="azure-ad-support-for-fido2-security-keys-in-hybrid-environments"></a>Compatibilidad de Azure AD con las claves de seguridad de FIDO2 en entornos híbridos

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios
 
Anunciamos la versión preliminar pública de la compatibilidad de Azure AD con las claves de seguridad de FIDO2 en entornos híbridos. Los usuarios ahora pueden usar las claves de seguridad de FIDO2 para iniciar sesión en sus dispositivos de Windows 10 unidos a Azure AD híbrido y obtener un inicio de sesión fluido en sus recursos locales y en la nube. La compatibilidad con entornos híbridos ha sido la característica más solicitada de nuestros clientes sin contraseña, ya que inicialmente lanzamos la versión preliminar pública para la compatibilidad con FIDO2 en dispositivos unidos a Azure AD. La autenticación sin contraseña mediante tecnologías avanzadas como la biométrica y la criptografía de clave privada y pública es cómoda y fácil de usar a la vez que segura. Con esta versión preliminar pública, ahora puede usar la autenticación moderna como las claves de seguridad de FIDO2 para acceder a los recursos de Active Directory tradicionales. Para obtener más información, vaya a [Inicio de sesión único en recursos locales](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises). 

Para empezar, visite [Habilitación de las claves de seguridad FIDO2 para un inquilino](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key) para obtener instrucciones paso a paso. 

---
 
### <a name="the-new-my-account-experience-is-now-generally-available"></a>La nueva experiencia de Mi cuenta ya está disponible con carácter general.

**Tipo:** Característica modificada  
**Categoría del servicio:** Mi perfil/cuenta  
**Funcionalidad del producto:** Experiencias de usuario final
 
Mi cuenta, el punto en el que se atienden todas las necesidades de administración de las cuentas de usuario final, ya está disponible con carácter general. Los usuarios finales pueden tener acceso a este nuevo sitio a través de la dirección URL o en el encabezado de la nueva experiencia Mis aplicaciones. Obtenga más información sobre todas las funcionalidades de autoservicio que ofrece la nueva experiencia en [¿Qué es el portal Mi cuenta?](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview)

---
 
### <a name="my-account-site-url-updating-to-myaccountmicrosoftcom"></a>Actualización de la dirección URL del sitio de Mi cuenta a myaccount.microsoft.com

**Tipo:** Característica modificada  
**Categoría del servicio:** Mi perfil/cuenta  
**Funcionalidad del producto:** Experiencias de usuario final
 
La nueva experiencia de usuario final de Mi cuenta actualizará su dirección URL a `https://myaccount.microsoft.com` el próximo mes. Obtenga más información sobre la experiencia y todas las funcionalidades de autoservicio de cuenta que ofrece a los usuarios finales en la [ayuda del portal Mi cuenta](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview).

---
 
## <a name="january-2020"></a>Enero de 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>El nuevo portal Mis aplicaciones ya está disponible con carácter general

**Tipo:** Plan de cambio  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** Experiencias de usuario final
 
Actualice su organización al nuevo portal Mis aplicaciones que ya está disponible con carácter general. Puede encontrar más información sobre el nuevo portal y las colecciones en [Creación de colecciones en el portal Mis aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Las áreas de trabajo de Azure AD se han renombrado a colecciones

**Tipo:** Característica modificada  
**Categoría del servicio:** Mis aplicaciones   
**Funcionalidad del producto:** Experiencias de usuario final
 
Las áreas de trabajo, que son filtros que los administradores pueden configurar para organizar las aplicaciones de sus usuarios, ahora se llamarán colecciones. Obtenga más información sobre cómo configurarlas en [Creación de colecciones en el portal Mis aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Registro e inicio de sesión mediante teléfono en Azure AD B2C con una directiva personalizada (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
Con el registro y el inicio de sesión mediante el número de teléfono, los desarrolladores y las empresas pueden permitir a sus clientes registrarse e iniciar sesión con una contraseña de un solo uso enviada al número de teléfono del usuario a través de un SMS. Esta característica también permite al cliente cambiar su número de teléfono si pierde el acceso a su dispositivo. Con la eficacia de las directivas personalizadas, el registro y el inicio de sesión mediante el teléfono permiten a los desarrolladores y a las empresas comunicar su marca a través de la personalización de la página. Aprenda cómo [configurar el registro y el inicio de sesión en el teléfono con directivas personalizadas en Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication).
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Nuevos conectores de aprovisionamiento en la galería de aplicaciones de Azure AD (enero de 2020)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler Private Access](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (enero de 2020)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En enero de 2020, hemos agregado estas 33 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones: 

[JOSA](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [Fastly Edge Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [Terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [Spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Abibot Netlogistik](https://azuremarketplace.microsoft.com/marketplace/apps/aad.abibotnetlogistik), [SkyKick](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access), [Upshotly](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial), [LeaveBot](https://leavebot.io/#home), [DataCamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [TripActions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [SmartWork](https://www.intumit.com/english/SmartWork.html), [Dotcom-Monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [SSOGEN - Azure AD SSO Gateway for Oracle E-Business Suite - EBS, PeopleSoft y JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial), [Hosted MyCirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [Yuhu Property Management Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [LumApps](https://sites.lumapps.com/login), [Upwork Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [Talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [SmartDB for Microsoft Teams](http://teams.smartdb.jp/login/), [PressPage](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [ContractSafe Saml2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [Maxient Conduct Manager Software](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [Helpshift](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/), [CoreView](https://portal.coreview.com/), [Squelch Cloud Office365 Connector](https://laxmi.squelch.io/login), [PingFlow Authentication](https://app-staging.pingview.io/), [ PrinterLogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [Taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [Sandwai](https://app.sandwai.com/), [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [AssetSonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [Akari Virtual Assistant](https://akari.io/akari-virtual-assistant/)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="two-new-identity-protection-detections"></a>Dos nuevas detecciones de Identity Protection

**Tipo:** Nueva característica  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Hemos agregado a Identity Protection dos nuevos tipos de detección vinculados al inicio de sesión: Reglas de manipulación sospechosa de la bandeja de entrada y Viaje imposible. Microsoft Cloud App Security (MCAS) descubre estas detecciones sin conexión e influye en los riesgos de usuario y de inicio de sesión en Identity Protection. Para más información sobre estas detecciones, consulte nuestros [tipos de riesgo de inicio de sesión](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk).
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Cambio de última hora: los fragmentos de URI no se trasladarán a través de la redirección de inicio de sesión

**Tipo:** Característica modificada  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios
 
A partir del 8 de febrero de 2020, cuando se envíe una solicitud a login.microsoftonline.com para que un usuario inicie sesión, el servicio anexará un fragmento vacío a la solicitud.  Esto evita una clase de ataques de redireccionamiento, ya que se asegura de que el explorador borra todo fragmento existente en la solicitud. Ninguna aplicación debe tener una dependencia de este comportamiento. Para más información, consulte [Cambios importantes](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020) en la documentación de la plataforma de identidad de Microsoft.

---

## <a name="december-2019"></a>Diciembre de 2019

### <a name="integrate-sap-successfactors-provisioning-into-azure-ad-and-on-premises-ad-public-preview"></a>Integración del aprovisionamiento de SAP SuccessFactors en Azure AD y AD local (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Ahora puede integrar SAP SuccessFactors como origen de identidad relevante en Azure AD. Esta integración le ayuda a automatizar el ciclo de vida de la identidad de un extremo a otro, incluido el uso de eventos basados en recursos humanos, como nuevas contrataciones o resoluciones de contrato, para controlar el aprovisionamiento de cuentas de Azure AD.

Para obtener más información sobre cómo configurar el aprovisionamiento de entrada de SAP SuccessFactors para Azure AD, consulte el tutorial sobre [configuración del aprovisionamiento automático de SAP SuccessFactors](https://aka.ms/SAPSuccessFactorsInboundTutorial).

---

### <a name="support-for-customized-emails-in-azure-ad-b2c-public-preview"></a>Compatibilidad con correos electrónicos personalizados en Azure AD B2C (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C

Ahora puede usar Azure AD B2C para crear correos electrónicos personalizados cuando los usuarios se suscriban para usar las aplicaciones. Mediante el uso de DisplayControls (actualmente en versión preliminar) y un proveedor de correo electrónico de terceros (por ejemplo, [SendGrid](https://sendgrid.com/), [SparkPost](https://sparkpost.com/) o una API REST personalizada), puede usar su propia plantilla de correo electrónico, dirección **De** y texto del asunto, además de admitir la localización y la configuración personalizada de contraseña de un solo uso (OTP).

Para obtener más información, consulte el artículo sobre [comprobación de correo electrónico personalizada en Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-email).

---

### <a name="replacement-of-baseline-policies-with-security-defaults"></a>Sustitución de directivas de base de referencia por valores predeterminados de seguridad

**Tipo:** Característica modificada  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Como parte de un modelo predeterminado seguro para la autenticación, vamos a quitar las directivas de protección de base de referencia existentes de todos los inquilinos. Se prevé que esta retirada se complete a finales de febrero. El reemplazo de estas directivas de protección de base de referencia son los valores predeterminados de seguridad. Si ha usado directivas de protección de base de referencia, debe planear la migración a la nueva directiva de valores predeterminados de seguridad o al acceso condicional. Si no ha usado estas directivas, no tiene que realizar ninguna acción.

Para obtener más información sobre los nuevos valores predeterminados de seguridad, consulte [¿Qué son los valores predeterminados de seguridad?](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) Para obtener más información sobre las directivas de acceso condicional, consulte [Directivas de acceso condicional habituales](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common).

---

## <a name="november-2019"></a>Noviembre de 2019

### <a name="support-for-the-samesite-attribute-and-chrome-80"></a>Compatibilidad con el atributo SameSite y Chrome 80

**Tipo:** Plan de cambio  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

Como parte de un modelo predeterminado y seguro para las cookies, el explorador Chrome 80 cambia el modo en que trata las cookies sin el atributo `SameSite`. Cualquier cookie que no especifique el atributo `SameSite` se tratará como si se hubiera establecido en `SameSite=Lax`, lo que producirá un bloqueo de Chrome en ciertos escenarios de uso compartido de cookies entre dominios de los que puede depender su aplicación. Para mantener el comportamiento de Chrome anterior, puede utilizar el atributo `SameSite=None` y agregar un atributo `Secure` adicional, por lo que solo se puede tener acceso a las cookies entre sitios a través de conexiones HTTPS. Chrome está programado para completar este cambio el 4 de febrero de 2020.

Se recomienda que todos los desarrolladores prueben sus aplicaciones siguiendo esta guía:

- Establecer el valor predeterminado de la opción **Usar cookies seguras** en **Sí**.

- Establecer el valor predeterminado del atributo **SameSite** en **Ninguno**.

- Agregar un atributo `SameSite` adicional de **Seguro**.

Para obtener más información, consulte el artículo sobre [próximos cambios en la cookie de SameSite en ASP.NET y ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) y [Posible interrupción para los sitios web de los clientes y los servicios de Microsoft en la versión 79 de Chrome Beta y versiones posteriores](https://support.microsoft.com/help/4522904/potential-disruption-to-microsoft-services-in-chrome-beta-version-79).

---

### <a name="new-hotfix-for-microsoft-identity-manager-mim-2016-service-pack-2-sp2"></a>Nueva revisión para Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2)

**Tipo:** Corregido  
**Categoría del servicio:** Microsoft Identity Manager  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Un paquete acumulativo de revisiones (compilación 4.6.34.0) está disponible para Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2). Este paquete acumulativo resuelve problemas y agrega mejoras que se describen en la sección sobre "problemas corregidos y mejoras agregadas en esta actualización".

Para obtener más información y descargar el paquete de revisiones, consulte el artículo [El paquete acumulativo de actualizaciones de Microsoft Identity Manager 2016 Service Pack 2 (compilación 4.6.34.0) está disponible](https://support.microsoft.com/help/4512924/microsoft-identity-manager-2016-service-pack-2-build-4-6-34-0-update-r).

---

### <a name="new-ad-fs-app-activity-report-to-help-migrate-apps-to-azure-ad-public-preview"></a>Nuevo informe de actividad de aplicaciones de AD FS para ayudar a migrar las aplicaciones a Azure AD (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Use el nuevo informe de actividad de aplicaciones de Servicios de federación de Active Directory (AD FS) en Azure Portal para identificar las aplicaciones que se pueden migrar a Azure AD. El informe evalúa todas las aplicaciones de AD FS con respecto a la compatibilidad con Azure AD, comprueba si hay problemas y proporciona instrucciones sobre cómo preparar aplicaciones concretas para su migración.

Para obtener más información, consulte [Uso del informe de actividades de aplicaciones de AD FS para migrar aplicaciones a Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity).

---

### <a name="new-workflow-for-users-to-request-administrator-consent-public-preview"></a>Nuevo flujo de trabajo para que los usuarios soliciten el consentimiento del administrador (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Control de acceso

El flujo de trabajo de consentimiento del administrador proporciona a los administradores una manera de conceder acceso a las aplicaciones que requieren la aprobación del administrador. Si un usuario intenta acceder a una aplicación, pero no puede dar su consentimiento, puede enviar una solicitud de aprobación del administrador. La solicitud se envía por correo electrónico, y se coloca en una cola a la que se puede acceder desde Azure Portal, a todos los administradores que se han designado como revisores. Una vez que un revisor realiza una acción en una solicitud pendiente, se notifica la acción a los usuarios que realizan la solicitud.

Para obtener más información, consulte [Configuración del flujo de trabajo de consentimiento del administrador (versión preliminar)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow).

---

### <a name="new-azure-ad-app-registrations-token-configuration-experience-for-managing-optional-claims-public-preview"></a>Nueva experiencia de configuración del token de registros de aplicación de Azure AD para administrar notificaciones opcionales (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Experiencia para el desarrollador

La nueva hoja de **configuración del token de registros de aplicación de Azure AD** de Azure Portal muestra ahora a los desarrolladores de aplicaciones una lista dinámica de notificaciones opcionales para sus aplicaciones. Esta nueva experiencia ayuda a simplificar las migraciones de aplicaciones de Azure AD y a minimizar las configuraciones incorrectas de las notificaciones opcionales.

Para obtener más información, consulte [Proporcionar notificaciones opcionales a la aplicación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).

---

### <a name="new-two-stage-approval-workflow-in-azure-ad-entitlement-management-public-preview"></a>Nuevo flujo de trabajo de aprobación en dos fases de administración de derechos de Azure AD (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Administración de derechos

Hemos presentado un nuevo flujo de trabajo de aprobación en dos fases que le permite exigir dos aprobadores para aprobar la solicitud de un usuario a un paquete de acceso. Por ejemplo, puede establecerlo de manera que el administrador del usuario que realiza la solicitud deba dar su aprobación en primer lugar y, a continuación, también puede exigir que un propietario del recurso dé su aprobación. Si uno de los aprobadores no da su aprobación, no se concede el acceso.

Para obtener más información, consulte [Cambio de la configuración de la solicitud y aprobación para un paquete de acceso de administración de derechos de Azure AD](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-request-policy).

---

### <a name="updates-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Actualizaciones de la página Mis aplicaciones con nuevas áreas de trabajo (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** Integración de terceros

Ahora puede personalizar la forma en que los usuarios de la organización ven y acceden a la experiencia actualizada de Mis aplicaciones. Esta nueva experiencia también incluye la nueva característica de áreas de trabajo, que facilita a los usuarios la búsqueda y organización de aplicaciones.

Para obtener más información sobre la nueva experiencia de Mis aplicaciones y la creación de áreas de trabajo, consulte el artículo sobre [creación de áreas de trabajo en el portal Mis aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="google-social-id-support-for-azure-ad-b2b-collaboration-general-availability"></a>Compatibilidad con identificadores sociales de Google para la colaboración B2B de Azure AD (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** Autenticación de usuarios

La nueva compatibilidad con el uso de identificadores sociales de Google (cuentas Gmail) en Azure AD ayuda a simplificar la colaboración entre sus usuarios y partners. Ya no hay necesidad de que los partners creen y administren una cuenta específica de Microsoft. Microsoft Teams ahora es totalmente compatible con los usuarios de Google en todos los clientes y en los puntos de conexión de autenticación comunes y relacionados con los inquilinos.

Para obtener más información, consulte [Incorporación de Google como proveedor de identidades para los usuarios invitados de B2B](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

### <a name="microsoft-edge-mobile-support-for-conditional-access-and-single-sign-on-general-availability"></a>Compatibilidad de Microsoft Edge para dispositivos móviles con el acceso condicional y el inicio de sesión único (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** Acceso condicional  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Azure AD para Microsoft Edge en iOS y Android ahora admite acceso condicional e inicio de sesión único de Azure AD:

- **Inicio de sesión único (SSO) de Microsoft Edge:** El inicio de sesión único ahora está disponible en los clientes nativos (como Microsoft Outlook y Microsoft Edge) para todas las aplicaciones conectadas a Azure AD.

- **Acceso condicional de Microsoft Edge:** A través de las directivas de acceso condicional basado en aplicaciones, los usuarios deben usar exploradores protegidos por Microsoft Intune, como Microsoft Edge.

Para obtener más información sobre el acceso condicional y el inicio de sesión único con Microsoft Edge, consulte la entrada de blog sobre [compatibilidad de Microsoft Edge para dispositivos móviles con el acceso condicional y el inicio de sesión único ahora disponible con carácter general](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Microsoft-Edge-Mobile-Support-for-Conditional-Access-and-Single/ba-p/988179). Para obtener más información sobre cómo configurar las aplicaciones cliente con [acceso condicional basado en aplicaciones](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) o [acceso condicional basado en dispositivos](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices), consulte [Administración del acceso web mediante un explorador protegido por directivas de Microsoft Intune](https://docs.microsoft.com/intune/apps/app-configuration-managed-browser).

---

### <a name="azure-ad-entitlement-management-general-availability"></a>Administración de derechos de Azure AD (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Administración de derechos

La administración de derechos de Azure AD es una nueva característica de gobernanza de identidades, que ayuda a las organizaciones a administrar el ciclo de vida de identidad y acceso a escala. Esta nueva característica ayuda mediante la automatización de los flujos de trabajo de solicitud de acceso, las asignaciones de acceso, las revisiones y la expiración entre grupos, aplicaciones y sitios de SharePoint Online.

Con la administración de derechos de Azure AD, puede administrar el acceso de forma más eficaz tanto para los empleados como para los usuarios ajenos a la organización que han de acceder a esos recursos.

Para obtener más información, consulte [¿Qué es la administración de derechos de Azure AD?](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview#license-requirements)

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatización del aprovisionamiento de cuentas de usuario para estas aplicaciones SaaS recién admitidas

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros  

Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

[SAP Cloud Platform Identity Authentication Service](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-provisioning-tutorial), [SpaceIQ](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-provisioning-tutorial), [Miro](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-provisioning-tutorial), [Cloudgate](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-provisioning-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloudsuite-provisioning-tutorial), [OfficeSpace Software](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-software-provisioning-tutorial), [Priority Matrix](https://docs.microsoft.com/azure/active-directory/saas-apps/priority-matrix-provisioning-tutorial)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: noviembre de 2019

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En noviembre de 2019, hemos agregado estas 21 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[Airtable](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [Hootsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [Blue Access for Members (BAM)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [Bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [ResLife Portal](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal Single Sign On (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), [TeamsChamp](https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279), [Motus](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial), [MyAryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial), [BlueMail](https://loginself1.bluemail.me/), [Beedle](https://teams-web.beedle.co/#/), [Visma](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial), [OneDesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [Foko Retail](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [Qmarkets Idea & Innovation Management](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [Netskope User Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial), [uniFLOW Online](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial), [Claromentis](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial), [Jisc Student Voter Registration](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial), [e4enable](https://portal.e4enable.com/)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="new-and-improved-azure-ad-application-gallery"></a>La nueva y mejorada galería de aplicaciones de Azure AD

**Tipo:** Característica modificada  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Hemos actualizado la galería de aplicaciones de Azure AD para que le resulte más fácil encontrar aplicaciones integradas previamente que admitan el aprovisionamiento, OpenID Connect y SAML en su inquilino de Azure Active Directory.

Para obtener más información, consulte [Incorporación de una aplicación a un inquilino de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal).

---

### <a name="increased-app-role-definition-length-limit-from-120-to-240-characters"></a>Límite de longitud de definición de roles de aplicación aumentado de 120 a 240 caracteres

**Tipo:** Característica modificada  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Hemos escuchado de los clientes que el límite de longitud del valor de definición de rol de aplicación en algunas aplicaciones y servicios es demasiado corto en 120 caracteres. En respuesta, hemos aumentado la longitud máxima de la definición de valor de rol a 240 caracteres.

Para obtener más información sobre el uso de definiciones de roles específicas de la aplicación, consulte Para ver más ejemplos e información, consulte [Procedimiento para agregar roles de aplicación en la aplicación y recibirlos en el token](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps).

---
