---
title: Habilitación de Azure Monitor para VM mediante Azure Policy
description: En este artículo se describe cómo habilitar Azure Monitor para VM para varias máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure mediante Azure Policy.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 73c18d45136eea90ad29dc1bd40c4539dddc0ee6
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81767258"
---
# <a name="enable-azure-monitor-for-vms-by-using-azure-policy"></a>Habilitación de Azure Monitor para VM mediante Azure Policy

En este artículo se explica cómo habilitar Azure Monitor para VM para máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure mediante Azure Policy. Al final de este proceso, habrá configurado correctamente la habilitación de Log Analytics y los agentes de Dependency Agent e identificado las máquinas virtuales que no son compatibles.

Para detectar, administrar y habilitar Azure Monitor para VM para todas las máquinas virtuales o conjuntos de escalado de máquinas virtuales puede usar Azure Policy o Azure PowerShell. Azure Policy es el método recomendado porque le permite administrar las definiciones de la directiva para controlar de forma eficaz sus suscripciones de forma que se garantice un cumplimiento coherente y se habiliten automáticamente las VM recién aprovisionadas. Estas definiciones de directivas:

* Implementan el agente de Log Analytics y Dependency Agent.
* Informan sobre los resultados de cumplimiento.
* Corrigen las máquinas virtuales no compatibles.

Si está interesado en llevar a cabo estas tareas con Azure PowerShell o con una plantilla de Azure Resource Manager, vea [Habilitar Azure Monitor para VM mediante Azure PowerShell o una plantilla de Resource Manager](vminsights-enable-at-scale-powershell.md).

## <a name="prerequisites"></a>Prerrequisitos
Antes de usar la Directiva para incorporar las máquinas virtuales de Azure y los conjuntos de escalado de máquinas virtuales a Azure Monitor para VM, debe habilitar la solución VMInsights en el área de trabajo que usará para almacenar los datos de supervisión. Esta tarea se puede completar desde la página **Introducción** de Azure Monitor en la pestaña **Other onboarding options** (Otras opciones de incorporación).  Seleccione **Configurar un área de trabajo**; al elegir esta opción se le pedirá que seleccione el área de trabajo que se va a configurar.

![Configuración del área de trabajo](media/vminsights-enable-at-scale-policy/configure-workspace.png)

También puede configurar el área de trabajo eligiendo **Enable using policy** (Habilitar mediante directiva) y luego seleccionando el botón de la barra de herramientas **Configurar área de trabajo**.  Se instalará la solución VMInsights en el área de trabajo seleccionada, lo que permitirá que dicha área almacene los datos de supervisión enviados por las máquinas virtuales y los conjuntos de escalado de máquinas virtuales que se habilitan mediante Azure Policy. 

![Habilitar mediante directiva](media/vminsights-enable-at-scale-policy/enable-using-policy.png)

## <a name="manage-policy-coverage-feature-overview"></a>Información general a la característica Administrar la cobertura de la directiva

La cobertura de la directiva de Azure Monitor para VM simplifica la detección, administración y habilitación a escala de la iniciativa **Habilitar Azure Monitor para VM**, que incluye las definiciones de directiva se ha mencionadas anteriormente. Para acceder a esta característica, seleccione **Other onboarding options** (Otras opciones de incorporación) en la pestaña **Introducción** de Azure Monitor para VM. Seleccione **Administrar la cobertura de la directiva** para abrir la página **Cobertura de directiva de Azure Monitor para VM**.

![Azure Monitor desde la pestaña Primeros pasos de las VM](./media/vminsights-enable-at-scale-policy/get-started-page.png)

Desde aquí, puede comprobar y administrar la cobertura de la iniciativa a través de las suscripciones y grupos de administración. Puede conocer cuántas VM existen en cada una de las suscripciones y grupos de administración, y su estado de cumplimiento.

![Página de administración de máquinas virtuales de Azure Monitor para VM](media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Esta información es útil para ayudarle a planear y ejecutar el escenario de gobernanza de Azure Monitor para VM desde una ubicación central. Mientras que Azure Policy proporciona una vista de cumplimiento cuando se asigna una directiva o iniciativa a un ámbito, con esta nueva página puede detectar el lugar donde la directiva o iniciativa no está asignada y asignarla en su lugar. Todas las acciones, como asignar, ver y editar redirigen directamente a Azure Policy. La página **Azure Monitor for VMs Policy Coverage** (Cobertura de directiva de Azure Monitor para VM) es una experiencia integrada y expandida solo para la iniciativa **Habilitar Azure Monitor para VM**.

Desde esta página, también puede configurar el área de trabajo de Log Analytics de Azure Monitor para VM, el cual:

- Instala la solución Service Map.
- Habilita los contadores de rendimiento del sistema operativo utilizados por los gráficos de rendimiento, los libros y las alertas y consultas del registro personalizado.

![Configuración del área de trabajo de Azure Monitor para VM](media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Esta opción no está relacionada con las acciones de directiva. Está disponible para proporcionar una manera fácil de satisfacer los [requisitos previos](vminsights-enable-overview.md) necesarios para habilitar Azure Monitor para VM.  

### <a name="what-information-is-available-on-this-page"></a>¿Qué información está disponible en esta página?

En la tabla siguiente se proporciona un desglose de la información que se presenta en la página de cobertura de la directiva y cómo interpretarla.

| Función | Descripción | 
|----------|-------------| 
| **Ámbito** | Grupo de administración y suscripciones a las que tenga o haya heredado el acceso con capacidad de explorar en profundidad la jerarquía de grupos de administración.|
| **Rol** | Rol para el ámbito, que puede ser lector, propietario o colaborador. En algunos casos, puede aparecer en blanco para indicar que es posible acceder a la suscripción pero no al grupo de administración al que pertenece. La información de las otras columnas varía en función de su rol. El rol es clave para determinar qué datos puede ver y qué acciones puede realizar en cuanto a la asignación de directivas e iniciativas (propietario), editarlas o ver el cumplimiento. |
| **TOTAL DE VM** | Número de VM en ese ámbito. Para un grupo de administración, es una suma de las VM anidadas bajo las suscripciones o el grupo de administración secundario. |
| **Cobertura de la asignación** | Porcentaje de VM que están cubiertas por la directiva o iniciativa. |
| **Estado de la asignación** | Información sobre el estado de su asignación de directiva o iniciativa. |
| **Máquinas virtuales compatibles** | Número de VM que son compatibles con la directiva o iniciativa. Para la iniciativa **Habilitar Azure Monitor para VM**, es el número de máquinas virtuales que tienen el agente de Log Analytics y el agente de dependencia. En algunos casos, podría parecer en blanco porque no hay ninguna asignación, ninguna VM o no tiene permisos suficientes. Se proporciona información en **Estado de cumplimiento**. |
| **Cumplimiento normativo** | La cifra de cumplimiento general es la suma de los distintos recursos que son Conforme dividida por la suma de todos los recursos distintos. |
| **Estado de cumplimiento** | Información sobre el estado de cumplimiento de su asignación de directiva o iniciativa.|

Al asignar la directiva o iniciativa, el ámbito seleccionado en la asignación podría ser el ámbito que se muestra o un subconjunto del mismo. Por ejemplo, es posible que haya creado una asignación para una suscripción (ámbito de la directiva) y no un grupo de administración (ámbito de la cobertura). En este caso, el valor de **cobertura de la asignación** indica el número de VM del ámbito de la directiva o iniciativa dividido entre las VM del ámbito de la cobertura. En otro caso, es posible que haya excluido algunas VM, grupos de recursos o una suscripción del ámbito de la directiva. Si el valor está en blanco, indica que la directiva o la iniciativa no existe o no tiene permiso. Se proporciona información en **Estado de asignación**.

## <a name="enable-by-using-azure-policy"></a>Habilitación mediante Azure Policy

Para habilitar Azure Monitor para VM mediante Azure Policy en su inquilino:

- Asigne la iniciativa a un ámbito: grupo de administración, suscripción o grupo de recursos.
- Revise y corrección corrija resultados del cumplimiento.

Para más información acerca de la asignación de Azure Policy, consulte [Información general de Azure Policy](../../governance/policy/overview.md#assignments) y consulte la [introducción a los grupos de administración](../../governance/management-groups/overview.md) antes de continuar.

### <a name="policies-for-azure-vms"></a>Directivas para las máquinas virtuales de Azure

En la tabla siguiente se enumeran las definiciones de directiva para una VM de Azure:

|Nombre |Descripción |Tipo |
|-----|------------|-----|
|Habilitar Azure Monitor para VM |Se habilita Azure Monitor para las máquinas virtuales del ámbito especificado (grupo de administración, suscripción o grupo de recursos). Toma el área de trabajo de Log Analytics como parámetro. |Iniciativa |
|Auditoría de implementación de Dependency Agent: imagen de la VM (SO) no mostrada |Notifica que las máquinas virtuales no son compatibles si la imagen de la máquina virtual (SO) no está definida en la lista y el agente no está instalado. |Directiva |
|Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista |Notifica que las máquinas virtuales no son compatibles si la imagen de la máquina virtual (SO) no está definida en la lista y el agente no está instalado. |Directiva |
|Implementar Dependency Agent en máquinas virtuales Linux |Se implementa Dependency Agent en las máquinas virtuales Linux si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |
|Implementar Dependency Agent en máquinas virtuales Windows |Se implementa Dependency Agent en las máquinas virtuales Windows si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |
|Implementar el agente de Log Analytics en máquinas virtuales de Linux |Se implementa el agente de Log Analytics en máquinas virtuales Linux si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |
|Implementar el agente de Log Analytics en máquinas virtuales Windows |Se implementa el agente de Log Analytics en máquinas virtuales Windows si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Directivas para conjuntos de escalado de máquinas virtuales de Azure

En la tabla siguiente se enumeran las definiciones de directiva para un conjunto de escalado de máquinas virtuales de Azure:

|Nombre |Descripción |Tipo |
|-----|------------|-----|
|Habilitar Azure Monitor para conjunto de escalado de máquinas virtuales |Se habilita Azure Monitor para los conjuntos de escalado de máquinas virtuales en el ámbito especificado (grupo de administración, suscripción o grupo de recursos). Toma el área de trabajo de Log Analytics como parámetro. Nota: Si establece la directiva Actualizar el conjunto de escalado en Manual, aplique la extensión a todas las máquinas virtuales del conjunto mediante una llamada a la actualización. En el CLI, esto es `az vmss update-instances`. |Iniciativa |
|Auditoría de implementación de Dependency Agent en conjuntos de escalado de máquinas virtuales: la imagen de la VM (SO) no está en la lista |Notifica que los conjuntos de escalado de máquinas virtuales no son compatibles si la imagen de la máquina virtual (SO) no está definida en la lista y el agente no está instalado. |Directiva |
|Auditoría de implementación del agente de Log Analytics en conjuntos de escalado de máquinas virtuales: la imagen de la VM (SO) no está en la lista |Notifica que los conjuntos de escalado de máquinas virtuales no son compatibles si la imagen de la máquina virtual (SO) no está definida en la lista y el agente no está instalado. |Directiva |
|Implementar Dependency Agent para conjuntos de escalado de máquinas virtuales Linux |Se implementa Dependency Agent en los conjuntos de escalado de máquinas virtuales Linux si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |
|Implementar Dependency Agent para conjuntos de escalado de máquinas virtuales Windows |Se implementa Dependency Agent en los conjuntos de escalado de máquinas virtuales Windows si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |
|Implementar el agente de Log Analytics para conjuntos de escalado de máquinas virtuales Linux |Se implementa el agente de Log Analytics en los conjuntos de escalado de máquinas virtuales Linux si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |
|Implementar el agente de Log Analytics para conjuntos de escalado de máquinas virtuales Windows |Se implementa el agente de Log Analytics en los conjuntos de escalado de máquinas virtuales Windows si la imagen de la máquina virtual (SO) está en la lista definida y el agente no está instalado. |Directiva |

La directiva independiente (no se incluye con la iniciativa) se describe a continuación:

|Nombre |Descripción |Tipo |
|-----|------------|-----|
|Auditar área de trabajo de Log Analytics en la máquina virtual: error de coincidencia del informe |Se notifica que las máquinas virtuales no son compatibles si no se registran en el área de trabajo de Log Analytics especificada en la asignación de la directiva o iniciativa. |Directiva |

### <a name="assign-the-azure-monitor-initiative"></a>Asignar la iniciativa de Azure Monitor

Para crear la asignación de directiva desde la página **Cobertura de directiva de Azure Monitor para VM**, siga estos pasos. Para saber cómo completar estos pasos, consulte  [Create a policy assignment from the Azure portal](../../governance/policy/assign-policy-portal.md) (Creación de una asignación de directiva desde Azure Portal).

Al asignar la directiva o iniciativa, el ámbito seleccionado en la asignación podría ser el ámbito que se muestra aquí o un subconjunto del mismo. Por ejemplo, es posible que haya creado una asignación para la suscripción (ámbito de la directiva) y no el grupo de administración (ámbito de la cobertura). En este caso, el porcentaje de cobertura indicaría el número de VM del ámbito de la directiva o iniciativa dividido entre las VM del ámbito de la cobertura. En otro caso, es posible que haya excluido algunas VM, grupos de recursos o una suscripción del ámbito de la directiva. Si está en blanco, indica que la directiva o la iniciativa no existe o no tiene permiso. Se proporciona información en **Estado de asignación**.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. En Azure Portal, seleccione **Monitor**. 

3. Elija **Máquinas virtuales** en la sección **Insights**.
 
4. Seleccione la pestaña **Introducción**. En la página, seleccione **Administrar la cobertura de la directiva**.

5. Seleccione un grupo de administración o una suscripción de la tabla. Seleccione **Ámbito** haciendo clic en el botón de puntos suspensivos (...). En el ejemplo, un ámbito limita la asignación de directiva a una agrupación de máquinas virtuales para su aplicación.

6. La página **Asignación de Azure Policy**, está rellenada previamente con la iniciativa **Habilitar Azure Monitor para VM**. 
    El cuadro **Nombre de asignación** se ha rellenado automáticamente con el nombre de la iniciativa, pero puede cambiarlo. También puede agregar una descripción, si lo desea. El cuadro **Asignado por** se rellena automáticamente en función de quién haya iniciado sesión. Este valor es opcional.

7. (Opcional) Para quitar uno o varios recursos del ámbito, seleccione **Exclusiones**.

8. En la lista desplegable **Área de trabajo de Log Analytics** para la región admitida, seleccione un área de trabajo.

   > [!NOTE]
   > Si el área de trabajo está fuera del ámbito de la asignación, conceda los permisos de *colaborador de Log Analytics* al identificador de la entidad de seguridad. de la asignación de la directiva. Si no lo hace, es posible que vea un error de implementación, como: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` Para conceder acceso, consulte [cómo configurar manualmente la identidad administrada](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  La casilla **Identidad administrada** se selecciona porque la iniciativa que se asigna incluye una directiva con el efecto *deployIfNotExists*.
    
9. En la lista desplegable **Ubicación de la identidad administrada**, seleccione la región adecuada.

10. Seleccione **Asignar**.

Después de crear la asignación, la página **Cobertura de directiva de Azure Monitor para VM** actualiza **Cobertura de la asignación**, **Estado de la asignación**, **VM compatibles** y **Estado de cumplimiento** para que reflejen los cambios. 

La siguiente matriz asigna cada estado de cumplimiento posible a la iniciativa.  

| Estado de cumplimiento | Descripción | 
|------------------|-------------|
| **Compatible** | Todas las VM del ámbito tienen los agentes de Log Analytics y Dependency Agent implementados en ellas.|
| **No compatible** | No todas las VM del ámbito tienen los agentes de Log Analytics y Dependency Agent implementados en ellas y puede ser necesaria una corrección.|
| **No iniciado** | Se ha agregado una nueva asignación. |
| **Bloquear** | No tiene suficientes privilegios para el grupo de administración.<sup>1</sup> | 
| **En blanco** | No se ha asignado ninguna directiva. | 

<sup>1</sup> Si no tiene acceso al grupo de administración, solicite a un propietario que se lo proporcione. O bien, vea el cumplimiento y administre las asignaciones a través de suscripciones o grupos de administración secundarios. 

En la tabla siguiente se asigna cada estado de la asignación posible a la iniciativa.

| Estado de la asignación | Descripción | 
|------------------|-------------|
| **Success** | Todas las VM del ámbito tienen los agentes de Log Analytics y Dependency Agent implementados en ellas.|
| **Warning (ADVERTENCIA)** | La suscripción no está en un grupo de administración.|
| **No iniciado** | Se ha agregado una nueva asignación. |
| **Bloquear** | No tiene suficientes privilegios para el grupo de administración.<sup>1</sup> | 
| **En blanco** | No existe ninguna VM o no se ha asignado una directiva. | 
| **Acción** | Asigne una directiva o edite una asignación. | 

<sup>1</sup> Si no tiene acceso al grupo de administración, solicite a un propietario que se lo proporcione. O bien, vea el cumplimiento y administre las asignaciones a través de suscripciones o grupos de administración secundarios.

## <a name="review-and-remediate-the-compliance-results"></a>Revisión y corrección de los resultados del cumplimiento

El ejemplo siguiente es para una máquina virtual de Azure, pero también se aplica a conjuntos de escalado de máquinas virtuales. Para aprender a revisar los resultados de cumplimiento, vea [identificación de los resultados de no cumplimiento](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). En la página **Cobertura de directiva de Azure Monitor para VM**, seleccione un grupo de administración o una suscripción de la tabla. Seleccione **Ver compatibilidad** haciendo clic en el botón de puntos suspensivos (...).   

![Cumplimiento de directivas en máquinas virtuales de Azure](./media/vminsights-enable-at-scale-policy/policy-view-compliance.png)

En función de los resultados de las directivas incluidas con la iniciativa, las máquinas virtuales se notifican como no compatibles en los siguientes escenarios:

* No están implementados los agentes de Log Analytics o Dependency Agent.  
    Este escenario es típico de un ámbito con las máquinas virtuales existentes. Para mitigarlo, implemente los agentes necesarios. Para ello, [cree tareas de corrección](../../governance/policy/how-to/remediate-resources.md) en una directiva no compatible.  
    - Implementar Dependency Agent en máquinas virtuales Linux
    - Implementar Dependency Agent en máquinas virtuales Windows
    - Implementar el agente de Log Analytics en máquinas virtuales de Linux
    - Implementar el agente de Log Analytics en máquinas virtuales Windows

* La imagen de la máquina virtual (SO) no está identificada en la definición de la directiva.  
    Los criterios de la directiva de implementación solo incluyen las máquinas virtuales que se implementan a partir de imágenes de máquina virtual de Azure conocidas. Consulte en la documentación si el sistema operativo de VM es compatible. Si no lo es, duplique la directiva de implementación y actualícela o modifíquela para que la imagen sea compatible.  
    - Auditoría de implementación de Dependency Agent: imagen de la VM (SO) no mostrada
    - Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista

* Las máquinas virtuales no inician sesión en el área de trabajo de Log Analytics especificada.  
    Es posible que algunas máquinas virtuales en el ámbito de la iniciativa inicien sesión en un área de trabajo de Log Analytics distinta de la que se especifica en la asignación de directiva. Esta directiva es una herramienta para identificar qué máquinas virtuales notifican a un área de trabajo no compatible.  
    - Auditar área de trabajo de Log Analytics en la máquina virtual: error de coincidencia del informe

## <a name="edit-an-initiative-assignment"></a>Editar una asignación de iniciativa

En cualquier momento después de asignar una iniciativa a un grupo de administración o una suscripción, puede editarla para modificar las propiedades siguientes:

- Nombre de asignación
- Descripción
- Asignada por
- Área de trabajo de Log Analytics
- Excepciones

## <a name="next-steps"></a>Pasos siguientes

Ahora que la supervisión está habilitada para las máquinas virtuales, esta información está disponible para analizarse con Azure Monitor para VM. 

- Para ver las dependencias de las aplicaciones detectadas, consulte [Uso de la asignación de Azure Monitor para VM (versión preliminar) para conocer los componentes de una aplicación](vminsights-maps.md). 

- Para identificar los cuellos de botella y el uso general con el rendimiento de la máquina virtual, vea [Cómo representar el rendimiento en gráficos con Azure Monitor para VM (versión preliminar)](vminsights-performance.md). 
