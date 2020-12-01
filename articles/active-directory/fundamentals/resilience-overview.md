---
title: Aumento de la resistencia en la administración de identidad y acceso con Azure Active Directory
description: Guía para arquitectos, administradores de TI y desarrolladores sobre la creación de resistencia para la interrupción de sus sistemas de identidad.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7dfd51b0ed43badbc6a4882f619cb718952b0e85
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919125"
---
# <a name="building-resilience-into-identity-and-access-management-with-azure-active-directory"></a>Aumento de la resistencia en la administración de identidad y acceso de clientes con Azure Active Directory

La administración de identidad y acceso (IAM) es un marco de procesos, directivas y tecnologías que facilitan la administración de identidades y su acceso. Incluye los numerosos componentes que admiten la autenticación y la autorización del usuario y otras cuentas en el sistema.

La resistencia de IAM es la capacidad de soportar la interrupción de los componentes del sistema y de recuperarse con un impacto mínimo en la empresa, los usuarios, los clientes y las operaciones. La reducción de las dependencias, la complejidad y los únicos puntos de error, a la vez que se garantiza un control de errores completo, aumentará la resistencia.

La interrupción puede proviene de cualquier componente de los sistemas IAM. Para compilar un sistema IAM resistente, vamos a suponer que se producen interrupciones y que se programa para ellas. 

Al planear la resistencia de la solución IAM, tenga en cuenta los siguientes elementos: 

* Las aplicaciones que se basan en el sistema IAM.

* Las infraestructuras públicas que usan las llamadas de autenticación, incluidas las compañías de telecomunicaciones, los proveedores de servicios de Internet y los proveedores de claves públicas.

* Los proveedores de identidades locales y en la nube.

* Otros servicios que dependen de su IAM y las API que los conectan.

* Cualquier otro componente local del sistema.

Independientemente del origen, es importante reconocer las contingencias y planearse para ellas. Sin embargo, agregar sistemas de identidad adicionales, así como sus dependencias y complejidad resultantes, puede reducir la resistencia en lugar de aumentarla.

Para crear más resistencia en los sistemas, revise los artículos siguientes:

* [Aumento de la resistencia en la infraestructura de IAM](resilience-in-infrastructure.md)

* [Aumento de la resistencia IAM en las aplicaciones](resilience-app-development-overview.md)

* [Aumento de la resistencia en los sistemas CIAM](resilience-b2c.md)
