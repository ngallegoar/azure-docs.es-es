---
title: Guía de referencia de operaciones de Azure Active Directory
description: Esta guía de referencia de operaciones describe las comprobaciones y las acciones que debe realizar para proteger la administración de identidad y acceso, la autenticación, la gobernanza y las operaciones
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: c815256e60e5ca6582220d247d587a4488c4a55a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74534772"
---
# <a name="azure-active-directory-operations-reference-guide"></a>Guía de referencia de operaciones de Azure Active Directory

Esta guía de referencia de operaciones describe las comprobaciones y las acciones que debe realizar para proteger y mantener las áreas siguientes:

- **[La administración de identidades y accesos](active-directory-ops-guide-iam.md)** : capacidad de administrar el ciclo de vida de las identidades y sus derechos.
- **[Administración de autenticación](active-directory-ops-guide-auth.md)** : capacidad de administrar las credenciales, definir la experiencia de autenticación, delegar la asignación, medir el uso y definir directivas de acceso basadas en la postura de seguridad de la empresa.
- **[Gobernanza](active-directory-ops-guide-govern.md)** : capacidad para evaluar y confirmar el acceso concedido a las identidades sin privilegios y con privilegios, así como auditar y controlar los cambios en el entorno.
- **[Operaciones](active-directory-ops-guide-ops.md)** : optimizar las operaciones de Azure Active Directory (Azure AD).

Algunas de estas recomendaciones podrían no aplicarse al entorno de todos los clientes; por ejemplo, podrían no aplicarse los procedimientos recomendados de AD FS si la organización usara la sincronización de hash de contraseñas.

> [!NOTE]
> Estas recomendaciones están actualizadas hasta la fecha de publicación, pero pueden cambiar con el tiempo. Las organizaciones deben evaluar continuamente sus prácticas de identidad a medida que los productos y servicios de Microsoft evolucionen. Las recomendaciones pueden cambiar cuando las organizaciones se suscriben a una licencia de Azure AD Premium diferente. Por ejemplo, Azure AD Premium P2 incluirá más recomendaciones de gobernanza.

## <a name="stakeholders"></a>Partes interesadas

Cada sección de esta guía de referencia recomienda asignar las partes interesadas para planear e implementar correctamente las tareas clave. En la tabla siguiente se describe la lista de todas las partes interesadas de esta guía:

| Parte interesada | Descripción |
| :- | :- |
| Equipo de operaciones IAM | Este equipo controla la administración de las operaciones diarias del sistema de Administración de identidades y acceso |
| Equipo de productividad | Este equipo posee y administra las aplicaciones de productividad como el correo electrónico, el uso compartido de archivos y la colaboración, la mensajería instantánea y las conferencias. |
| Application Owner | Este equipo es el propietario de la aplicación específica de una empresa y, normalmente, una perspectiva técnica de una organización. |
| Equipo de arquitectura de InfoSec | Este equipo planea y diseña las prácticas de seguridad de la información de una organización. |
| Equipo de operaciones de InfoSec | Este equipo ejecuta y supervisa los procedimientos de seguridad de la información implementados del equipo de arquitectura de InfoSec. |

## <a name="next-steps"></a>Pasos siguientes

Empiece con las [comprobaciones y las acciones de administración de la autenticación](active-directory-ops-guide-iam.md).
