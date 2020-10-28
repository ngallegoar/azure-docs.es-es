---
title: Sincronización de SCIM con Azure Active Directory
description: Guía arquitectónica para lograr este patrón de sincronización
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
ms.openlocfilehash: be8dbd4d8deccd42d6fcc391eab1e57df7514401
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92113963"
---
# <a name="scim-synchronization-with-azure-active-directory"></a>Sincronización de SCIM con Azure Active Directory

System for Cross-domain Identity Management (SCIM) es un protocolo estándar abierto para automatizar el intercambio de información de la identidad de usuarios entre dominios de identidad y sistemas de TI. SCIM garantiza que se creen automáticamente cuentas en Azure Active Directory (Azure AD) o Active Directory de Windows Server para los empleados que se agreguen al sistema de administración de capital humano (HCM). Los atributos y perfiles de usuario se sincronizan entre los dos sistemas, actualizando y eliminando usuarios en función del estado de usuario o el cambio de rol.

SCIM es una definición estándar de dos puntos de conexión: /Users (Usuarios) y /Groups (Grupos). Usa verbos de REST comunes para crear, actualizar y eliminar objetos. También usa un esquema predefinido para atributos comunes, como el nombre de grupo, nombre de usuario, nombre, apellidos y correo electrónico. Las aplicaciones que ofrecen una API de REST de SCIM 2.0 pueden reducir o eliminar la molestia de trabajar con productos o API propios de administración de usuarios. Por ejemplo, cualquier cliente compatible con SCIM puede crear un método HTTP POST de un objeto JSON para el punto de conexión /Users a fin de crear una nueva entrada de usuario. En lugar de necesitar una API ligeramente diferente para las mismas acciones básicas, las aplicaciones que cumplan con el estándar SCIM pueden aprovechar al instante los clientes, herramientas y código ya existentes. 

## <a name="use-when"></a>Úsala en estos casos: 

Quiere aprovisionar automáticamente la información de los usuarios de un sistema HCM a Azure AD y Windows Server Active Directory y, a continuación, a sistemas de destino si es necesario. 

![Diagrama de arquitectura](./media/authentication-patterns/scim-auth.png)


## <a name="components-of-system"></a>Componentes del sistema 

* **Sistema HCM** : Aplicaciones y tecnologías que permiten el proceso y las prácticas de administración de capital humano que admiten y automatizan los procesos de recursos humanos a lo largo del ciclo de vida de los empleados. 

* **Servicio de aprovisionamiento de Azure AD** : Usa el protocolo SCIM 2.0 para el aprovisionamiento automático. El servicio se conecta al punto de conexión de SCIM para la aplicación y usa el esquema de objetos de usuario SCIM y las API de REST para automatizar el aprovisionamiento y el desaprovisionamiento de usuarios y grupos.  

* **Azure AD** : Repositorio de usuarios usado para administrar el ciclo de vida de las identidades y sus derechos. 

* **Sistema de destino** : Aplicación o sistema que tiene el punto de conexión de SCIM y funciona con el aprovisionamiento de Azure AD para habilitar el aprovisionamiento automático de usuarios y grupos.  

## <a name="implement-scim-with-azure-ad"></a>Implementación de SCIM con Azure AD 

* [Funcionamiento del aprovisionamiento en Azure AD](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works)

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales en Azure Portal](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

* [Creación de un punto de conexión SCIM y configuración del aprovisionamiento de usuarios con Azure AD](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups)

* [Cumplimiento del protocolo SCIM 2.0 con el servicio de aprovisionamiento de Azure AD](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groupsapplication-provisioning-config-problem-scim-compatibility)

