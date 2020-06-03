---
title: Habilitación de la característica Start/Stop VMs during off-hours de Azure Automation
description: En este artículo se indica cómo habilitar la característica Start/Stop VMs during off-hours en las máquinas virtuales de Azure.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: dde2c3e4cf496bb15ca91c72d9a41936af7051c5
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743756"
---
# <a name="enable-startstop-vms-during-off-hours"></a>Habilitación de Start/Stop VMs during off-hours

Siga los pasos de este tema en orden para habilitar la característica Start/Stop VMs during off-hours en las máquinas virtuales con una cuenta de Automation y un área de trabajo de Log Analytics vinculada nuevas o existentes. Después de completar el proceso de instalación, configure las variables para personalizar la característica.

>[!NOTE]
>Para usar esta característica con máquinas virtuales clásicas, necesita una cuenta de ejecución clásica, que no se crea de forma predeterminada. Consulte [Creación de una cuenta de ejecución clásica](automation-create-standalone-account.md#create-a-classic-run-as-account).
>

## <a name="create-resources-for-the-feature"></a>Creación de recursos para la característica

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Busque y seleccione **Cuentas de Automation**.
3. En la página Cuentas de Automation, seleccione su cuenta de Automation en la lista.
4. En la cuenta de Automation, seleccione **Start/Stop VM** en **Recursos relacionados**. Desde aquí, puede hacer clic en **Más información acerca de la solución y cómo habilitarla**. Si ya tiene implementada la característica, puede hacer clic en **Manage the solution** (Administrar la solución) y buscarla en la lista.

   ![Habilitación desde una cuenta de Automation](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > También puede crear el recurso desde cualquier lugar de Azure Portal; para ello, haga clic en **Crear un recurso**. En la página de Marketplace, escriba una palabra clave como **Iniciar** o **Inciar/Detener**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Como alternativa, puede escribir una o varias palabras clave del nombre completo de la característica y, luego, presionar **Entrar**. Seleccione **Start/Stop VMs during off-hours** en los resultados de la búsqueda.

5. En la página Start/Stop VMs during off-hours de la implementación seleccionada, revise la información de resumen y, luego, haga clic en **Crear**.

   ![Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)

## <a name="configure-the-feature"></a>Configuración de la característica

Con el recurso creado, aparece la página Agregar solución. Se le pide que configure la solución antes de importarla en la suscripción de Automation. Consulte [Configuración de la solución Start/Stop VMs during off-hours](automation-solution-vm-management-config.md).

   ![Página Agregar solución de administración de máquinas virtuales](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

## <a name="select-a-log-analytics-workspace"></a>Selección de un área de trabajo de Log Analytics

1. En la página Agregar solución, seleccione **Área de trabajo**. Seleccione un área de trabajo de Log Analytics que esté vinculada a la suscripción de Azure que usa la cuenta de Automation. 

2. Si no tiene ningún área de trabajo, seleccione **Crear nueva área de trabajo**. En la página Área de trabajo de Log Analytics, siga estos pasos:

   - Especifique un nombre para la nueva área de trabajo de Log Analytics como, por ejemplo, **ContosoLAWorkspace**.
   - Seleccione la **suscripción** a la que vincularlo en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
   - En **Grupo de recursos**, puede crear un grupo de recursos nuevo o seleccionar uno existente.
   - Seleccione una **ubicación**.
   - Seleccione un **plan de tarifa**. Elija la opción **Por GB (independiente)** . Los registros de Azure Monitor tienen [precios](https://azure.microsoft.com/pricing/details/log-analytics/) actualizados y el nivel por GB es la única opción.

   > [!NOTE]
   > Al habilitar las características, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

3. Después de proporcionar la información necesaria en la página Área de trabajo de Log Analytics, haga clic en **Crear**. Se puede realizar un seguimiento de su progreso en la opción **Notificaciones** del menú, que le devuelve a la página Agregar solución cuando haya finalizado.

## <a name="add-automation-account"></a>Agregar cuenta de Automation

Acceda de nuevo a la página Agregar solución y seleccione **Cuenta de Automation**. Puede seleccionar una cuenta de Automation existente que no esté aún vinculada a un área de trabajo de Log Analytics. Si va a crear un área de trabajo de Log Analytics, puede crear una cuenta de Automation a la que asociarla. Seleccione una cuenta de Automation existente o haga clic en **Crear una cuenta de automatización** y, en la página Add Automation account (Agregar cuenta de Automation), proporcione el nombre de la cuenta de Automation en el campo **Nombre**.

Las restantes opciones se rellenan automáticamente en función del área de trabajo de Log Analytics seleccionada. No se pueden modificar estas opciones. Una cuenta de ejecución de Azure es el método de autenticación predeterminado para los runbooks incluidos en esta característica. 

Después de hacer clic en **Aceptar**, se validan las opciones de configuración y se crea la cuenta de Automation. Puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

## <a name="define-feature-parameters"></a>Definición de los parámetros de la característica

1. En la página Agregar solución, seleccione **Configuración**. Aparece la página Parámetros.

    ![Página Parámetros para la solución](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

2. Especifique un valor para el campo **Target ResourceGroup Names** (Nombres de grupos de recursos de destino). El campo define los nombres de grupos que contienen las máquinas virtuales que se van a administrar con la característica. Puede especificar varios nombres y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se puede usar un carácter comodín si quiere dirigirse a las máquinas virtuales de todos los grupos de recursos de la suscripción. Los valores se almacenan en las variables `External_Start_ResourceGroupNames` y `External_Stop_ResourceGroupNames`.

    > [!IMPORTANT]
    > El valor predeterminado de los **nombres del grupo de recursos de destino** es **&ast;** . Esta opción va dirigida a todas las máquinas virtuales de una suscripción. Si no desea que esto suceda, debe proporcionar una lista de nombres de grupos de recursos antes de seleccionar una programación.
  
3. Especifique un valor en el campo **VM Exclude List (string)** (Lista de exclusión de máquinas virtuales [cadena]). Este valor es el nombre de una o varias máquinas virtuales del grupo de recursos de destino. Puede especificar varios nombres y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se admite el uso de un carácter comodín. Este valor se almacena en la variable `External_ExcludeVMNames`.
  
4. Use el campo **Programación** para seleccionar una programación para la administración de máquinas virtuales mediante la característica. Seleccione la fecha y hora de inicio de la programación para crear una programación diaria periódica que comience a la hora seleccionada. No se puede seleccionar otra región. Para configurar la programación según una zona horaria específica después de configurar la característica, consulte [Modificación de las programaciones de inicio y apagado](automation-solution-vm-management-config.md#modify-the-startup-and-shutdown-schedules).

5. Para recibir notificaciones por correo electrónico de un [grupo de acciones](../azure-monitor/platform/action-groups.md), acepte el valor predeterminado de **Sí** en el campo **Notificaciones por correo electrónico** y proporcione una dirección de correo electrónico válida. Si selecciona **No** pero decide en un momento posterior que desea recibir notificaciones por correo electrónico, puede actualizar el grupo de acciones que se crea con direcciones de correo electrónico válidas separadas por coma. 

6. Habilite las reglas de alerta siguientes:

   - `AutoStop_VM_Child`
   - `Scheduled_StartStop_Parent`
   - `Sequenced_StartStop_Parent`

## <a name="create-alerts"></a>Creación de alertas

La característica Start/Stop VMs during off-hours no incluye un conjunto predefinido de alertas. Consulte el artículo [Creación de alertas de registro con Azure Monitor](../azure-monitor/platform/alerts-log.md) para aprender a crear alertas de errores de trabajo con el fin de ayudar a los procesos y procedimientos operativos o de DevOps.

## <a name="deploy-the-feature"></a>Habilitación de la característica

1. Después de configurar los valores iniciales necesarios de la característica, haga clic en **Aceptar** para cerrar la página Parámetros.

2. Haga clic en **Crear**. Cuando todos los valores se hayan validado, la característica se implementa en su suscripción. Este proceso puede tardar varios segundos en finalizar y se puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

    > [!NOTE]
    > Si tiene una suscripción del Proveedor de soluciones en la nube de Azure (CSP de Azure), una vez completada la implementación, acceda a su cuenta de Automation, vaya a **Variables** en **Shared Resources** (Recursos compartidos) y establezca la variable [External_EnableClassicVMs](automation-solution-vm-management.md#variables) en **False**. Esta operación detiene la solución para que deje de buscar recursos de la máquina virtual clásica.

## <a name="next-steps"></a>Pasos siguientes

* Para configurar la característica, consulte [Configuración de la solución Start/Stop VMs during off-hours](automation-solution-vm-management-config.md).
* Para resolver errores de la característica, consulte [Solución de problemas de Start/Stop VMs during off-hours](troubleshoot/start-stop-vm.md).