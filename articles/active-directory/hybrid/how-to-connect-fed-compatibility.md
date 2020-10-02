---
title: Lista de compatibilidad de federación de Azure AD
description: Esta página presenta proveedores de identidades de terceros que se pueden utilizar para implementar el inicio de sesión único.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 895e7347feea3190ba9bdc273200d2985dfaa0d1
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661819"
---
# <a name="azure-ad-federation-compatibility-list"></a>Lista de compatibilidad de federación de Azure AD
Azure Active Directory brinda mayor seguridad en el acceso a la aplicación e inicio de sesión único para Microsoft 365 y otros servicios en línea de Microsoft para implementaciones híbridas y solo en la nube sin que sea necesaria ninguna solución de terceros. Microsoft 365, al igual que la mayoría de los servicios en línea de Microsoft, está integrado con Azure Active Directory para autorización, autenticación y servicios de directorio. Azure Active Directory también proporciona inicio de sesión único a miles de aplicaciones SaaS y aplicaciones web locales. Consulte la [galería de aplicaciones](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) de Azure Active Directory para ver las aplicaciones SaaS compatibles. 

## <a name="idp-validation"></a>Validación de IDP
Si su organización usa una solución de federación de terceros, puede configurar el inicio de sesión único para los usuarios de Active Directory local con los servicios en línea de Microsoft, como Microsoft 365, siempre que la solución de federación de terceros sea compatible con Azure Active Directory.  Si tiene preguntas acerca de la compatibilidad, póngase en contacto con su proveedor de identidades.  Si desea ver una lista de proveedores de identidades cuya compatibilidad con Azure AD se ha probado con anterioridad por parte de Microsoft, consulte [Documentos de compatibilidad del proveedor de identidades de Azure AD](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Microsoft ya no proporciona pruebas de validación a proveedores de identidad independientes para comprobar la compatibilidad con Azure Active Directory. Si desea probar la interoperabilidad del producto, consulte estas otras [directrices](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Pasos siguientes

- [Integración de los directorios locales con Azure Active Directory](whatis-hybrid-identity.md)
- [Azure AD Connect y la federación](how-to-connect-fed-whatis.md)
