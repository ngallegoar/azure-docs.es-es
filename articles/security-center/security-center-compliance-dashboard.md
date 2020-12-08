---
title: 'Tutorial: Comprobaciones de cumplimiento normativo: Azure Security Center'
description: 'Tutorial: Aprenda a mejorar el cumplimiento de reglamentaciones mediante Azure Security Center.'
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2020
ms.author: memildin
ms.openlocfilehash: bbc36dbb2a17d379d31a9a235898500aea36247d
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96533917"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Tutorial: Mejora del cumplimiento normativo

Azure Security Center resulta de gran ayuda para simplificar el proceso necesario para cumplir los requisitos de cumplimiento normativo, para lo que se usa el **panel de cumplimiento normativo.** . En el panel, Security Center proporciona información detallada acerca de su estado de cumplimiento de la norma, gracias a la valoración continua de su entorno de Azure. Security Center analiza los factores de riesgo en su entorno de nube híbrida según los procedimientos recomendados de seguridad. Estas valoraciones se asignan a los controles de cumplimiento desde un conjunto de estándares compatible. En el panel de cumplimiento normativo se ve el estado de todas estas valoraciones dentro de su entorno en el contexto de una norma o reglamento concretos. Al actuar sobre las recomendaciones y reducir los factores de riesgo en su entorno, su estado de cumplimiento normativo mejora.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Evaluar su cumplimiento normativo mediante el panel de cumplimiento normativo.
> * Mejorar su estado de cumplimiento normativo mediante la realización de acciones sobre las recomendaciones

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para recorrer las características descritas en este tutorial:

- [Azure Defender](azure-defender.md) debe estar habilitado. Puede probar Azure Defender gratis durante 30 días.
- Debe haber iniciado sesión con una cuenta que tenga acceso de lectura a los datos de cumplimiento de directivas (**Lector de seguridad** no es suficiente). El rol **Lector global** para la suscripción funcionará. Como mínimo, necesitará tener asignados los roles **Colaborador de directivas de recursos** y **Administrador de seguridad**.

##  <a name="assess-your-regulatory-compliance"></a>Mejora del cumplimiento de reglamentaciones

Security Center evalúa constantemente la configuración de los recursos para identificar problemas de seguridad y vulnerabilidades. Estas valoraciones se presentan como recomendaciones, que se centran en mejorar su higiene de seguridad. En el panel de cumplimiento normativo verá un conjunto de normas de cumplimiento con todos sus requisitos en el que los compatibles se asignan a las valoraciones de seguridad aplicables. Esto le permite ver su estado de cumplimiento de la norma, en función del estado de estas valoraciones.

La vista del panel de cumplimiento de reglamentaciones puede ayudarle a centrar la atención en los espacios que hay en el cumplimiento de un estándar o reglamentación que es importante para usted. También le permite supervisar continuamente su puntuación de cumplimiento a lo largo del tiempo en entornos híbrido y de nube dinámicos.

>[!NOTE]
> De forma predeterminada, Security Center admite los siguientes estándares: Azure CIS, PCI DSS 3.2, ISO 27001 y SOC TSP. 
>
> La característica de [paquetes de cumplimiento dinámicos (versión preliminar)](update-regulatory-compliance-packages.md) permite actualizar los estándares que se muestran en el panel de cumplimiento normativo a los nuevos paquetes *dinámicos*. También puede usar la misma característica en vista previa para agregar nuevos paquetes de cumplimiento y supervisar el cumplimiento de estándares adicionales. 

1. En el menú de Security Center, seleccione **Cumplimiento normativo**. <br>
En la parte superior de la pantalla se ve un panel con información general acerca del estado de cumplimiento con el conjunto de reglamentaciones de cumplimiento admitidas. Puede ver la puntuación global de cumplimiento y el número de valoraciones aprobadas y suspendidas asociadas a cada estándar.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Panel de cumplimiento normativo":::

1. Seleccione la pestaña de la norma que le interese (1). Verá las suscripciones en las que se aplica la norma (2) y la lista de todos los controles de esa norma (3). Para sabe qué controles se pueden aplicar, puede ver los detalles de las valoraciones aprobadas y suspendidas asociadas a ese control (4) y la cantidad de recursos afectados (5). Algunos controles aparecen atenuados, no tienen valoraciones de Security Center asociadas. Compruebe los requisitos de estos y valórelos en su entorno. Es posible que algunos de ellos estén relacionados con los procesos y no sean técnicos.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Exploración de los detalles de cumplimiento con una norma específica":::

1. Para generar y descargar un informe en PDF que resuma su estado actual de cumplimiento de un estándar concreto, haga clic en **Download report** (Descargar informe).

    El informe proporciona un resumen de alto nivel del estado de cumplimiento del estándar seleccionado, para lo que usa los datos de las evaluaciones de Security Center, y se organiza según los controles de dicho estándar concreto. El informe se puede compartir con las partes interesadas competentes y puede proporcionar evidencia a los auditores internos y externos.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Descarga del informe de cumplimiento":::

## <a name="improve-your-compliance-posture"></a>Mejora de su estado de cumplimiento de la norma

Con toda la información del panel de cumplimiento normativo puede mejorar su situación respecto a este tema mediante la resolución de recomendaciones directamente en el panel.

1.  Haga clic en cualquiera de las valoraciones suspensas que aparecen en el panel para ver los detalles de dicha recomendación. Cada recomendación incluye un conjunto de pasos de corrección que se deben seguir si se desea resolver el problema.

1.  Puede seleccionar un recurso concreto para ver más detalles y resolver la recomendación del mismo. <br>Por ejemplo, en **Azure CIS 1.1.0 (New) standard** (Norma de Azure CIS 1.1.0 [Nueva]), puede seleccionar la recomendación **El cifrado de disco se debe aplicar en las máquinas virtuales**.

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="Al seleccionar una recomendación de una norma se va directamente a la página de detalles de la recomendación.":::

1. En este ejemplo, al seleccionar **Realizar acción** en la página de detalles de la recomendación, llegará a las páginas de la máquina virtual de Azure de Azure Portal, donde puede abrir la pestaña **Seguridad** y habilitar el cifrado:

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="Botón Realizar acción en la página de detalles de la recomendación que conduce a las opciones de corrección":::

    Para más información sobre cómo aplicar las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md).

1.  Después de realizar las acciones necesarias para resolver las recomendaciones, verá su repercusión en informe del panel de cumplimiento, ya que la puntuación de cumplimiento mejora.

    > [!NOTE]
    > Las valoraciones se ejecutan aproximadamente cada 12 horas, por lo que el efecto sobre los datos de cumplimiento solo se constatará tras la ejecución siguiente de la valoración en cuestión.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a utilizar el panel de cumplimiento normativo de Security Center para:

-   Ver y supervisar su estado de cumplimiento de la norma, en relación con los estándares y regulaciones que le resultan importantes.
-   Mejorar el estado de cumplimiento mediante la resolución de las recomendaciones pertinentes y la observación de la mejora en la puntuación del cumplimiento.

El panel de cumplimiento normativo simplifica considerablemente el proceso de cumplimiento y reduce en gran medida el tiempo necesario para la recopilación de pruebas de cumplimiento en el entorno de Azure e híbrido.

Para más información, consulte los artículos relacionados:

-   [Actualización de los paquetes de cumplimiento dinámicos en el panel de cumplimiento normativo (versión preliminar)](update-regulatory-compliance-packages.md): aprenda sobre esta característica en versión preliminar que permite actualizar las normas que se muestran en el panel de cumplimiento normativo a los nuevos paquetes *dinámicos*. También puede usar la misma característica en vista previa para agregar nuevos paquetes de cumplimiento y supervisar el cumplimiento de estándares adicionales. 
-   [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
-   [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): aprenda a usar las recomendaciones de Azure Security Center como ayuda para la protección de los recursos de Azure.
-   [Mejora de la puntuación de seguridad de Azure Security Center](secure-score-security-controls.md): aprenda a dar prioridad a los puntos vulnerables y las recomendaciones de seguridad para mejorar al máximo su postura ante la seguridad.
