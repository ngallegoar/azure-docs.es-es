---
title: Supervisión de identidad y acceso en Azure Security Center | Microsoft Docs
description: Aprenda a usar la funcionalidad de identidad y acceso de Azure Security Center para supervisar los problemas relacionados con la actividad de acceso e identidad de los usuarios.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2020
ms.author: memildin
ms.openlocfilehash: b36f52fd97a9f7ff294f14ec394ff9a9f607b5b0
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96187160"
---
# <a name="monitor-identity-and-access"></a>Supervisión de la identidad y el acceso

El perímetro de seguridad ha evolucionado de un perímetro de red a uno de identidad. Con este desarrollo, la seguridad se centra menos en proteger la red y más en administrar la seguridad de las aplicaciones, los datos y los usuarios.

Con la supervisión de las actividades y la configuración relacionada con la identidad, se pueden realizar acciones proactivas antes de que se produzca un incidente o acciones reactivas para detener intentos de ataque.

## <a name="what-identity-and-access-safeguards-does-security-center-provide"></a>¿Qué medidas de seguridad de identidad y acceso proporciona Security Center? 

Azure Security Center tiene dos controles de seguridad dedicados para asegurarse de que cumple los requisitos de identidad y seguridad de la organización: 

 - **Administración del acceso y los permisos**: le recomendamos que adopte el [modelo de acceso con privilegios mínimos](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) y que se asegure de conceder a los usuarios solo el acceso necesario para que puedan realizar sus tareas. Este control también incluye recomendaciones para implementar el [control de acceso basado en roles de Azure (Azure RBAC)](../role-based-access-control/overview.md) para controlar el acceso a los recursos.
 
 - **Habilitación de MFA**: con [MFA](https://www.microsoft.com/security/business/identity/mfa) habilitada, las cuentas son más seguras y los usuarios podrán seguir autenticándose en casi cualquier aplicación con inicio de sesión único.

### <a name="example-recommendations-for-identity-and-access"></a>Ejemplos de recomendaciones de identidad y acceso

Ejemplos de recomendaciones que puede ver en estos dos controles en la página de **Recomendaciones** de Security Center:

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción
- Debe designar un máximo de tres propietarios para la suscripción
- Las cuentas externas con permisos de lectura deben quitarse de la suscripción
- Debe quitar las cuentas en desuso de las suscripciones, ya que son cuentas que ya no son necesarias y que tienen bloqueada la opción de inicio de sesión en Azure Active Directory.

> [!TIP]
> Para más información sobre estas recomendaciones y otras que puede ver en estos controles, consulte [Recomendaciones de identidad y acceso](recommendations-reference.md#recs-identity).

### <a name="limitations"></a>Limitaciones

Existen algunas limitaciones para las protecciones de acceso e identidad de Security Center:

- Las recomendaciones de identidad no están disponibles para las suscripciones con más de 600 cuentas. En tales casos, estas recomendaciones se enumerarán en "evaluaciones no disponibles".
- Las recomendaciones de identidad no están disponibles para los agentes de administración del asociado del Proveedor de soluciones en la nube (CSP).
- Las recomendaciones de identidad no identifican las cuentas que se administran con un sistema de Privileged Identity Management (PIM). Si usa una herramienta PIM, podría observar resultados inexactos en el control **Manage access and permissions** (Administrar el acceso y los permisos).

## <a name="multi-factor-authentication-mfa-and-azure-active-directory"></a>Multi-Factor Authentication (MFA) y Azure Active Directory 

Habilitar MFA requiere [permisos de inquilino de Azure Active Directory (AD)](../active-directory/roles/permissions-reference.md).

- Si tiene una edición Premium de AD, habilite MFA mediante el [acceso condicional](../active-directory/conditional-access/concept-conditional-access-policy-common.md).
- Si usa la edición gratuita de AD, habilite **valores predeterminados de seguridad** tal y como se describe en la [documentación de Azure Active Directory](../active-directory/fundamentals/concept-fundamentals-security-defaults.md).

## <a name="identify-accounts-without-multi-factor-authentication-mfa-enabled"></a>Identificación de cuentas sin Multi-Factor Authentication (MFA) habilitado

Para ver qué cuentas no tienen MFA habilitado, use la siguiente consulta de Azure Resource Graph. La consulta devuelve todas los recursos (cuentas) incorrectos de la recomendación "MFA debe estar habilitado en las cuentas con permisos de propietario en la suscripción". 

1. Abra **Azure Resource Graph Explorer**.

    :::image type="content" source="./media/security-center-identity-access/opening-resource-graph-explorer.png" alt-text="Inicio de la página de recomendaciones de Azure Resource Graph Explorer**" :::

1. Escriba la siguiente consulta y seleccione **Ejecutar consulta**.

    ```kusto
    securityresources
     | where type == "microsoft.security/assessments"
     | where properties.displayName == "MFA should be enabled on accounts with owner permissions on your subscription"
     | where properties.status.code == "Unhealthy"
    ```

1. La propiedad `additionalData` revela la lista de identificadores de objeto de cuenta para las cuentas que no tienen MFA aplicado. 

    > [!NOTE]
    > Las cuentas se muestran como identificadores de objeto en lugar de nombres de cuenta para proteger la privacidad de los titulares.

> [!TIP]
> También puede usar el método [Assessments - Get](/rest/api/securitycenter/assessments/get) de la API REST de Security Center.


## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte el siguiente artículo:

- [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)