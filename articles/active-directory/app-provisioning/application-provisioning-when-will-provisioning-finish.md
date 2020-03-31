---
title: El aprovisionamiento de usuarios para la aplicación de la galería de Azure AD tarda horas o días
description: Cómo averiguar por qué el aprovisionamiento de la aplicación puede estar tardando más de lo previsto
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb22d19733fbeb162fba55dd732c10e552879c78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522652"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Aprovisionamiento de usuarios a una aplicación de la Galería de Azure AD que tarda horas o más

Cuando se habilita por primera vez el aprovisionamiento automático de una aplicación, el ciclo inicial puede tardar desde 20 minutos hasta varias horas, según el tamaño del directorio de Azure AD y el número de usuarios en el ámbito de aprovisionamiento. 

Los ciclos posteriores al inicial serán más rápidos, ya que el servicio de aprovisionamiento almacena marcas de agua que representan el estado de ambos sistemas tras el ciclo inicial, lo cual mejora el rendimiento de los posteriores.

## <a name="how-to-improve-provisioning-performance"></a>Cómo mejorar el rendimiento del aprovisionamiento

Si el ciclo inicial está tardando más de unas horas, puede hacer algo para mejorar el rendimiento:

-   **Filtros de ámbito del usuario.** Los filtros de ámbito le permiten ajustar los datos que el servicio de aprovisionamiento de Azure AD extrae mediante el filtrado de usuarios basado en valores de atributo determinados. Para más información sobre los filtros de ámbito, consulte [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

## <a name="next-steps"></a>Pasos siguientes
[Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](user-provisioning.md)

