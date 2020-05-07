---
title: Control del mantenimiento de máquinas virtuales de Azure mediante Azure Portal
description: Aprenda a controlar cuándo se aplica el mantenimiento a las máquinas virtuales de Azure mediante el control de mantenimiento y Azure Portal.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/22/2020
ms.author: cynthn
ms.openlocfilehash: c0cb4800bdabe5eb500422fca55b3060b6422e8e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82138895"
---
# <a name="control-updates-with-maintenance-control-and-the-azure-portal"></a>Control de las actualizaciones con el control de mantenimiento y Azure Portal

El control de mantenimiento permite decidir cuándo se aplican las actualizaciones tanto a las máquinas virtuales aisladas como a los hosts dedicados de Azure. En este tema se tratan las opciones de Azure Portal para el control de mantenimiento. Para más información sobre las ventajas de usar el control de mantenimiento, sus limitaciones y otras opciones de administración, consulte el artículo acerca de la [administración de las actualizaciones de las distintas plataformas con control de mantenimiento](maintenance-control.md).

## <a name="create-a-maintenance-configuration"></a>Creación de una configuración de mantenimiento

1. Inicie sesión en Azure Portal.

1. Busque **Configuraciones de mantenimiento**.

   ![Captura de pantalla en la que se muestra cómo abrir Configuraciones de mantenimiento](media/virtual-machines-maintenance-control-portal/maintenance-configurations-search.png)

1. Haga clic en **Agregar**.

   ![Captura de pantalla en la que se muestra cómo agregar una configuración de mantenimiento](media/virtual-machines-maintenance-control-portal/maintenance-configurations-add.png)

1. Elija una suscripción y un grupo de recursos, especifique un nombre para la configuración y elija una región. Haga clic en **Next**.

   ![Captura de pantalla en la que se muestran los elementos básicos de las configuraciones de mantenimiento](media/virtual-machines-maintenance-control-portal/maintenance-configurations-basics.png)

1. Agregue las etiquetas y los valores. Haga clic en **Next**.

   ![Captura de pantalla en la que se muestra cómo agregar etiquetas a una configuración de mantenimiento](media/virtual-machines-maintenance-control-portal/maintenance-configurations-tags.png)

1. Revise el resumen. Haga clic en **Crear**.

   ![Captura de pantalla en la que se muestra cómo crear una configuración de mantenimiento](media/virtual-machines-maintenance-control-portal/maintenance-configurations-create.png)

1. Cuando termine la implementación, haga clic en **Ir al recurso**.

   ![Captura de pantalla en la que se muestra que la implementación de la configuración de mantenimiento ha finalizado](media/virtual-machines-maintenance-control-portal/maintenance-configurations-deployment-complete.png)

## <a name="assign-the-configuration"></a>Asignación de la configuración

En la página de detalles de la configuración de mantenimiento, haga clic en Asignaciones y, después, en **Asignar recurso**. 

![Captura de pantalla en la que muestra cómo asignar un recurso](media/virtual-machines-maintenance-control-portal/maintenance-configurations-add-assignment.png)

Seleccione los recursos a los que desea asignar la configuración de mantenimiento y haga clic en **Aceptar**. En la columna **Tipo** se muestra si el recurso es una máquina virtual aislada o un host dedicado de Azure. Para asignar la configuración, es preciso que la máquina virtual esté en ejecución. Si se intenta asignar una configuración a una máquina virtual que está detenida, se produce un error. 

<!---Shantanu to add details about the error case--->

![Captura de pantalla en la que muestra cómo seleccionar un recurso](media/virtual-machines-maintenance-control-portal/maintenance-configurations-select-resource.png)

## <a name="check-configuration"></a>Comprobación de configuración

Desde **Configuraciones de mantenimiento** no solo se puede comprobar si la configuración se ha aplicado correctamente, sino que también se pueden ver todas las configuraciones de mantenimiento que está asignadas actualmente. En la columna **Tipo** se muestra si la configuración está asignada a una máquina virtual aislada o a un host dedicado de Azure. 

![Captura de pantalla en la que se muestra cómo comprobar una configuración de mantenimiento](media/virtual-machines-maintenance-control-portal/maintenance-configurations-host-type.png)

También puede comprobar la configuración de una máquina virtual específica en su página de propiedades. Haga clic en **Mantenimiento** para ver la configuración asignada a esa máquina virtual.

![Captura de pantalla en la que se muestra cómo comprobar el mantenimiento de un host](media/virtual-machines-maintenance-control-portal/maintenance-configurations-check-config.png)

## <a name="check-for-pending-updates"></a>Búsqueda de actualizaciones pendientes

También hay dos maneras de comprobar si las actualizaciones tienen una configuración de mantenimiento pendiente. En **Configuraciones de mantenimiento**, en los detalles de la configuración, haga clic en **Asignaciones** y vea el valor de **Estado de mantenimiento**.

![Captura de pantalla en la que se muestra cómo comprobar las actualizaciones pendientes](media/virtual-machines-maintenance-control-portal/maintenance-configurations-pending.png)

También puede comprobar un host específico mediante **Virtual Machines** o las propiedades del host dedicado. 

![Captura de pantalla en la que se muestra cómo comprobar el mantenimiento de un host](media/virtual-machines-maintenance-control-portal/maintenance-configurations-pending-vm.png)

## <a name="apply-updates"></a>Aplicación de actualizaciones

Las actualizaciones pendientes se pueden aplicar a petición mediante **Virtual Machines**. En los detalles de la máquina virtual, haga clic en **Mantenimiento** y, después, en **Aplicar mantenimiento ahora**.

![Captura de pantalla en la que se muestra cómo aplicar las actualizaciones pendientes](media/virtual-machines-maintenance-control-portal/maintenance-configurations-apply-updates-now.png)

## <a name="check-the-status-of-applying-updates"></a>Comprobación del estado de aplicación de las actualizaciones 

El progreso de las actualizaciones de una configuración se puede consultar en **Configuraciones de mantenimiento** o en **Virtual Machines**. En los detalles de la máquina virtual, haga clic en **Mantenimiento**. En el ejemplo siguiente, **Estado de mantenimiento**  muestra que una actualización está en estado **Pendiente**.

![Captura de pantalla en la que muestra cómo comprobar el estado de las actualizaciones pendientes](media/virtual-machines-maintenance-control-portal/maintenance-configurations-status.png)

## <a name="delete-a-maintenance-configuration"></a>Eliminación de una configuración de mantenimiento

Para eliminar una configuración, abra los detalles de la misma y haga clic en **Eliminar**.

![Captura de pantalla en la que se muestra cómo comprobar el mantenimiento de un host](media/virtual-machines-maintenance-control-portal/maintenance-configurations-delete.png)


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte [Mantenimiento y actualizaciones](maintenance-and-updates.md).
