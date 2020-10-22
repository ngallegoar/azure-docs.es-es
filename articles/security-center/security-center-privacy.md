---
title: Administración de datos de usuario en Azure Security Center | Microsoft Docs
description: Aprenda a administrar los datos de usuario en Azure Security Center. La administración de los datos de usuario incluye la capacidad de acceder, eliminar o exportar datos.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: memildin
ms.openlocfilehash: 4e270185550fac5a06d800d74d5fcd751aceb25d
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92342236"
---
# <a name="manage-user-data-in-azure-security-center"></a>Administrar datos de usuario en Azure Security Center
En este artículo se proporciona información sobre cómo administrar los datos de usuario en Azure Security Center. La administración de los datos de usuario incluye la capacidad de acceder, eliminar o exportar datos.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede acceder a los datos del cliente dentro de la herramienta. Para obtener más información sobre el rol Administrador de cuenta, consulte [Roles integrados para el control de acceso basado en roles de Azure](../role-based-access-control/built-in-roles.md) para obtener más información sobre los roles Lector, Propietario y Colaborador. Consulte el artículo sobre los [administradores de la suscripción de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md).

## <a name="searching-for-and-identifying-personal-data"></a>Búsqueda e identificación de datos personales
Un usuario de Security Center puede ver sus datos personales a través de Azure Portal. En Security Center solo se almacenan los detalles de contacto de seguridad como direcciones de correo electrónico y números de teléfono. Para obtener más información, consulte [Provisión de detalles de contacto de seguridad en Azure Security Center](security-center-provide-security-contact-details.md).

En Azure Portal, un usuario puede ver las configuraciones de IP permitidas mediante el la característica de acceso Just-In-Time a la máquina virtual de Security Center. Para obtener más información, consulte [Administrar el acceso a máquina virtual mediante Just-In-Time](security-center-just-in-time.md).

En Azure Portal, un usuario puede ver las alertas de seguridad proporcionadas por Security Center, incluidos los detalles de atacantes y direcciones IP. Para obtener más información, consulte [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md).

## <a name="classifying-personal-data"></a>Clasificación de datos personales
No es necesario clasificar los datos personales que se encuentran en la característica de contactos de seguridad de Security Center. Los datos guardados son una dirección de correo electrónico (o varias) y un número de teléfono. Security Center valida los [datos de contacto](security-center-provide-security-contact-details.md).

No es necesario clasificar las direcciones IP y los números de puerto guardados por la característica [Just-In-Time](security-center-just-in-time.md) de Security Center.

Solo un usuario asignado al rol de Administrador puede clasificar los datos personales mediante la [visualización de alertas](security-center-managing-and-responding-alerts.md) en Security Center.

## <a name="securing-and-controlling-access-to-personal-data"></a>Protección y control del acceso a los datos personales
Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede tener acceso a los [datos de contacto de seguridad](security-center-provide-security-contact-details.md).

Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede tener acceso a las directivas [Just-In-Time](security-center-just-in-time.md).

Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede ver las [alertas](security-center-managing-and-responding-alerts.md).

## <a name="updating-personal-data"></a>Actualización de datos personales
Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede actualizar los [datos de contacto de seguridad](security-center-provide-security-contact-details.md) a través de Azure Portal.

Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede actualizar las [directivas Just-In-Time](security-center-just-in-time.md).

Un Administrador de cuenta no puede editar incidentes de alertas. Un [incidente de alerta](security-center-managing-and-responding-alerts.md) se considera datos de seguridad y es de solo lectura.

## <a name="deleting-personal-data"></a>Eliminación de datos personales
Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede eliminar los [datos de contacto de seguridad](security-center-provide-security-contact-details.md) a través de Azure Portal.

Un usuario de Security Center que tenga asignado el rol Propietario, Colaborador o Administrador de cuenta puede eliminar las [directivas Just-In-Time](security-center-just-in-time.md) a través de Azure Portal.

Un usuario de Security Center no puede eliminar los incidentes de alerta. Por motivos de seguridad, un [incidente de alerta](security-center-managing-and-responding-alerts.md) se considera datos de solo lectura.

## <a name="exporting-personal-data"></a>Exportación de datos personales
Un usuario de Security Center que tenga asignado el rol Lector, Propietario, Colaborador o Administrador de cuenta puede exportar los [datos de contacto de seguridad](security-center-provide-security-contact-details.md) si:

- Copia desde Azure Portal
- Ejecuta la llamada de API de REST de Azure, GET HTTP:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
  ```

Un usuario de Security Center que tenga asignado el rol Administrador de cuenta puede exportar las [directivas Just-In-Time](security-center-just-in-time.md) que contienen las direcciones IP si:

- Copia desde Azure Portal
- Ejecuta la llamada de API de REST de Azure, GET HTTP:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
  ```

Un Administrador de cuenta puede exportar los detalles de alerta si:

- Copia desde Azure Portal
- Ejecuta la llamada de API de REST de Azure, GET HTTP:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
  ```

Para obtener más información, consulte [Obtener alertas de seguridad (colección GET)](/previous-versions/azure/reference/mt704050(v=azure.100)).

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>Restricción del uso de datos personales para la generación de perfiles o marketing sin consentimiento
Un usuario de Security Center puede elegir no participar mediante la eliminación de sus [datos de contacto de seguridad](security-center-provide-security-contact-details.md).

Los [datos Just-In-Time](security-center-just-in-time.md) se consideran datos no identificables y se conservan durante un período de 30 días.

Los [datos de alertas](security-center-managing-and-responding-alerts.md) se consideran datos de seguridad y se conservan durante un período de dos años.

## <a name="auditing-and-reporting"></a>Informes y auditoría
Los registros de auditoría de contacto de seguridad, Just-In-Time y las actualizaciones de alertas se mantienen en [los registros de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md).