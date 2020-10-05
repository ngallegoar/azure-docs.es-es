---
title: 'Azure Security Benchmark v2: administración de identidades'
description: Administración de identidades de Azure Security Benchmark v2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: bcd88f9f21c68f7f6cdda7299ac97d67e97dc009
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059097"
---
# <a name="security-control-identity-management"></a>Control de seguridad: Administración de identidades

La administración de identidades abarca los controles para establecer una identidad segura y controles de acceso mediante Azure Active Directory. Esto incluye el uso del inicio de sesión único, las autenticaciones seguras, las identidades administradas y los principios de servicio para las aplicaciones, el acceso condicional y la supervisión de las anomalías de la cuenta.

## <a name="id-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>ID-1: Unificación en Azure Active Directory como sistema central de identidad y autenticación

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-1 | 16.1, 16.2, 16.4, 16.5 | IA-2, IA-8, AC-2, AC-3 |

Azure Active Directory (Azure AD) es un servicio de administración de identidades y acceso predeterminado de Azure. Debe unificar en Azure AD para regir la administración de identidades y accesos de la organización en:
- Recursos de Microsoft Cloud, como Azure Portal, Azure Storage, Azure Virtual Machines (Linux y Windows), Azure Key Vault, PaaS y aplicaciones SaaS.

- Los recursos de su organización, como aplicaciones en Azure o los recursos de la red corporativa.

La protección de Azure AD debe tener máxima prioridad en la práctica de seguridad en la nube de su organización. Azure AD proporciona una puntuación de seguridad de la identidad para ayudarle a evaluar la posición de seguridad de las identidades en relación con los procedimientos recomendados de Microsoft. Use la puntuación para medir el grado de coincidencia de la configuración con los procedimientos recomendados y para hacer mejoras en la posición de seguridad.

Nota: Azure AD admite proveedores de identidades externos, que permiten a los usuarios que no tienen un cuenta de Microsoft iniciar sesión en sus aplicaciones y recursos con su identidad externa.

- [Inquilinos en Azure AD](../../active-directory/develop/single-and-multi-tenant-apps.md)

- [Procedimiento para crear y configurar una instancia de Azure AD](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Defina los inquilinos de Azure AD](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)  

- [Uso de proveedores de identidades externos para una aplicación](/azure/active-directory/b2b/identity-providers)

- [¿Qué es la puntuación de seguridad de la identidad en Azure AD?](../../active-directory/fundamentals/identity-secure-score.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Identidad y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys) 

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-2-manage-application-identities-securely-and-automatically"></a>ID-2: Administración de identidades de aplicaciones de forma segura y automática

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-2 | N/D | AC-2, AC-3, IA-2, IA-4, IA-9 |

En el caso de cuentas no humanas, como servicios o automatización, use identidades administradas de Azure, en lugar de crear una cuenta de usuario más eficaz para acceder a los recursos o ejecutar código. Las identidades administradas de Azure le permiten autenticarse en los servicios y recursos de Azure que admiten la autenticación de Azure AD. La autenticación se habilita mediante reglas de concesión de acceso predefinidas que permiten evitar las credenciales codificadas de forma rígida en los archivos de código fuente o de configuración. 

En el caso de los servicios que no admiten identidades administradas, use Azure AD para crear entidades de servicio con permisos restringidos en el nivel de recurso.  Se recomienda configurar entidades de servicio con credenciales de certificado y revertir a secretos de cliente. En ambos casos, Azure Key Vault se puede usar junto con las identidades administradas de Azure, de modo que el entorno en tiempo de ejecución (por ejemplo, una función de Azure) pueda recuperar la credencial del almacén de claves.

- [Identidades administradas de Azure](../../active-directory/managed-identities-azure-resources/overview.md)

- [Servicios que admiten identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Entidad de servicio de Azure](/powershell/azure/create-azure-service-principal-azureps)

- [Creación de una entidad de servicio con certificado](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

Use Azure Key Vault para el registro de entidades de seguridad: authentication#authorize-a-security-principal-to-access-key-vault

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Identidad y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>ID-3: Uso del inicio de sesión único de Azure AD para acceder a las aplicaciones

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-3 | 4.4. | IA-2, IA-4 |

Azure AD proporciona administración de identidades y acceso a los recursos de Azure, las aplicaciones en la nube y las aplicaciones locales. La administración de identidades y acceso se aplica a las identidades empresariales, como los empleados, así como a las identidades externas, como asociados y proveedores.

Use el inicio de sesión único de Azure AD para administrar y proteger el acceso a los datos y los recursos de su organización locales y en la nube. Conecte todos los usuarios, las aplicaciones y los dispositivos a Azure AD para un acceso seguro y sin problemas, y para conseguir más visibilidad y control. 

- [Información sobre el inicio de sesión único (SSO) en aplicaciones con Azure AD](../../active-directory/manage-apps/what-is-single-sign-on.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Identidad y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>ID-4: Uso de controles con autenticación multifactor sólida para todo el acceso basado en Azure Active Directory

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-4 | 4.2, 4.4 4.5, 11.5, 12.11, 16.3 | AC-2, AC-3, IA-2, IA-4 |

Azure AD admite controles de autenticación sólida a través de la autenticación multifactor (MFA) y métodos seguros sin contraseña.  
- Autenticación multifactor: habilite la MFA de Azure AD y siga las recomendaciones de administración de identidades y acceso de Azure Security Center para la configuración de MFA. La MFA se puede exigir a todos los usuarios, a usuarios concretos o a nivel de cada usuario en función de los factores de riesgo y las condiciones de inicio de sesión. 

- Autenticación sin contraseña: hay tres opciones de autenticación sin contraseña disponibles, a saber, Windows Hello para empresas, aplicación Microsoft Authenticator y métodos de autenticación locales, como las tarjetas inteligentes. 

Para administradores y usuarios con privilegios, asegúrese de que se usa el nivel más alto del método de autenticación sólida, seguido de la implementación de la directiva de autenticación sólida adecuada para otros usuarios.

Si la autenticación con contraseña heredada todavía se usa para la autenticación de Azure AD, tenga en cuenta que las cuentas solo en la nube (cuentas de usuario creadas directamente en Azure) tienen una directiva de contraseñas de línea de base predeterminada. Además, las cuentas híbridas (cuentas de usuario que proceden de Active Directory local) siguen las directivas de contraseñas locales. Cuando se usa la autenticación con contraseña, Azure AD proporciona una funcionalidad de protección de contraseña que impide que los usuarios establezcan contraseñas fáciles de adivinar. Microsoft proporciona una lista global de contraseñas prohibidas que se actualiza en función de la telemetría, y los clientes pueden ampliarla en función de sus necesidades (por ejemplo, personalización de marca, referencias culturales, etc.). Esta protección de contraseñas se puede usar para las cuentas híbridas y solo en la nube. 

Nota: La autenticación basada solo en las credenciales de contraseña es susceptible a métodos de ataque populares. Para una mayor seguridad, use una autenticación sólida, como MFA y una directiva de contraseñas segura. En el caso de las aplicaciones de terceros y los servicios de Marketplace que pueden tener contraseñas predeterminadas, debe cambiarlas durante la configuración inicial del servicio. 

- [Procedimiento para habilitar la MFA en Azure](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Introducción a las opciones de autenticación sin contraseña de Azure Active Directory](../../active-directory/authentication/concept-authentication-passwordless.md)

- [Directiva de contraseñas predeterminada de Azure AD](../../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts)

- [Eliminación de contraseñas incorrectas mediante la Protección con contraseña de Azure AD](../../active-directory/authentication/concept-password-ban-bad.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Identidad y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-5-monitor-and-alert-on-account-anomalies"></a>ID-5: Supervisión y alerta de anomalías de cuenta

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-5 | 4.8, 4.9, 16.12, 16.13 | AC-2, AC-3, AC-7, AU-6 |

Azure AD proporciona los siguientes orígenes de datos: 
-   Inicios de sesión: el informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.

-   Registros de auditoría: proporcionan rastreabilidad mediante los registros de todos los cambios realizados mediante diversas características de Azure AD. Entre los ejemplos de registros de auditoría de los cambios registrados destacan agregar o eliminar usuarios, aplicaciones, grupos, roles y directivas.

-   Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

-   Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Estos orígenes de datos se pueden integrar con Azure Monitor, Azure Sentinel o sistemas SIEM de terceros.

Azure Security Center también puede alertar sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos y cuentas en desuso en la suscripción. 

Azure Advanced Threat Protection (AATP) es una solución de seguridad que puede usar señales de Active Directory locales para identificar, detectar e investigar amenazas avanzadas, identidades en riesgo y acciones internas malintencionadas.

- [Informes de actividad de auditoría en Azure AD](../../active-directory/reports-monitoring/concept-audit-logs.md)

- [Visualización de los inicios de sesión de riesgo de Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Procedimiento para identificar usuarios de Azure AD marcados por una actividad de riesgo](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Supervisión de la actividad de identidad y acceso de los usuarios en Azure Security Center](../../security-center/security-center-identity-access.md)

- [Alertas del módulo de protección de inteligencia sobre amenazas de Azure Security Center](//azure/security-center/alerts-reference)

- [Integración de los registros de actividad de Azure en Azure Monitor](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Conectar datos de Azure AD Identity Protection](../../sentinel/connect-azure-ad-identity-protection.md)

- [Azure Advanced Threat Protection](/azure-advanced-threat-protection/what-is-atp)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-6-restrict-azure-resource-access-based-on-conditions"></a>ID-6: Restricción del acceso a recursos de Azure en función de las condiciones

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-6 | N/D | AC-2, AC-3 |

Use el acceso condicional de Azure AD para un control de acceso más granular basado en condiciones definidas por el usuario, como requerir inicios de sesión de usuario desde determinados intervalos IP para usar MFA. También se puede usar una administración de sesión de autenticación granular mediante la directiva de acceso condicional de Azure AD para distintos casos de uso. 

- [Introducción al acceso condicional de Azure](../../active-directory/conditional-access/overview.md)

- [Directivas de acceso condicional habituales](../../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Frecuencia de inicio de sesión de usuario](../../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Identidad y claves](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="id-7-eliminate-unintended-credential-exposure"></a>ID-7: Elimine la exposición de credenciales no intencionada

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-7 | 18.1, 18.7 | IA-5 |

Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-8-secure-user-access-to-legacy-applications"></a>ID-8: Protección del acceso de los usuarios a las aplicaciones heredadas

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| ID-8 | 14.6 | AC-2, AC-3, SC-11 |

Asegúrese de que tiene controles de acceso modernos y supervisión de sesiones para las aplicaciones heredadas y los datos que almacenan y procesan. Mientras que las VPN suelen usarse para acceder a las aplicaciones heredadas, a menudo tienen un control de acceso básico y una supervisión de sesión limitada.

Azure AD Application Proxy permite publicar aplicaciones locales heredadas en usuarios remotos con el inicio de sesión único (SSO) mientras se valida explícitamente la confiabilidad de los usuarios y dispositivos remotos con el acceso condicional de Azure AD. 

Como alternativa, Microsoft Cloud App Security es un servicio de agente de seguridad de acceso a la nube (CASB) que puede proporcionar controles para supervisar las sesiones de aplicación de un usuario y las acciones de bloqueo, para aplicaciones locales heredadas y aplicaciones de software como servicio (SaaS) en la nube. 

- [Azure AD Application Proxy](../../active-directory/manage-apps/application-proxy.md#what-is-application-proxy)

- [Procedimientos recomendados de Microsoft Cloud App Security](/cloud-app-security/best-practices)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Seguridad de las aplicaciones y DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

