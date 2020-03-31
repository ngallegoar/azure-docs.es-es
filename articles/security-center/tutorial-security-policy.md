---
title: Trabajo con directivas de seguridad | Microsoft Docs
description: En este artículo se describe cómo trabajar con directivas de seguridad en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 45a27be400753b56c42310a340334feba8a420c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73906859"
---
# <a name="working-with-security-policies"></a>Uso de directivas de seguridad

En este artículo se explica cómo configurar directivas de seguridad y cómo visualizarlas en Security Center. 

## <a name="introduction-to-security-policies"></a>Introducción a las directivas de seguridad

Una directiva de seguridad define la configuración deseada de las cargas de trabajo y le ayuda a garantizar el cumplimiento de los requisitos de seguridad normativos o de empresa.

Azure Security Center realiza sus recomendaciones de seguridad en función de las directivas elegidas. Las directivas de Security Center se basan en las iniciativas de directiva creadas en Azure Policy. Puede configurarlas en [Azure Policy](../governance/policy/overview.md), que también le permite establecer directivas en grupos de administración y entre varias suscripciones.

Security Center le ofrece las siguientes opciones para trabajar con directivas de seguridad:

* **Ver y editar la directiva predeterminada integrada**: al habilitar Security Center, una iniciativa integrada denominada "ASC default" se asigna automáticamente a todas las suscripciones registradas de Security Center (niveles gratis o estándar). Para personalizar esta iniciativa, puede habilitar o deshabilitar las directivas individuales de la misma. Consulte la lista de [directivas de seguridad integradas](security-center-policy-definitions.md) para comprender las opciones disponibles.

* **Agregue sus propias directivas personalizadas**: si quiere personalizar las iniciativas de seguridad que se aplican a su suscripción, puede hacerlo en Security Center. A continuación, recibirá recomendaciones si las máquinas no siguen las directivas que creó. Para obtener instrucciones sobre la creación y asignación de directivas personalizadas, consulte [Uso de las directivas de seguridad personalizadas](custom-security-policies.md).

* **Agregar directivas de cumplimiento normativo**: el panel de cumplimiento normativo de Security Center muestra el estado de todas las evaluaciones del entorno, en el contexto de una normativa o estándar determinado (por ejemplo, Azure CIS, NIST SP 800-53 R4 o SWIFT CSP CSCF-v2020). Para obtener más información, consulte [Mejora del cumplimiento normativo](security-center-compliance-dashboard.md).


## <a name="managing-your-security-policies"></a>Administración de las directivas de seguridad

Para ver las directivas de seguridad de Security Center:

1. En el panel de **Security Center**, seleccione **Directiva de seguridad**.

    ![El panel Administración de directivas](./media/security-center-policies/security-center-policy-mgt.png)

   En la pantalla **Administración de directivas**, puede ver el número de grupos de administración, suscripciones y áreas de trabajo, así como la estructura del grupo de administración.

1. Seleccione el grupo de administración o la suscripción cuyas directivas desea ver.

1. Aparecerá la página de directiva de seguridad de la suscripción o el grupo de administración. Esta muestra las directivas disponibles y asignadas.

   ![pantalla de la directiva](./media/tutorial-security-policy/security-policy-page.png)

    > [!NOTE]
    > Si hay una etiqueta "MG Inherited" junto a la directiva predeterminada, significa que la directiva se ha asignado a un grupo de administración y que la ha heredado la suscripción que está viendo.


1. Elija entre las opciones disponibles en esta página:

    1. Para usar directivas del sector, haga clic en **Agregar más estándares**. Para obtener más información, consulte [Actualizar a los paquetes de cumplimiento dinámicos](update-regulatory-compliance-packages.md).

    1. Para asignar y administrar iniciativas personalizadas, haga clic en **Agregar iniciativas personalizadas**. Para obtener más información, consulte [Usar directivas de seguridad personalizadas](custom-security-policies.md).

    1. Para ver y editar la directiva predeterminada, haga clic en **Ver directivas vigentes** y continúe tal como se describe a continuación. 

       ![pantalla de la directiva](./media/security-center-policies/policy-screen.png)
       
       La pantalla de **directiva de seguridad** refleja las acciones que realizaron las directivas asignadas en el grupo de administración o la suscripción que seleccionó.
       
       * En la parte superior, use los vínculos proporcionados para abrir cada **asignación** de directiva que se aplique al grupo de administración o la suscripción. Puede usar esos vínculos para obtener acceso a la asignación y editar o deshabilitar la directiva. Por ejemplo, si ve que una asignación de directiva determinada está denegando la protección de puntos de conexión, puede usar el vínculo para editar o deshabilitar la directiva.
       
       * En la lista de directivas, puede ver la aplicación efectiva de la directiva en su suscripción o grupo de administración. Esto significa que se tiene en cuenta la configuración de cada directiva que se aplica al ámbito y se proporciona el resultado acumulado de la acción que realiza esa directiva. Por ejemplo, si en una asignación la directiva está deshabilitada, pero en otra está definida en AuditIfNotExist, el efecto acumulado aplica AuditIfNotExist. El efecto más activo siempre tiene prioridad.
       
       * El efecto de las directivas puede ser: Append, Audit, AuditIfNotExists, Deny, DeployIfNotExists, Disabled. Para más información sobre cómo se aplican los efectos, consulte [Descripción de los efectos de Policy](../governance/policy/concepts/effects.md).

       > [!NOTE]
       > Cuando vea directivas asignadas, puede ver varias asignaciones y cómo se configura cada asignación individualmente.


## <a name="who-can-edit-security-policies"></a>¿Quién puede editar directivas de seguridad?

Puede editar directivas de seguridad mediante el portal de Azure Policy, la API REST o con Windows PowerShell.

Security Center usa el control de acceso basado en rol (RBAC), que proporciona roles integrados que se pueden asignar a usuarios, grupos y servicios en Azure. Cuando un usuario abre Security Center, solo ve la información relacionada con los recursos a los que tiene acceso. Esto significa que a los usuarios se les asigna el rol de *propietario*, *colaborador* o *lector* para la suscripción del recurso. Además de estos roles, existen dos roles específicos de Security Center:

- **Lector de seguridad**: el usuario tiene derecho a visualizar el contenido de Security Center (recomendaciones, alertas, directivas y estados) pero no puede realizar cambios.
- **Administrador de seguridad**: tiene los mismos derechos que el *lector de seguridad*, pero también puede actualizar la directiva de seguridad o descartar recomendaciones y alertas.


## <a name="disable-security-policies"></a>Deshabilitar las directivas de seguridad
Si la directiva de seguridad predeterminada genera una recomendación que no es pertinente para su entorno, puede deshabilitar la definición de directiva que envía la recomendación para detenerla.
Para más información sobre las recomendaciones, consulte [Administración de las recomendaciones de seguridad](security-center-recommendations.md).

1. En Security Center, en **DIRECTIVA Y CUMPLIMIENTO**, seleccione **Directiva de seguridad**.

   ![administración de directivas](./media/tutorial-security-policy/policy-management.png)

2. Haga clic en el grupo de administración o de suscripción para el que quiere deshabilitar la recomendación.

   > [!NOTE]
   > Recuerde que un grupo de administración aplica sus directivas a sus suscripciones. Por lo tanto, si deshabilita la directiva de una suscripción y la suscripción pertenece a un grupo de administración que usa la misma directiva, seguirá recibiendo las recomendaciones de directivas. La directiva se seguirá aplicando desde el nivel de administración y las recomendaciones se seguirán generando.

1. Haga clic en **Ver directiva efectiva**.

   ![deshabilitar la directiva](./media/tutorial-security-policy/view-effective-policy.png)

1. Haga clic en la directiva asignada.

   ![deshabilitar la directiva](./media/tutorial-security-policy/security-policy.png)

1. En la sección **PARÁMETROS**, busque la directiva que invoca la recomendación que quiere deshabilitar y, en la lista desplegable, seleccione **Deshabilitado**

   ![deshabilitar la directiva](./media/tutorial-security-policy/disable-policy.png)

1. Haga clic en **Save**(Guardar).

   > [!NOTE]
   > Los cambios de deshabilitación de directiva pueden tardar hasta 12 horas en surtir efecto.



## <a name="next-steps"></a>Pasos siguientes
En este artículo obtuvo información sobre las directivas de seguridad. Para obtener más información relacionada, consulte los siguientes artículos:

* Para obtener instrucciones sobre cómo establecer directivas con PowerShell, consulte [Inicio rápido: creación de una asignación de directiva para identificar recursos no compatibles mediante el módulo de Azure PowerShell](../governance/policy/assign-policy-powershell.md).

* Para obtener instrucciones sobre cómo editar una directiva de seguridad en Azure Policy, consulte [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md).

* Para obtener instrucciones sobre cómo establecer una directiva entre suscripciones o grupos de administración mediante Azure Policy, consulte [¿Qué es Azure Policy?](../governance/policy/overview.md)