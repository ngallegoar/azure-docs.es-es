---
title: Administración entre inquilinos en Azure Security Center | Microsoft Docs
description: La administración entre inquilinos le permite ver y administrar la posición de seguridad de varios inquilinos en Security Center con Administración de recursos delegados de Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 7d51291a-4b00-4e68-b872-0808b60e6d9c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2019
ms.author: memildin
ms.openlocfilehash: 7700e151a49dbfbad452927ba4a308651a0b584a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84782819"
---
# <a name="cross-tenant-management-in-security-center"></a>Administración entre inquilinos en Security Center

La administración entre inquilinos le permite ver y administrar la posición de seguridad de varios inquilinos en Security Center con [Administración de recursos delegados de Azure](../lighthouse/concepts/azure-delegated-resource-management.md). Administre varios inquilinos de manera eficaz, desde una sola vista, sin que sea necesario que inicie sesión en el directorio de cada inquilino.

- Los proveedores de servicios pueden administrar la posición de seguridad de los recursos, para varios clientes, desde su propio inquilino.

- Los equipos de seguridad de las organizaciones con varios inquilinos pueden ver y administrar su posición de seguridad desde una ubicación única.

## <a name="set-up-cross-tenant-management"></a>Configuración de la administración entre inquilinos

Para configurar la administración entre inquilinos, delegue el acceso a los recursos de los inquilinos administrados a su propio inquilino con [Administración de recursos delegados de Azure](../lighthouse/concepts/azure-delegated-resource-management.md).

> [!NOTE]
> La administración de recursos delegados de Azure es uno de los componentes clave de Azure Lighthouse.

## <a name="how-does-cross-tenant-management-work-in-security-center"></a>Funcionamiento de la administración entre inquilinos en Security Center

Puede revisar y administrar suscripciones en varios inquilinos del mismo modo que administra varias suscripciones en un mismo inquilino.

En la barra de menús superior, haga clic en el icono de filtro y seleccione las suscripciones, del directorio de cada inquilino, que quiere ver.

  ![Filtrado de inquilinos](./media/security-center-cross-tenant-management/cross-tenant-filter.png)

Las vistas y acciones son básicamente las mismas. Estos son algunos ejemplos:

- **Administrar directivas de seguridad**: desde una vista, administre la posición de seguridad de muchos recursos con las [directivas](tutorial-security-policy.md), haga acciones con las recomendaciones de seguridad y recopile y administre los datos relacionados con la seguridad.
- **Mejorar la puntuación de seguridad y la posición de cumplimiento**: la visibilidad entre inquilinos le permite ver la posición de seguridad general de todos los inquilinos y dónde y cómo mejorar más la [puntuación segura](secure-score-security-controls.md) y la [posición de cumplimiento](security-center-compliance-dashboard.md) de cada uno de ellos.
- **Corrección de recomendaciones**: supervise y corrija una [recomendación](security-center-recommendations.md) para varios recursos de varios inquilinos de una sola vez. Luego, puede abordar de inmediato las vulnerabilidades que presentan el riesgo más alto en todos los inquilinos.
- **Administrar alertas**: detecte [alertas](security-center-alerts-overview.md) en los distintos inquilinos. Tome medidas en los recursos que no cumplen con los [pasos de corrección](security-center-managing-and-responding-alerts.md) accionables.

- **Administrar las características de protección en la nube avanzada y mucho más**: administre los distintos servicios de protección contra amenazas, como el [acceso a VM Just-in-Time (JIT)](security-center-just-in-time.md), la [protección de red adaptable](security-center-adaptive-network-hardening.md), los [controles de aplicaciones adaptables](security-center-adaptive-application.md), etc.
 
## <a name="next-steps"></a>Pasos siguientes
En este artículo se explica cómo funciona la administración entre inquilinos en Security Center. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Refuerce su seguridad con Azure Security Center](security-center-monitoring.md): descubra cómo supervisar el estado de sus recursos de Azure.
* [Preguntas más frecuentes sobre Azure Security Center](faq-general.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Obtenga información acerca de Azure Lighthouse en escenarios empresariales](https://docs.microsoft.com/azure/lighthouse/concepts/enterprise): conozca cómo Azure Lighthouse puede simplificar la administración entre inquilinos en empresas que usan varios inquilinos de Azure AD.