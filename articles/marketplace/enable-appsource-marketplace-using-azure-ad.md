---
title: Habilitar una descripción de Microsoft AppSource y Azure Marketplace mediante Azure Active Directory | Azure
description: Habilite un tipo de aplicación mediante Azure Active Directory en Azure Marketplace y AppSource para los anunciantes de aplicaciones y servicios.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: dsindona
ms.openlocfilehash: 45855038e60dcdc3be4f98cfdceed69df5e8c946
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286326"
---
# <a name="enable-an-appsource-and-marketplace-listing-by-using-azure-active-directory"></a>Habilitar una descripción de AppSource y Marketplace mediante Azure Active Directory

 Azure Active Directory (Azure AD) es un servicio de identidad en la nube que permite la autenticación con una cuenta de Microsoft. Azure AD usa marcos estándar del sector. [Más información acerca de Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="azure-ad-benefits"></a>Ventajas de Azure AD

Los clientes de Microsoft AppSource y Azure Marketplace usan las experiencias del producto para buscar los catálogos de aplicaciones. Estas acciones requieren que los clientes inicien sesión en el producto. La integración de Azure AD proporciona las ventajas siguientes:

- Involucración más rápida y una experiencia de cliente optimizada
- Inicio de sesión único (SSO) para millones de usuarios de empresas
- Experiencia de inicio de sesión coherente en todas las aplicaciones que hayan publicado diferentes partners
- Opción de autenticación escalable y multiplataforma para sus aplicaciones móviles y en la nube

## <a name="offers-that-require-azure-ad"></a>Ofertas que requieren Azure AD

Las distintas [opciones de aplicaciones y tipos de ofertas](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) para AppSource y Azure Marketplace tienen requisitos distintos para implementarse en Azure AD. Consulte la tabla siguiente para más detalles:

| **Tipo de oferta**    | **¿Se requiere el SSO de Azure AD?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Ponerse en contacto conmigo | Versión de prueba | Versión de prueba | Transacción |
| Máquina virtual | N/D | No | No | No |
| Azure Apps (plantilla de solución)  | N/D | N/D | N/D | N/D |
| Aplicaciones administradas  | N/D | N/D | N/D | No |
| SaaS  | No | Sí | Sí | Sí |
| Contenedores  | N/D | N/D | N/D | No |
| Servicios de consultoría  | No | N/D | N/D | N/D |

Para obtener más información acerca de los requisitos técnicos de SaaS, consulte la [Guía de publicación de ofertas de aplicaciones de SaaS](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="azure-ad-integration"></a>Integración de Azure AD

- Para obtener información sobre cómo habilitar el inicio de sesión único mediante la integración de Azure AD en su descripción, consulte [Azure Active Directory para desarrolladores]( https://aka.ms/aaddev).
- Para conocer más detalles sobre el inicio de sesión único de Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="enable-a-trial-listing"></a>Habilitar una descripción de prueba

Automatizar la instalación de cliente puede aumentar la probabilidad de conversión. Cuando el cliente selecciona la descripción de prueba y se redirecciona al entorno de prueba, usted puede configurar el cliente directamente, sin necesidad de pasos de inicio de sesión adicionales.

Durante la autenticación, Azure AD envía un token a la aplicación u oferta. La información de usuario proporcionada por el token permite crear una cuenta de usuario en la aplicación u oferta. Para obtener más información, consulte los [tokens de ejemplo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

Cuando usa Azure AD para habilitar la autenticación con un solo clic en la aplicación o en la descripción de prueba, obtiene las siguientes ventajas:

- Optimice la experiencia del cliente de Marketplace a la descripción de prueba.
- Mantenga la sensación de una "experiencia dentro del producto" incluso cuando se redirige al usuario desde Marketplace al dominio o entorno de evaluación.
- Reduzca la probabilidad de abandono cuando redireccione a los usuarios al no haber pasos adicionales de inicio de sesión.
- Reduzca las barreras de implementación para la gran población de usuarios de Azure AD.

## <a name="verify-azure-ad-integration"></a>Comprobación de la integración de Azure AD

### <a name="multitenant-solutions"></a>Soluciones multiinquilino

Use Azure AD para admitir las acciones siguientes:

- Registre la aplicación en uno de los escaparates de Marketplace. Consulte el vínculo sobre el [registro de una aplicación](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) o la [obtención de un certificado de AppSource](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified) para obtener más información.
- Habilite la característica de compatibilidad multiinquilino en Azure AD para obtener una experiencia de evaluación con un clic.

Si no está familiarizado con el uso del inicio de sesión único federado de Azure AD, siga estos pasos:

1. Registre la aplicación en Marketplace.
1. Desarrolle el SSO con Azure AD mediante el uso de [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) u [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
1. Habilite la característica de compatibilidad multiinquilino en Azure AD para proporcionar una experiencia de evaluación con un clic.

### <a name="single-tenant-solutions"></a>Soluciones de un solo inquilino

Use Azure AD para admitir alguna de las acciones siguientes:

- Agregue usuarios al directorio como invitados con [B2B de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- Configure manualmente las versiones de evaluación de los clientes mediante la opción de publicación **Ponerse en contacto conmigo**.
- Desarrolle una versión de prueba por cliente.
- Cree una aplicación de demostración de ejemplo para varios inquilinos que use el inicio de sesión único.

## <a name="next-steps"></a>Pasos siguientes

- Asegúrese de haberse [registrado en Azure Marketplace](https://azuremarketplace.microsoft.com/sell).
- Consulte [Creación de una cuenta del Centro de partners](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) para obtener más información sobre cómo crear o completar su oferta.
