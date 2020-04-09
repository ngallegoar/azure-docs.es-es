---
title: Cómo actualizar a la supervisión de cumplimiento normativo dinámico en el panel de cumplimiento normativo de Azure Security Center | Microsoft Docs
description: Actualización de los paquetes de cumplimiento normativo (versión preliminar)
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 4080825bbb1f6c274f5b5aafd28e8c672148b98f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159292"
---
# <a name="update-to-dynamic-compliance-packages-in-your-regulatory-compliance-dashboard-preview"></a>Actualización a los paquetes de cumplimiento dinámicos en el panel de cumplimiento normativo (versión preliminar)

Azure Security Center compara continuamente la configuración de los recursos con los requisitos de los estándares del sector, las regulaciones y los bancos de pruebas. En el **panel de cumplimiento normativo** se proporciona información sobre su postura de cumplimiento en función de cómo cumple los requisitos y controles de cumplimiento específicos.

Un estándar del que pueda realizar un seguimiento de su postura de cumplimiento es [Azure CIS 1.1.0](https://www.cisecurity.org/benchmark/azure/) (más formalmente, "CIS Microsoft Azure Foundations Benchmark, versión 1.1.0"). 

La representación de Azure CIS que aparece inicialmente en el panel de cumplimiento depende de un conjunto estático de reglas que se incluye con Security Center.

Gracias a la característica de **paquetes de cumplimiento dinámicos (versión preliminar)** , Security Center mejora automáticamente su cobertura de los estándares del sector a lo largo del tiempo. Los paquetes de cumplimiento son esencialmente iniciativas definidas en Azure Policy. Se pueden asignar al ámbito seleccionado (suscripción, grupo de administración, etc.). Para ver los datos de cumplimiento asignados como evaluaciones en el panel, agregue un paquete de cumplimiento a la suscripción o el grupo de administración desde la directiva de seguridad. Al agregar un paquete de cumplimiento, se asigna de forma eficaz la iniciativa de cumplimiento normativo al ámbito seleccionado. De esta manera, puede realizar un seguimiento en el panel de las iniciativas normativas recién publicadas como estándares de cumplimiento. Cuando Microsoft publica contenido nuevo para la iniciativa (nuevas directivas que se asignan a más controles en el estándar), el contenido adicional aparece automáticamente en el panel.

El paquete de cumplimiento dinámico del banco de pruebas de Azure CIS, **Azure CIS 1.1.0 (nuevo)** , mejora la versión *estática* de la original de la siguiente manera:

* Incluye más directivas
* Se actualiza automáticamente con la nueva cobertura a medida que se agrega 

Actualice al nuevo paquete dinámico tal como se describe a continuación.

## <a name="adding-a-dynamic-compliance-package"></a>Agregar un paquete de cumplimiento dinámico

En los pasos siguientes se explica cómo agregar el paquete dinámico para supervisar su cumplimiento con el banco de pruebas de Azure CIS v1.1.0.   

### <a name="update-to-the-azure-cis-110-new-dynamic-compliance-package"></a>Actualización al paquete de cumplimiento dinámico de Azure CIS 1.1.0 (nuevo) 

1. Abra la página **Directiva de seguridad**. En esta página se muestra el número de grupos de administración, suscripciones y áreas de trabajo, así como la estructura del grupo de administración.

1. Seleccione la suscripción o el grupo de administración para el que desea administrar la postura de cumplimiento normativo. Se recomienda seleccionar el ámbito más alto para el que se aplica el estándar y así poder agregar y realizar un seguimiento de los datos de cumplimiento para todos los recursos anidados. 

1. En la sección de estándares del sector y normativos (versión preliminar), verá que Azure CIS 1.1.0 se puede actualizar para obtener el nuevo contenido. Haga clic en **Actualizar**. 

1. Opcionalmente, haga clic en **Agregar más estándares** para abrir la página **Adición de estándares de cumplimiento normativo**. En ella puede buscar de forma manual **Azure CIS 1.1.0 (Nuevo)** y los paquetes dinámicos de otros estándares de cumplimiento, como **NIST SP 800-53 R4**, **SWIFT CSP CSCF-v2020**, **UKO y UK NHS** y **Canada PBMM**.
    
    > [!TIP]
    > Solo los usuarios que son propietario o colaborador de directivas tienen los permisos necesarios para agregar estándares de cumplimiento. 

    ![Agregar paquetes normativos al panel de cumplimiento normativo de Azure Security Center](./media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-additional-standards.png)


1. En la barra lateral de Security Center, seleccione **Cumplimiento normativo** para abrir el panel de cumplimiento normativo. 
    * Azure CIS 1.1.0 (Nuevo) aparece ahora en la lista de estándares normativos y del sector. 
    * La vista *estática* original del cumplimiento de Azure CIS 1.1.0 también permanecerá junto a ella. Es posible que se quite automáticamente en el futuro.

    > [!NOTE]
    > Asimismo, un estándar recién agregado en el panel de cumplimiento puede tardar unas horas en aparecer en el mismo.


    [![Panel de cumplimiento normativo que muestra los nuevos y viejos valores de Azure CIS](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png)](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png#lightbox)


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido lo siguiente:

* Cómo **actualizar los estándares** que se muestran en el panel de cumplimiento normativo a los nuevos paquetes *dinámicos*.
* Cómo **agregar paquetes de cumplimiento** para supervisar el cumplimiento de estándares adicionales. 

Para obtener material relacionado, consulte los siguientes artículos: 

- [Panel de cumplimiento normativo de Security Center](security-center-compliance-dashboard.md)
- [Uso de las directivas de seguridad](tutorial-security-policy.md)
- [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): aprenda a usar las recomendaciones de Azure Security Center como ayuda para la protección de los recursos de Azure.