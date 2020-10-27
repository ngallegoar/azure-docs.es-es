---
title: Uso del panel de cumplimiento normativo de Azure Security Center
description: Aprenda a agregar y quitar estándares normativos desde el panel de cumplimiento normativo de Security Center
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2020
ms.author: memildin
ms.openlocfilehash: 4ec75931a335170a8262cc480f43a0f22db0b79b
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92342202"
---
# <a name="customizing-the-set-of-standards-in-your-regulatory-compliance-dashboard"></a>Personalización del conjunto de estándares en el panel de cumplimiento normativo

Azure Security Center compara continuamente la configuración de los recursos con los requisitos de los estándares del sector, las regulaciones y los bancos de pruebas. En el **panel de cumplimiento normativo** se proporciona información sobre su postura de cumplimiento en función de cómo cumple los requisitos y controles de cumplimiento específicos.


## <a name="overview-of-compliance-packages"></a>Información general sobre los paquetes de cumplimiento

Los estándares del sector, los estándares normativos y las pruebas comparativas se representan en Security Center como *paquetes de cumplimiento* .  Cada paquete es una iniciativa definida en Azure Policy. Para ver los datos de cumplimiento asignados como evaluaciones en el panel, agregue un paquete de cumplimiento a la suscripción o al grupo de administración desde la página **Directiva de seguridad** . (Obtenga más información sobre Azure Policy y las iniciativas en [Uso de directivas de seguridad](tutorial-security-policy.md)).

Cuando haya incorporado un estándar o una prueba comparativa, asignará la iniciativa al ámbito y el estándar aparecerá en el panel de cumplimiento normativo con todos los datos de cumplimiento asociados asignados como evaluaciones. También puede descargar informes de resumen para cualquiera de los estándares que se hayan incorporado.

Microsoft también realiza un seguimiento de los estándares normativos y mejora automáticamente su cobertura en algunos de los paquetes a lo largo del tiempo. Cuando Microsoft publica contenido nuevo para la iniciativa (nuevas directivas que se asignan a más controles en el estándar), el contenido adicional aparece automáticamente en el panel.

> [!TIP]
> Un estándar que mejora con el tiempo a medida que Microsoft publica nuevo contenido es **Azure CIS 1.1.0 (nuevo)** (más formalmente, [CIS Microsoft Azure Foundations Benchmark, versión 1.1.0](https://www.cisecurity.org/benchmark/azure/)). Tendrá que agregarlo al panel junto a "Azure CIS 1.1.0", la representación de Azure CIS que está configurada de forma predeterminada en cada entorno de Security Center. Ese paquete se basa en un conjunto estático de reglas. El paquete más reciente incluye más directivas y se actualizará automáticamente con el tiempo. Actualice al nuevo paquete dinámico tal como se describe a continuación.


## <a name="available-packages"></a>Paquetes disponibles

Puede agregar estándares como NIST SP 800-53 R4, SWIFT CSP CSCF-v2020, UK Official y UK NHS, Canada Federal PBMM y Azure CIS 1.1.0 (nuevo), una representación más completa de Azure CIS 1.1.0. 

Además, puede agregar **Azure Security Benchmark** , las instrucciones específicas de Azure creadas por Microsoft para los procedimientos recomendados de seguridad y cumplimiento basados en los marcos de cumplimiento comunes. ([Mas información sobre Azure Security Benchmark](../security/benchmarks/introduction.md)).

Se admitirán estándares adicionales en el panel a medida que estén disponibles. 


## <a name="add-a-regulatory-standard-to-your-dashboard"></a>Adición de un estándar de cumplimiento al panel

En los pasos siguientes se explica cómo agregar un paquete para supervisar el cumplimiento de uno de los estándares normativos admitidas.

> [!NOTE]
> Solo los usuarios que son propietario o colaborador de directivas tienen los permisos necesarios para agregar estándares de cumplimiento. 

1. En la barra lateral de Security Center, seleccione **Cumplimiento normativo** para abrir el panel de cumplimiento normativo. Aquí puede ver los estándares de cumplimiento asignados actualmente a las suscripciones que están seleccionadas.   

1. En la parte superior de la página, seleccione **Administrar directivas de cumplimiento** . Se mostrará la página Administración de directivas.

1. Seleccione la suscripción o el grupo de administración para el que desea administrar la postura de cumplimiento normativo. 

    > [!TIP]
    > Se recomienda seleccionar el ámbito más alto para el que se aplica el estándar y así poder agregar y realizar un seguimiento de los datos de cumplimiento para todos los recursos anidados. 

1. Para agregar los estándares pertinentes para su organización, haga clic en **Agregar más estándares** . 

1. En la página **Adición de estándares de cumplimiento normativo** , puede buscar paquetes para cualquiera de los estándares disponibles. Algunos de los estándares disponibles son los siguientes:

    - **Azure Security Benchmark**
    - **NIST SP 800-53 R4**
    - **SWIFT CSP CSCF-v2020**
    - **UKO y UK NHS**
    - **Canada PBMM**
    
    ![Agregar paquetes normativos al panel de cumplimiento normativo de Azure Security Center](./media/update-regulatory-compliance-packages/dynamic-regulatory-compliance-additional-standards.png)

1. Seleccione **Agregar** y escriba todos los detalles necesarios para la iniciativa específica, como el ámbito, los parámetros y la corrección.

1. En la barra lateral de Security Center, vuelva a seleccionar **Cumplimiento normativo** para volver al panel de cumplimiento normativo.
    * El nuevo estándar aparece ahora en la lista de estándares normativos y del sector. 
    * Si ha agregado **Azure CIS 1.1.0 (nuevo)** , la vista *estática* original del cumplimiento de Azure CIS 1.1.0 también permanecerá junto a él. Es posible que se quite automáticamente en el futuro.

    > [!NOTE]
    > Asimismo, un estándar recién agregado en el panel de cumplimiento puede tardar unas horas en aparecer en el mismo.

    [![Panel de cumplimiento normativo que muestra los nuevos y viejos valores de Azure CIS](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark-small.png)](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark.png#lightbox)


## <a name="removing-a-standard-from-your-dashboard"></a>Eliminación de un estándar del panel

Si alguno de los estándares normativos proporcionados no es relevante para su organización, quitarlo de la interfaz de usuario es un proceso sencillo. Esto le permite personalizar aún más el panel de cumplimiento normativo y centrarse solo en los estándares que se apliquen en su caso.

Para quitar un estándar:

1. En el menú de Security Center, seleccione **Directiva de seguridad** .

1. Seleccione la suscripción pertinente de la que desea quitar un estándar.

    > [!NOTE]
    > Puede quitar un estándar de una suscripción, pero no de un grupo de administración. 

    Se abrirá la página de directivas de seguridad. En la suscripción seleccionada, se muestra la directiva predeterminada, el sector y los estándares normativos, así como las iniciativas personalizadas que haya creado.

    :::image type="content" source="./media/update-regulatory-compliance-packages/remove-standard.png" alt-text="Eliminación de un estándar normativo del panel de cumplimiento normativo en Azure Security Center":::

1. Para el estándar que desea quitar, seleccione **Deshabilitar** . Se abrirá una ventana de confirmación.

    :::image type="content" source="./media/update-regulatory-compliance-packages/remove-standard-confirm.png" alt-text="Eliminación de un estándar normativo del panel de cumplimiento normativo en Azure Security Center":::.

1. Seleccione **Sí** . Se quitará el estándar. 


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo **agregar paquetes de cumplimiento** para supervisar el cumplimiento de estándares adicionales. 

Para obtener material relacionado, consulte los siguientes artículos: 

- [Azure Security Benchmark](../security/benchmarks/introduction.md)
- [Panel de cumplimiento normativo de Security Center](security-center-compliance-dashboard.md)
- [Uso de las directivas de seguridad](tutorial-security-policy.md)