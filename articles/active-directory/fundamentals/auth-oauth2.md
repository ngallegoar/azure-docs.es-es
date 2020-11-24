---
title: Autenticación de OAuth 2.0 con Azure Active Directory
description: Guía de arquitectura para lograr la autenticación de OAUTH 2.0 con Azure Active Directory.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: daf40a2ced3f753619e9c4723dbe78cd7e51ff21
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94577913"
---
# <a name="oauth-20-authentication-with-azure-active-directory"></a>Autenticación de OAuth 2.0 con Azure Active Directory

OAuth 2.0 es el protocolo de autorización del sector. Permite que un usuario conceda acceso limitado a sus recursos protegidos. Diseñado para trabajar específicamente con el Protocolo de transferencia de hipertexto (HTTP), OAuth separa el rol del cliente del propietario del recurso. El cliente solicita acceso a los recursos que controla el propietario del recurso y que hospeda el servidor de recursos. El servidor de recursos emite tokens de acceso con la aprobación del propietario del recurso. El cliente usa los tokens de acceso para acceder a los recursos protegidos que hospeda el servidor de recursos. 

OAuth 2.0 está directamente relacionado con OpenID Connect (OIDC). Como OIDC es una capa de autenticación y autorización basada en OAuth 2.0, no es compatible con OAuth 1.0, una versión anterior. Azure Active Directory (Azure AD) admite todos los flujos de OAuth 2.0. 

## <a name="use-when"></a>Úsala en estos casos:

En escenarios de aplicaciones modernas y clientes enriquecidos y acceso a la API web RESTful.

![Diagrama de la arquitectura](./media/authentication-patterns/oauth.png)

## <a name="components-of-system"></a>Componentes del sistema

* **Usuario**: solicita un servicio a la aplicación web (aplicación). Por lo general, el usuario es el propietario del recurso que posee los datos y tiene la capacidad de permitir que los clientes accedan a los datos o al recurso. 

* **Explorador web**: el explorador web con el que interactúa el usuario es el cliente OAuth. 

* **Aplicación web**: la aplicación web o el servidor de recursos es donde residen los datos o el recurso. Confía en el servidor de autorizaciones para autenticar y autorizar de forma segura al cliente OAuth. 

* **Azure AD**: Azure AD es el servidor de autorización, también conocido como el proveedor de identidades (IdP). Controla de forma segura todo lo que se debe hacer con la información del usuario, su acceso y la relación de confianza. Es responsable de emitir los tokens que conceden y revocan el acceso a los recursos.

## <a name="implement-oauth-20-with-azure-ad"></a>Implementación de OAuth 2.0 con Azure AD

* [Integración de aplicaciones con Azure AD](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) 

* [Protocolos OAuth 2.0 y OpenID Connect en la Plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) 

* [Tipos de aplicación y OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) 

 
