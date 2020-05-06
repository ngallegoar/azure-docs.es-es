---
title: Habilitación de la solución Start/Stop VMs during off-hours de Azure Automation
description: En este artículo se describe cómo habilitar la solución Start/Stop VM de Azure Automation para las máquinas virtuales de Azure.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: d47daa29c65f847fdeb33b9e24a892ac1f31b52a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82096969"
---
# <a name="enable-azure-automation-startstop-vms-solution"></a>Habilitación de la solución Start/Stop VMs de Azure Automation

Realice los siguientes pasos para agregar la solución **Start/Stop VMs during off-hours** a una cuenta de Automation nueva o a una ya existente que esté vinculada al área de trabajo de Log Analytics. Después de completar el proceso de incorporación, configure las variables para personalizar la solución.

>[!NOTE]
>Para usar esta solución con máquinas virtuales clásicas, necesita una cuenta de ejecución clásica, que no se crea de forma predeterminada. Para obtener instrucciones sobre cómo crear una cuenta de ejecución clásica, consulte [Creación de una cuenta de ejecución clásica](automation-create-standalone-account.md#create-a-classic-run-as-account).
>

## <a name="enable-solution"></a>Habilitar solución

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Busque y seleccione **Cuentas de Automation**.

3. En la página Cuentas de Automation, seleccione su cuenta de Automation en la lista.

4. En la cuenta de Automation, seleccione **Start/Stop VM** en **Recursos relacionados**. Desde aquí, puede hacer clic en **Más información acerca de la solución y cómo habilitarla**. Si ya tiene una solución Start/Stop VM implementada, puede seleccionarla; para ello, haga clic en **Administrar la solución** y búsquela en la lista.

   ![Habilitación desde una cuenta de Automation](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > También puede crearla desde cualquier lugar en Azure Portal; para ello, haga clic en **Crear un recurso**. En la página de Marketplace, escriba una palabra clave como **Iniciar** o **Inciar/Detener**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Como alternativa, puede escribir en una o varias palabras clave del nombre completo de la solución y presione Entrar. Seleccione **Start/Stop VMs during off-hours** en los resultados de la búsqueda.

5. En la página **Start/Stop VMs during off-hours**, revise la información de resumen y luego haga clic en **Crear**.

   ![Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)

6. Aparece la página Agregar solución. Se le pide que configure la solución antes de importarla en la suscripción de Automation.

   ![Página Agregar solución de administración de VM](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

7. En la página Agregar solución, seleccione **Área de trabajo**. Seleccione un área de trabajo de Log Analytics que esté vinculada a la suscripción de Azure en la que se encuentra la cuenta de Automation. Si no tiene ningún área de trabajo, seleccione **Crear nueva área de trabajo**. En la página Área de trabajo de Log Analytics, siga estos pasos:

   - Especifique un nombre para la nueva área de trabajo de Log Analytics como, por ejemplo, **ContosoLAWorkspace**.
   - Seleccione la **suscripción** a la que vincularlo en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
   - En **Grupo de recursos**, puede crear un grupo de recursos nuevo o seleccionar uno existente.
   - Seleccione una **ubicación**.
   - Seleccione un **plan de tarifa**. Elija la opción **Por GB (independiente)** . Los registros de Azure Monitor tienen [precios](https://azure.microsoft.com/pricing/details/log-analytics/) actualizados y el nivel por GB es la única opción.

   > [!NOTE]
   > Al habilitar las soluciones, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation.
   >
   > Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

8. Después de proporcionar la información necesaria en la página Área de trabajo de Log Analytics, haga clic en **Crear**. Se puede realizar un seguimiento de su progreso en la opción **Notificaciones** del menú, que le devuelve a la página Agregar solución cuando haya finalizado.

9. En la página Agregar solución, seleccione **Cuenta de Automation**. Si va a crear una nueva área de trabajo de Log Analytics, puede crear una cuenta de Automation para asociarla o seleccionar una cuenta de Automation existente que no esté ya vinculada a un área de trabajo de Log Analytics. Seleccione una cuenta de Automation existente o haga clic en **Crear una cuenta de Automation** y, en la página Agregar cuenta de Automation, especifique la siguiente información:
 
   - En el campo **Nombre**, escriba el nombre de la cuenta de Automation.

     Las restantes opciones se rellenan automáticamente en función del área de trabajo de Log Analytics seleccionada. Dichas opciones no se pueden modificar. Una cuenta de ejecución de Azure es el método de autenticación predeterminado para los Runbooks incluidos en esta solución. Después de hacer clic en **Aceptar**, se validan las opciones de configuración y se crea la cuenta de Automation. Puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

10. Por último, en la página Agregar solución, seleccione **Configuración**. Aparece la página Parámetros.

    ![Página Parámetros para la solución](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   En este caso, se le pedirá que:
  
   - Especifique los **nombres del grupo de recursos de destino**. Estos valores son nombres de grupos de recursos que contienen máquinas virtuales que se administrarán mediante esta solución. Puede especificar más de un nombre y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se puede usar un carácter comodín si quiere dirigirse a las máquinas virtuales de todos los grupos de recursos de la suscripción. Este valor se almacena en las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames**.
  
   - Especifique el **lista de exclusiones de máquinas virtuales (cadena)** . Este valor es el nombre de una o varias máquinas virtuales del grupo de recursos de destino. Puede especificar más de un nombre y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se admite el uso de un carácter comodín. Este valor se almacena en la variable **External_ExcludeVMNames**.
  
   - Seleccione una **programación**. Seleccione una fecha y hora para la programación. Se creará una programación recurrente diaria a partir de la hora que ha seleccionado. No se puede seleccionar otra región. Para configurar su zona horaria concreta después de configurar la solución, consulte el siguiente artículo [Modificación de la programación de inicio y apagado](automation-solution-vm-management-config.md#modify-the-startup-and-shutdown-schedules).
  
   - Para recibir **notificaciones por correo electrónico** de un grupo de acciones, acepte el valor predeterminado **Sí** y proporcione una dirección de correo electrónico válida. Si selecciona **No** pero decide en un momento posterior que desea recibir notificaciones por correo electrónico, puede actualizar el [grupo de acciones](../azure-monitor/platform/action-groups.md) que se crea con las direcciones de correo electrónico válidas separadas por un punto y coma. También deberá habilitar las reglas de alerta siguientes:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > El valor predeterminado de los **nombres del grupo de recursos de destino** es **&ast;** . Este se dirige a todas las máquinas virtuales de una suscripción. Si no desea que la solución se dirija a todas las máquinas virtuales de su suscripción, este valor debe actualizarse a una lista de nombres del grupo de recursos antes de habilitar las programaciones.

11. Después de configurar los valores iniciales necesarios para la solución, haga clic en **Aceptar** para cerrar la página Parámetros y seleccione **Crear**. 

Cuando todos los valores se hayan validado, la solución se implementa en su suscripción. Este proceso puede tardar varios segundos en finalizar y se puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

> [!NOTE]
> Si tiene una suscripción del Proveedor de soluciones en la nube de Azure (CSP de Azure), una vez completada la implementación, acceda a su cuenta de Automation, vaya a **Variables** en **Shared Resources** (Recursos compartidos) y establezca la variable [External_EnableClassicVMs](automation-solution-vm-management.md#variables) en **False**. Esta operación detiene la solución para que deje de buscar recursos de la máquina virtual clásica.

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene habilitada la solución, puede [configurarla](automation-solution-vm-management-config.md) para que admita los requisitos de administración de la máquina virtual.