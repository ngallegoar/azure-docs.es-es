---
title: 'Licencia para el autoservicio de restablecimiento de contraseña: Azure Active Directory'
description: Obtenga información acerca de los requisitos de licencias del autoservicio de restablecimiento de contraseña de Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45ca11af061e37cf4f804ce2d7ceed72a9448294
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393079"
---
# <a name="licensing-requirements-for-azure-active-directory-self-service-password-reset"></a>Requisitos de licencias del autoservicio de restablecimiento de contraseña de Azure Active Directory

Para reducir las llamadas al departamento de soporte técnico y la pérdida de productividad cuando un usuario no puede iniciar sesión en su dispositivo o en una aplicación, las cuentas de usuario de Azure Active Directory (Azure AD) se pueden habilitar para el autoservicio de restablecimiento de contraseña (SSPR). Entre las características que componen SSPR se incluyen el cambio, restablecimiento, desbloqueo y escritura diferida de contraseñas en un directorio local. Las características básicas de SSPR están disponibles para Office 365 y para todos los usuarios de Azure AD sin costo.

En este artículo se detallan las diferentes formas en las que se puede obtener la licencia del autoservicio de restablecimiento de contraseña y cómo se puede usar. Para obtener información específica sobre precios y facturación, consulte [Página de precios de Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Comparación de ediciones y características

Se otorgan licencias de SSPR por usuario. Para garantizar el cumplimiento, se requiere que las organizaciones asignen la licencia adecuada a sus usuarios.

En la tabla siguiente se describen los distintos escenarios de SSPR para el cambio, restablecimiento o escritura diferida local de contraseñas y las SKU que proporcionan la característica.

| Característica | Azure AD Free | Office 365 Empresa Premium | Microsoft 365 Empresa | Azure AD Premium (P1 o P2) |
| --- |:---:|:---:|:---:|:---:|
| **Cambio de la contraseña del usuario solo en la nube**<br />Cuando un usuario de Azure AD conoce su contraseña y desea cambiarla por una nueva. | ● | ● | ● | ● |
| **Restablecimiento de la contraseña del usuario solo en la nube**<br />Cuando un usuario de Azure AD ha olvidado su contraseña y necesita restablecerla. | | ● | ● | ● |
| **Cambio de contraseña de usuario híbrido o restablecimiento con escritura diferida local**<br />Cuando un usuario de Azure AD que se sincroniza desde un directorio local mediante Azure AD Connect desea cambiar o restablecer su contraseña y también volver a escribir la nueva contraseña en el entorno local. | | | ● | ● |

> [!WARNING]
> Los planes de licencias independientes de Office 365 no admiten SSPR con escritura diferida local. Los planes de licencias de Office 365 requieren Azure AD Premium P1, Premium P2 o Microsoft 365 Empresa para que esta funcionalidad funcione.

Para obtener información adicional sobre licencias, incluidos los costos, consulte las siguientes páginas:

* [Precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Características y funcionalidades de Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 Empresa](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Habilitar licencias basadas en grupos o usuarios

Azure AD admite licencias basadas en grupos. Los administradores pueden asignar licencias en bloque a un grupo de usuarios en lugar de asignarlas una a una. Para más información, consulte [Asignar, comprobar y resolver los problemas con licencias](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Algunos servicios de Microsoft no están disponibles en todas las ubicaciones. Antes de poder asignar una licencia a un usuario, el administrador tiene que especificar la propiedad **Ubicación de uso** en el usuario. La asignación de licencias puede hacerse en la sección **Usuario** > **Perfil** > **Configuración** de Azure Portal. *Cuando se utiliza la asignación de licencias de grupo, los usuarios sin ubicación de uso especificada heredan la ubicación del directorio.*

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con SSPR, complete el siguiente tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Habilitación del autoservicio de restablecimiento de contraseña (SSPR)](tutorial-enable-sspr.md)
