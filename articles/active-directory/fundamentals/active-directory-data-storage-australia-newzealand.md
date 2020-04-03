---
title: 'Almacenamiento de datos de identidad para clientes australianos: Azure AD'
description: Obtenga información sobre dónde almacena Azure Active Directory los datos relacionados con la identidad de los clientes australianos.
services: active-directory
author: msaburnley
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0dc659f3d580646c1e86e5e97940268311e5546f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460541"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Almacenamiento de datos de identidad para clientes australianos y neozelandeses en Azure Active Directory

Azure AD almacena los datos de identidad en una ubicación geográfica en función de la dirección que proporcione la organización al suscribirse a un servicio de Microsoft Online, como Office 365 y Azure. Para más información sobre dónde se almacenan los datos de identidad del cliente, puede consultar la sección [Dónde se encuentran tus datos](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) del Centro de confianza de Microsoft.

> [!NOTE]
> Los servicios y las aplicaciones que se integran en Azure AD tienen acceso a datos de identidad del cliente. Evalúe cada servicio y aplicación que use para determinar la manera en que cada servicio y aplicación específico procesa los datos de identidad del cliente, y si cumple los requisitos de almacenamiento de datos de la empresa. Para obtener más información sobre la residencia de los datos de los servicios de Microsoft, consulte la sección Dónde se encuentran sus datos del Centro de confianza de Microsoft.

En el caso de los clientes que han proporcionado direcciones de Australia y Nueva Zelanda y utilizan la edición gratuita de Azure AD, Azure AD mantiene la información de identificación personal en reposo en centros de datos australianos. 

Todos los demás servicios de Azure AD prémium almacenan los datos del cliente en centros de datos globales. Para localizar el centro de datos de un servicio, consulte [Azure Active Directory: Dónde se encuentran sus datos](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located).

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Microsoft Azure Multi-Factor Authentication (MFA)

El servicio MFA de Azure AD almacena datos de identidad del cliente en centros de datos globales en reposo. Para más información sobre los datos del usuario que Azure MFA basado en la nube recopila y almacena, consulte [Recopilación de datos de usuario de Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-data-residency). Si los clientes usan MFA, sus datos se almacenarán fuera de los centros de datos en reposo de Australia. 

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre cualquiera de las características y funcionalidades que se han descrito anteriormente, consulte estos artículos:
- [¿Qué es Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
