---
title: Información general sobre la prueba comparativa de seguridad de Azure
description: Información general sobre la prueba comparativa de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-baselines
ms.openlocfilehash: c16d7247b781fea04cfa2d53b8854cff14e039c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616443"
---
# <a name="overview-of-azure-security-baselines"></a>Introducción a la línea de base de seguridad de Azure

Las líneas de base de seguridad de Azure le ayudan a reforzar la seguridad de nuestros productos mediante herramientas mejoradas, seguimiento y características de seguridad, y le proporcionan una experiencia coherente a la hora de proteger su entorno.

Las líneas de base de seguridad del servicio de Azure están enfocadas a las áreas de control centradas en la nube. Estos controles son coherentes con las pruebas comparativas de seguridad conocidas, como las descritas por Center for Internet Security (CIS). Nuestras líneas de base proporcionan una guía para las áreas de control enumeradas en la [Prueba comparativa de la seguridad de Azure](overview.md).

Cada recomendación incluye la siguiente información:
- **Identificador de Azure**: El identificador de la prueba comparativa de seguridad de Azure que corresponde a la recomendación.
- **Recomendación:** Justo después del identificador de Azure, la recomendación proporciona una descripción de alto nivel del control.
- **Guía**: La lógica de la recomendación y vínculos a instrucciones sobre cómo implementarla. Si la recomendación es compatible con Azure Security Center, esta información también se mostrará.
- **Responsabilidad**: Quién es el responsable de implementar el control. Las posibles respuestas son: responsabilidad del cliente, responsabilidad de Microsoft o responsabilidad compartida.
- **Supervisión de Azure Security Center**: Si Azure Security Center supervisa el control, con un vínculo a referencia.

Todas las recomendaciones, incluidas las recomendaciones que no son aplicables a este servicio concreto, se incluyen en la línea de base para proporcionarle una visión completa de la relación entre la prueba comparativa de la seguridad de Azure y cada servicio. Es posible que haya controles que no son aplicables por varias razones, como los controles de IaaS y los centrados en procesos (como, por ejemplo, los controles específicos de la administración de configuración del sistema operativo), que no se pueden aplicar a los servicios de PaaS.
