---
title: Sincronización de directorios con Azure Active Directory
description: Guía de arquitectura para lograr la sincronización de directorios con Azure Active Directory.
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
ms.openlocfilehash: c340f973193f9c46735423c86112816003fecfcd
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578916"
---
# <a name="directory-synchronization"></a>Sincronización de directorios

Muchas organizaciones tienen una infraestructura híbrida que abarca componentes locales y en la nube. Sincronizar las identidades de los usuarios entre directorios locales y en la nube permite a los usuarios acceder a los recursos con un único conjunto de credenciales. 

La sincronización es el proceso de: 

* crear un objeto basado en determinadas condiciones;
* mantener el objeto actualizado;
* quitar el objeto cuando ya no se cumplen las condiciones. 

El aprovisionamiento local implica el aprovisionamiento desde orígenes locales (como Active Directory) hacia Azure Active Directory (Azure AD). 

## <a name="use-when"></a>Cuándo se utiliza

Tiene que sincronizar datos de identidad de entornos locales de Active Directory con Azure AD.

![Diagrama de arquitectura](./media/authentication-patterns/dir-sync-auth.png)

## <a name="components-of-system"></a>Componentes del sistema

* **Usuario**: Accede a una aplicación mediante Azure AD.

* **Explorador web**: Componente con el que el usuario interactúa para acceder a la dirección URL externa de la aplicación.

* **Aplicación**: Aplicación web que se basa en el uso de Azure AD para fines de autenticación y autorización.

* **Azure AD**: Sincroniza la información de identidad del directorio local de la organización a través de Azure AD Connect. 

* **Azure AD Connect**: Herramienta para la conexión de infraestructuras de identidad locales con Microsoft Azure AD. El asistente y las experiencias guiadas le ayudan a implementar y configurar los requisitos previos y los componentes necesarios para la conexión, incluida la sincronización y el inicio de sesión de Active Directory a Azure AD. 

* **Active Directory**: Servicio de directorio incluido en la mayoría de los sistemas operativos Windows Server. Los servidores que ejecutan Active Directory Domain Services (AD DS) se denominan controladores de dominio. Autentican y autorizan a todos los usuarios y equipos del dominio.

## <a name="implement-directory-synchronization-with-azure-ad"></a>Implementación de la sincronización de directorios con Azure AD

* [Qué es el aprovisionamiento de identidades](https://docs.microsoft.com/azure/active-directory/cloud-provisioning/what-is-provisioning) 

* [Herramientas para la integración de directorios de identidades híbridas](https://docs.microsoft.com/azure/active-directory/hybrid/plan-hybrid-identity-design-considerations-tools-comparison) 

* [Hoja de ruta de instalación de Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-roadmap)
