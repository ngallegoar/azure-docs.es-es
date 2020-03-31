---
title: Uso del informe de actividades para trasladar aplicaciones de AD FS a Azure Active Directory | Microsoft Docs
description: El informe de actividades de aplicaciones de Servicios de federación de Active Directory (AD FS) permite migrar rápidamente aplicaciones de AD FS a Azure Active Directory (Azure AD). Esta herramienta de migración para AD FS identifica la compatibilidad con Azure AD y proporciona instrucciones de migración.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/14/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 333e440fdd5f5062dda45fb12a83543c63e66c04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978032"
---
# <a name="use-the-ad-fs-application-activity-report-preview-to-migrate-applications-to-azure-ad"></a>Uso del informe de actividades de aplicaciones de AD FS (versión preliminar) para migrar aplicaciones a Azure AD

Muchas organizaciones usan Servicios de federación de Active Directory (AD FS) para proporcionar un inicio de sesión único a las aplicaciones en la nube. El traslado de aplicaciones de AD FS a Azure AD para la autenticación aporta importantes ventajas, especialmente en lo que respecta a la administración de costos y riesgos, a la productividad, al cumplimiento y a la gobernanza. Sin embargo, entender qué aplicaciones son compatibles con Azure AD e identificar los pasos de migración específicos puede llevar mucho tiempo.

El informe de actividades de aplicaciones de AD FS (versión preliminar) de Azure Portal permite identificar rápidamente qué aplicaciones se pueden migrar a Azure AD. Evalúa todas las aplicaciones de AD FS con respecto a la compatibilidad con Azure AD, comprueba si hay problemas y proporciona instrucciones sobre cómo preparar aplicaciones concretas para su migración. Con del informe de actividades de aplicaciones de AD FS puede hacer lo siguiente:

* **Descubrir aplicaciones de AD FS y definir el ámbito de su migración.** En el informe de actividades de aplicaciones de AD FS se enumeran todas las aplicaciones de AD FS de su organización y se indica su grado de preparación para la migración a Azure AD.
* **Establecer prioridades de aplicaciones para la migración.** Obtenga el número de usuarios únicos que han iniciado sesión en la aplicación en los últimos 1, 7 o 30 días para ayudar a determinar la importancia o el riesgo de migrar la aplicación.
* **Ejecutar pruebas de migración y solucionar problemas.** El servicio de informes ejecuta automáticamente las pruebas para determinar si una aplicación está preparada para la migración. Los resultados se muestran en el informe de actividades de aplicaciones de AD FS como estado de la migración. Si se identifican posibles problemas de migración, obtendrá instrucciones específicas sobre cómo solucionar los problemas.

Los datos de actividad de aplicaciones de AD FS están disponibles para los usuarios que tienen asignados cualquiera de estos roles de administrador: administrador global, lector de informes, lector de seguridad, administrador de aplicaciones o administrador de aplicaciones en la nube.

## <a name="prerequisites"></a>Prerrequisitos

* En estos momentos, su organización debe usar AD FS para acceder a las aplicaciones.
* Azure AD Connect Health debe estar habilitado en el inquilino de Azure AD.
   * [Más información sobre Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)
   * [Introducción a la configuración de Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-agent-install)

## <a name="discover-ad-fs-applications-that-can-be-migrated"></a>Descubrimiento de aplicaciones de AD FS que se pueden migrar 

El informe de actividades de aplicaciones de AD FS está disponible en Azure Portal, en los informes de Azure AD de **Usage & insights** (Uso e información). El informe de actividades de aplicaciones de AD FS analiza cada aplicación de AD FS para determinar si se puede migrar tal cual o si es necesario realizar una revisión adicional. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con un rol de administrador que tenga acceso a los datos de actividad de las aplicaciones de AD FS (administrador global, lector de informes, lector de seguridad, administrador de aplicaciones o administrador de aplicaciones en la nube).

2. Seleccione **Azure Active Directory** y, luego, **Aplicaciones empresariales**.

3. En **Actividad**, seleccione **Usage & Insights (Preview)** (Uso e información [Versión preliminar]) y, a continuación, seleccione **Actividad de la aplicación de AD FS** para abrir una lista de todas las aplicaciones de AD FS de la organización.

   ![Actividad de aplicaciones de AD FS](media/migrate-adfs-application-activity/adfs-application-activity.png)

4. Para cada aplicación de la lista de actividades de aplicaciones de AD FS, vea el **estado de migración**:

   * **Listo para migrar** significa que la configuración de la aplicación de AD FS es totalmente compatible con Azure AD y se puede migrar tal cual.

   * **Revisión necesaria** significa que algunas de las configuraciones de la aplicación se pueden migrar a Azure AD, pero debe revisar la configuración que no se puede migrar tal cual.

   * **Pasos adicionales necesarios** significa que Azure AD no admite parte de la configuración de la aplicación, por lo que no se puede migrar en su estado actual.

## <a name="evaluate-the-readiness-of-an-application-for-migration"></a>Evaluación de la preparación de una aplicación para su migración 

1. En la lista de actividades de aplicaciones de AD FS, haga clic en el estado en la columna **Estado de migración** para abrir los detalles de la migración. Verá un resumen de las pruebas de configuración superadas, junto con los posibles problemas de migración.

   ![Detalles de la migración](media/migrate-adfs-application-activity/migration-details.png)

2. Haga clic en un mensaje para abrir detalles adicionales de la regla de migración. Para obtener una lista completa de las propiedades probadas, vea la tabla [Pruebas de configuración de aplicaciones de AD FS](#ad-fs-application-configuration-tests), que aparece a continuación.

   ![Detalles de la regla de migración](media/migrate-adfs-application-activity/migration-rule-details.png)

### <a name="ad-fs-application-configuration-tests"></a>Pruebas de configuración de aplicaciones de AD FS

En la tabla siguiente se enumeran todas las pruebas de configuración que se realizan en aplicaciones de AD FS.

|Resultado  |Sin errores/Advertencia/Error  |Descripción  |
|---------|---------|---------|
|Test-ADFSRPAdditionalAuthenticationRules <br> Se detectó al menos una regla no migrable para AdditionalAuthentication.       | Aprobada/Con advertencias          | El usuario de confianza tiene reglas para solicitar Multi-Factor Authentication (MFA). Para desplazarse a Azure AD, traduzca dichas reglas en directivas de acceso condicional. Si usa una aplicación local de MFA, se recomienda que se traslade a Azure MFA. [Obtenga más información sobre el acceso condicional](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).        |
|Test-ADFSRPAdditionalWSFedEndpoint <br> El usuario de confianza tiene establecido el valor de AdditionalWSFedEndpoint establecido en true.       | Sin errores/Error          | El usuario de confianza de AD FS permite varios puntos de conexión de aserción de WS-FED. Actualmente, Azure AD solo admite una. Si tiene un escenario en el que este resultado bloquea la migración, [háganoslo saber](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695621-allow-multiple-ws-fed-assertion-endpoints).     |
|Test-ADFSRPAllowedAuthenticationClassReferences <br> El usuario de confianza ha establecido el valor de AllowedAuthenticationClassReferences.       | Sin errores/Error          | Este valor de AD FS permite especificar si la aplicación se configura para permitir solo determinados tipos de autenticación. Se recomienda usar acceso condicional para lograr esta funcionalidad.  Si tiene un escenario en el que este resultado bloquea la migración, [háganoslo saber](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695672-allow-in-azure-ad-to-specify-certain-authentication).  [Obtenga más información sobre el acceso condicional](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).          |
|Test-ADFSRPAlwaysRequireAuthentication <br> AlwaysRequireAuthenticationCheckResult      | Sin errores/Error          | Este valor de AD FS permite especificar si la aplicación se configura para omitir las cookies de SSO y **solicitar siempre la autenticación**. En Azure AD, puede administrar la sesión de autenticación mediante directivas de acceso condicional para lograr un comportamiento similar. [Obtenga más información sobre cómo configurar la sesión de autenticación con acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).          |
|Test-ADFSRPAutoUpdateEnabled <br> El usuario de confianza tiene el valor de AutoUpdateEnabled en true.       | Aprobada/Con advertencias          | Este valor de AD FS permite especificar si AD FS se configura para actualizar automáticamente la aplicación en función de los cambios en los metadatos de federación. Azure AD no admite esta configuración actualmente, pero no debería impedir la migración de la aplicación a Azure AD.           |
|Test-ADFSRPClaimsProviderName <br> El usuario de confianza tiene varios parámetros ClaimsProviders habilitados.       | Sin errores/Error          | Este valor de AD FS llama a los proveedores de identidades de los que el usuario de confianza acepta notificaciones. En Azure AD, puede habilitar la colaboración externa con Azure AD B2B. [Obtenga más información sobre Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b).          |
|Test-ADFSRPDelegationAuthorizationRules      | Sin errores/Error          | La aplicación tiene definidas reglas de autorización de delegación personalizadas. Se trata de un concepto de WS-Trust que Azure AD admite mediante protocolos de autenticación modernos, como OpenID Connect y OAuth 2.0. [Obtenga más información sobre la plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc).          |
|Test-ADFSRPImpersonationAuthorizationRules       | Aprobada/Con advertencias          | La aplicación tiene definidas reglas de autorización de suplantación personalizadas. Se trata de un concepto de WS-Trust que Azure AD admite mediante protocolos de autenticación modernos, como OpenID Connect y OAuth 2.0. [Obtenga más información sobre la plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc).          |
|Test-ADFSRPIssuanceAuthorizationRules <br> Se detectó al menos una regla no migrable para IssuanceAuthorization.       | Aprobada/Con advertencias          | La aplicación tiene reglas de autorización de emisión personalizadas definidas en AD FS. Azure AD admite esta funcionalidad con el acceso condicional de Azure AD. [Más información sobre el acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) <br> También puede restringir el acceso a la aplicación por usuario o grupos asignados a la aplicación. [Obtenga más información sobre cómo asignar usuarios y grupos para acceder a las aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups).            |
|Test-ADFSRPIssuanceTransformRules <br> Se detectó al menos una regla no migrable para IssuanceTransform.       | Aprobada/Con advertencias          | La aplicación tiene reglas de autorización de transformación personalizadas definidas en AD FS. Azure AD admite la personalización de las notificaciones emitidas en el token. Para obtener más información, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).           |
|Test-ADFSRPMonitoringEnabled <br> El usuario de confianza tiene el valor de MonitoringEnabled establecido en true.       | Aprobada/Con advertencias          | Este valor de AD FS permite especificar si AD FS se configura para actualizar automáticamente la aplicación en función de los cambios en los metadatos de federación. Azure AD no admite esta configuración actualmente, pero no debería impedir la migración de la aplicación a Azure AD.           |
|Test-ADFSRPNotBeforeSkew <br> NotBeforeSkewCheckResult      | Aprobada/Con advertencias          | AD FS permite un desfase horario basado en las horas de NotBefore y NotOnOrAfter del token de SAML. Azure AD controla esto automáticamente de forma predeterminada.          |
|Test-ADFSRPRequestMFAFromClaimsProviders <br> El usuario de confianza tiene el valor de RequestMFAFromClaimsProviders establecido en true.       | Aprobada/Con advertencias          | Este valor de AD FS determina el comportamiento de MFA cuando el usuario procede de un proveedor de notificaciones diferente. En Azure AD, puede habilitar la colaboración externa con Azure AD B2B. Luego, puede aplicar directivas de acceso condicional para proteger el acceso de invitado. Obtenga más información sobre [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b) y [Acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview).          |
|Test-ADFSRPSignedSamlRequestsRequired <br> El usuario de confianza tiene el valor de SignedSamlRequestsRequired establecido en true.       | Sin errores/Error          | La aplicación se configura en AD FS para comprobar la firma de la solicitud SAML. Azure AD acepta una solicitud SAML firmada, pero no comprobará la firma. Azure AD tiene distintos métodos para protegerse frente a llamadas malintencionadas. Por ejemplo, Azure AD usa las direcciones URL de respuesta configuradas en la aplicación para validar la solicitud SAML. Azure AD solo enviará un token a las direcciones URL de respuesta configuradas para la aplicación. Si tiene un escenario en el que este resultado bloquea la migración, [háganoslo saber](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/13394589-saml-signature).          |
|Test-ADFSRPTokenLifetime <br> TokenLifetimeCheckResult        | Aprobada/Con advertencias         | La aplicación está configurada para una duración de token personalizada. El valor predeterminado de AD FS es de una hora. Azure AD admite esta funcionalidad mediante el acceso condicional. Para obtener más información, consulte [Configuración de la administración de las sesiones de autenticación con el acceso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).          |
|El usuario de confianza está configurado para cifrar notificaciones. Esta configuración es compatible con Azure AD.       | Pass (pasado)          | Con Azure AD puede cifrar el token enviado a la aplicación. Para obtener más información, consulte [Configuración del cifrado de tokens SAML de Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).          |
|EncryptedNameIdRequiredCheckResult      | Sin errores/Error          | La aplicación está configurada para cifrar la notificación nameID en el token SAML. Con Azure AD, puede cifrar todo el token enviado a la aplicación. Todavía no se admite el cifrado de notificaciones específicas. Para obtener más información, consulte [Configuración del cifrado de tokens SAML de Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).         |

## <a name="check-the-results-of-claim-rule-tests"></a>Comprobación de los resultados de las pruebas de reglas de notificaciones

Si ha configurado una regla de notificaciones para la aplicación en AD FS, la experiencia proporcionará un análisis granular para todas las reglas de notificaciones. Verá qué reglas de notificaciones se pueden trasladar a Azure AD y cuáles deben revisarse con más detalle.

1. En la lista de actividades de aplicaciones de AD FS, haga clic en el estado en la columna **Estado de migración** para abrir los detalles de la migración. Verá un resumen de las pruebas de configuración superadas, junto con los posibles problemas de migración.

2. En la página **Detalles de la regla de migración**, expanda los resultados para mostrar los detalles de los posibles problemas de migración y obtener instrucciones adicionales. Para ver una lista detallada de todas las reglas de notificaciones probadas, consulte la tabla de abajo [Comprobación de los resultados de las pruebas de reglas de notificaciones](#check-the-results-of-claim-rule-tests) de abajo.

   En el ejemplo siguiente se muestran los detalles de la regla de migración para la regla de IssuanceTransform. Muestra las partes específicas de la notificación que deben revisarse y abordarse antes de poder migrar la aplicación a Azure AD.

   ![Instrucciones adicionales de regla de migración](media/migrate-adfs-application-activity/migration-rule-details-guidance.png)

### <a name="claim-rule-tests"></a>Pruebas de reglas de notificaciones

En la tabla siguiente se enumeran todas las pruebas de reglas de notificaciones que se realizan en aplicaciones de AD FS.

|Propiedad  |Descripción  |
|---------|---------|
|UNSUPPORTED_CONDITION_PARAMETER      | La instrucción de condición utiliza expresiones regulares para evaluar si la notificación coincide con un patrón determinado.  Para lograr una funcionalidad similar en Azure AD, puede usar la transformación predefinida como IfEmpty(), StartWith() o Contains(). Para obtener más información, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|UNSUPPORTED_CONDITION_CLASS      | La instrucción de condición tiene varias condiciones que deben evaluarse antes de ejecutar la instrucción de emisión. Azure AD puede admitir esta funcionalidad con las funciones de transformación de la notificación, donde puede evaluar varios valores de notificaciones.  Para obtener más información, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|UNSUPPORTED_RULE_TYPE      | No se pudo reconocer la regla de notificaciones. Para obtener más información sobre cómo configurar notificaciones en Azure AD, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|CONDITION_MATCHES_UNSUPPORTED_ISSUER      | La instrucción de condición usa un emisor que no se admite en Azure AD. Actualmente, Azure AD no procesa las notificaciones de los almacenes que no sean de Active Directory o Azure AD. Si esto le impide migrar aplicaciones a Azure AD, [infórmenos](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire).         |
|UNSUPPORTED_CONDITION_FUNCTION      | La instrucción de condición utiliza una función de agregado para emitir o agregar una única notificación sin tener en cuenta el número de coincidencias.  En Azure AD puede evaluar el atributo de un usuario para decidir qué valor utilizar para la notificación con funciones como IfEmpty(), StartWith() o Contains(). Para obtener más información, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|RESTRICTED_CLAIM_ISSUED      | La instrucción de condición usa una notificación que está restringida en Azure AD. Es posible que pueda emitir una notificación restringida, pero no puede modificar su origen ni aplicar ninguna transformación. Para obtener más información, consulte [Personalice las notificaciones emitidas en tokens para una aplicación específica en Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).          |
|EXTERNAL_ATTRIBUTE_STORE      | La instrucción de emisión usa un almacén de atributos que no es Active Directory. Actualmente, Azure AD no procesa las notificaciones de los almacenes que no sean de Active Directory o Azure AD. Si esto le impide migrar aplicaciones a Azure AD, [háganoslo saber](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire).          |
|UNSUPPORTED_ISSUANCE_CLASS      | La instrucción de emisión usa AAD para agregar notificaciones al conjunto de notificaciones entrantes. En Azure AD esto puede configurarse como varias transformaciones de notificaciones.  Para obtener más información, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).         |
|UNSUPPORTED_ISSUANCE_TRANSFORMATION      | La instrucción de emisión utiliza expresiones regulares para transformar el valor de la notificación que se va a emitir. Para lograr una funcionalidad similar en Azure AD, puede usar la transformación predefinida como Extract(), Trim() o ToLower. Para obtener más información, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |


## <a name="next-steps"></a>Pasos siguientes

- [Vídeo: Cómo usar el informe de actividades de AD FS para migrar una aplicación](https://www.youtube.com/watch?v=OThlTA239lU)
- [Administración de aplicaciones con Azure Active Directory](what-is-application-management.md)
- [Administración del acceso a aplicaciones](what-is-access-management.md)
- [Federación de Azure AD Connect](../hybrid/how-to-connect-fed-whatis.md)
