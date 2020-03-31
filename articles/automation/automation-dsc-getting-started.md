---
title: Introducción a Azure Automation State Configuration
description: Explicación y ejemplos de las tareas más comunes de Azure Automation State Configuration (DSC)
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 04/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9fce9a769dd6d88c9926913d22716666284938c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74850965"
---
# <a name="getting-started-with-azure-automation-state-configuration"></a>Introducción a Azure Automation State Configuration

En este artículo se explica cómo realizar la mayoría de las tareas más comunes con Azure Automation State Configuration, como, por ejemplo, crear, importar y compilar configuraciones, incorporar máquinas para administrarlas y ver informes. Para más información sobre lo que es Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-overview.md). Para obtener documentación de Desired State Configuration, consulte [Información general sobre la configuración de estado deseado de Windows PowerShell](/powershell/scripting/dsc/overview/overview).

En este artículo se proporciona una guía paso a paso para usar Azure Automation State Configuration. Si busca un entorno de ejemplo que ya se haya configurado sin seguir los pasos descritos en este artículo, puede usar la siguiente plantilla de Resource Manager: [Azure Automation Managed Node](https://github.com/Azure/azure-quickstart-templates/tree/master/101-automation-configuration). Esta plantilla configura un entorno completado de Azure Automation State Configuration, incluida una máquina virtual de Azure administrada por Azure Automation State Configuration.

## <a name="prerequisites"></a>Prerrequisitos

Para completar los ejemplos de este artículo, se requiere lo siguiente:

- Una cuenta de Azure Automation Para obtener instrucciones sobre cómo crear una cuenta de ejecución de Azure Automation, consulte el artículo sobre las [cuentas de ejecución de Azure](automation-sec-configure-azure-runas-account.md).
- Una máquina virtual de Azure Resource Manager (no clásico) ejecuta un [sistema operativo compatible](automation-dsc-overview.md#operating-system-requirements). Para obtener instrucciones sobre la creación de una máquina virtual, consulte [Creación de la primera máquina virtual de Windows en el Portal de Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="creating-a-dsc-configuration"></a>Creación de una configuración de DSC

Crearemos una [configuración DSC](/powershell/scripting/dsc/configurations/configurations) simple que garantice la presencia o ausencia de la característica de Windows **Web-Server** (IIS), en función de cómo se asignen los nodos.

1. Inicie [VSCode](https://code.visualstudio.com/docs) (o cualquier editor de texto).
1. Escriba el siguiente texto:

    ```powershell
    configuration TestConfig
    {
        Node IsWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
            }
        }

        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
            }
        }
    }
    ```
1. Guarde el archivo como `TestConfig.ps1`.

Esta configuración llama a un recurso de cada bloque del nodo, el recurso [WindowsFeature](/powershell/scripting/dsc/reference/resources/windows/windowsfeatureresource), que garantiza la presencia o ausencia de la característica **Web-Server** .

## <a name="importing-a-configuration-into-azure-automation"></a>Importación de una configuración en Azure Automation

A continuación, debe importar la configuración en la cuenta de Automation.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la página **Cuenta de Automation**, seleccione **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State Configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Configuraciones** y, después, haga clic en **+ Agregar**.
1. En la página **Importar configuración**, examine el archivo `TestConfig.ps1` en el equipo.

   ![Captura de pantalla de la hoja **Importar configuración**](./media/automation-dsc-getting-started/AddConfig.png)

1. Haga clic en **OK**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Visualización de una configuración en Azure Automation

Después de importar una configuración, puede verla en el Portal de Azure.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la página **Cuenta de Automation**, seleccione **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Configurations** (Configuraciones) y, luego, en **TestConfig** (este es el nombre de la configuración que importó en el procedimiento anterior).
1. En la página **Configuración de TestConfig**, haga clic en **Ver el origen de configuración**.

   ![Captura de pantalla de la hoja TestConfig configuration (Configuración de TestConfig)](./media/automation-dsc-getting-started/ViewConfigSource.png)

   Se abre una página **TestConfig Configuration source** (Origen de configuración de TestConfig), que muestra el código de PowerShell de la configuración.

## <a name="compiling-a-configuration-in-azure-automation"></a>Compilación de una configuración en Azure Automation

Para poder aplicar un estado deseado a un nodo, se debe compilar una configuración de DSC que defina dicho estado en una o varias configuraciones de nodo (documento de MOF) y se debe colocar en el servidor de extracción de DSC de Automatización. Para una descripción más detallada de la compilación de configuraciones en Azure Automation State Configuration, consulte [Compilación de configuraciones en DSC de Azure Automation](automation-dsc-compile.md).
Para más información sobre la compilación de configuraciones, consulte [Configuraciones DSC](/powershell/scripting/dsc/configurations/configurations).

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la página **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Configurations** (Configuraciones) y, luego, en **TestConfig** (el nombre de la configuración anteriormente importada).
1. En la página **Configuración de TestConfig**, haga clic en **Compilar** y luego en **Sí**. Así se inicia un trabajo de compilación.

   ![Captura de pantalla de la página de configuración de TestConfig en la que se resalta el botón Compilar](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Cuando se compila una configuración en Azure Automation, esta implementa todos los archivos MOF de configuración de nodo creados en el servidor de extracción.

## <a name="viewing-a-compilation-job"></a>Visualización de un trabajo de compilación

Después de iniciar una compilación, puede verla en el icono **Compilation Jobs** (Trabajos de compilación) de la página **Configuración**. El icono **Compilation Jobs** (Trabajos de compilación) muestra los trabajos con errores, completados y en ejecución. Cuando abre la página de un trabajo de compilación, muestra información acerca del trabajo, incluidos los errores o advertencias encontrados, los parámetros de entrada usados en la configuración y los registros de compilación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la página **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Configurations** (Configuraciones) y, luego, en **TestConfig** (el nombre de la configuración anteriormente importada).
1. En **Trabajos de compilación**, seleccione el trabajo de compilación que desea ver. Se abre la página **Compilation Job** (Trabajo de compilación), etiquetada con la fecha en que se inició el trabajo de compilación.

   ![Captura de pantalla de la página Trabajo de compilación](./media/automation-dsc-getting-started/CompilationJob.png)

1. Haga clic en cualquiera de los iconos de la página **Trabajo de compilación** para ver detalles adicionales acerca del trabajo.

## <a name="viewing-node-configurations"></a>Visualización de configuraciones de nodo

La finalización correcta de un trabajo de compilación crea una o varias configuraciones de nodo nuevas. Una configuración de nodo es un documento MOF que se implementa en el servidor de extracción y está listo ser extraído y que lo apliquen uno o varios nodos. Puede ver las configuraciones de nodos de su cuenta de Automation en la página **State configuration (DSC)** (Configuración de estado [DSC]). Los nombres de las configuraciones de nodo tienen el formato *ConfigurationName*.*NodeName*.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la hoja **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado (DSC)), haga clic en la pestaña **Compiled configurations** (Configuraciones compiladas).

   ![Captura de pantalla de la pestaña de configuraciones compiladas](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-state-configuration"></a>Incorporación de una máquina virtual de Azure para administrarla con Azure Automation State Configuration

Puede usar Azure Automation State Configuration para administrar máquinas virtuales de Azure (tanto clásicas como de Resource Manager), máquinas virtuales locales, máquinas Linux, máquinas virtuales de AWS y máquinas físicas locales. En este artículo, se explicará cómo incorporar solo máquinas virtuales de Azure Resource Manager. Para más información sobre la incorporación de otros tipos de máquinas, consulte [Incorporación de máquinas para administrarlas con DSC de Azure Automation](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-state-configuration"></a>Para incorporar una máquina virtual de Azure Resource Manager para su administración por Azure Automation State Configuration, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la hoja **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), mientras está en la pestaña **Nodos**, haga clic en **+ Agregar**.

   ![Captura de pantalla de la página Nodos DSC en la que se resalta el botón Agregar máquina virtual de Azure](./media/automation-dsc-getting-started/OnboardVM.png)

1. En la página **Máquinas virtuales**, seleccione la máquina virtual.
1. En la página de detalles **Máquina virtual**, haga clic en **+ Conectar**.

   > [!IMPORTANT]
   > Debe ser una máquina virtual de Azure Resource Manager que ejecute un [sistema operativo compatible](automation-dsc-overview.md#operating-system-requirements).

2. En la página **Registro**, seleccione el nombre de la configuración del nodo que quiere aplicar a la máquina virtual en el cuadro **Nombre de la configuración del nodo**. Especificar un nombre en este momento es opcional. Puede cambiar la configuración de nodo asignada después de la incorporación del nodo.
   Marque **Reboot Node if Needed** (Reiniciar el nodo si es necesario) y haga clic en **Aceptar**.

   ![Captura de pantalla de la hoja Registro](./media/automation-dsc-getting-started/RegisterVM.png)

   La configuración de nodo que especificó se aplica a la máquina virtual según los intervalos que especifica el valor de **Frecuencia del modo de configuración** y la máquina virtual busca actualizaciones en la configuración del nodo según los intervalos que especifica el valor de **Frecuencia de actualización**. Para más información sobre cómo se utilizan estos valores, consulte [Configuración del administrador de configuración local](/powershell/scripting/dsc/managing-nodes/metaConfig).

Azure inicia el proceso de incorporación de la máquina virtual. Cuando finalice, la máquina virtual se muestra en la pestaña **Nodos** de la página **State configuration (DSC)** (Configuración de estado [DSC]) de la cuenta de Automation.

## <a name="viewing-the-list-of-managed-nodes"></a>Visualización de la lista de nodos administrados

Puede ver la lista de todas las máquinas que se han incorporado para administración en la cuenta de Automation, en la pestaña **Nodos**  de la página **State configuration (DSC)** (Configuración de estado [DSC]).

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la hoja **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Nodos**.

## <a name="viewing-reports-for-managed-nodes"></a>Visualización de informes de nodos administrados

Cada vez que Azure Automation State Configuration realiza una comprobación de coherencia en un nodo administrado, el nodo envía un informe de estado al servidor de extracción. Estos informes se pueden ver en la página de dicho nodo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la hoja **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Nodos**. Aquí, puede ver la información general del estado de configuración y los detalles de cada nodo.

   ![Captura de pantalla de la página de nodos](./media/automation-dsc-getting-started/NodesTab.png)

1. En la pestaña **Nodos**, haga clic en el registro del nodo para abrir los informes. Haga clic en el informe del que quiere ver detalles adicionales.

   ![Captura de pantalla de la hoja Informe](./media/automation-dsc-getting-started/NodeReport.png)

En la hoja de un informe individual puede ver la siguiente información de estado de la comprobación de coherencia correspondiente:

- El estado del informe, si el nodo es "Compatible", la configuración "Error" o el nodo es "No compatible" (cuando el nodo está en modo **ApplyandMonitor** y la máquina no está en el estado deseado).
- La hora de inicio de la comprobación de coherencia.
- El tiempo de ejecución total de la comprobación de coherencia.
- El tipo de comprobación de coherencia.
- Todos los errores, incluidos el código de error y el mensaje de error.
- Los recursos de DSC utilizados en la configuración y el estado de cada recurso (si el nodo está en el estado deseado para dicho recurso): puede hacer clic en cada recurso para más información.
- El nombre, la dirección IP y el modo de configuración del nodo.

También puede hacer clic en **Ver informe sin formato** para ver los datos reales que el nodo envía al servidor.
Para más información sobre el uso de esos datos, consulte [Uso de un servidor de informes de DSC](/powershell/scripting/dsc/pull-server/reportserver).

Después de que se incorpora un nodo puede pasar un tiempo antes de que el primer informe esté disponible. Después de incorporar un nodo es posible que tenga que esperar hasta 30 minutos para que aparezca el primer informe.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Reasignación de un nodo a una configuración de nodo diferente

Puede asignar un nodo para que use una configuración de nodo diferente a la que le asignó inicialmente.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la hoja **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Nodos**.
1. En la página **Nodos**, haga clic en el nombre del nodo que quiere reasignar.
1. En la página de dicho nodo, haga clic en **Asignar configuración de nodo**.

    ![Captura de pantalla de la página de detalles del nodo donde se resalta el botón Asignar configuración de nodo](./media/automation-dsc-getting-started/AssignNode.png)

1. En la página **Asignar configuración de nodo**, seleccione la configuración de nodo a la que desea asignar el nodo y luego haga clic en **Aceptar**.

    ![Captura de pantalla de la página Asignar configuración de nodo](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Anulación del registro de un nodo

Si ya no desea que DSC de Azure Automation administre un nodo, puede anular su registro.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la hoja **Cuenta de Automation**, haga clic en **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** (Configuración de estado [DSC]), haga clic en la pestaña **Nodos**.
1. En la pestaña **Nodos**, haga clic en el nombre del nodo cuyo registro desea anular.
1. En la página de dicho nodo, haga clic en **Anular registro**.

    ![Captura de pantalla de la página de detalles del nodo donde se resalta el botón Anular registro](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Artículos relacionados

- [Introducción a Azure Automation State Configuration](automation-dsc-overview.md)
- [Incorporación de máquinas para su administración mediante Azure Automation State Configuration](automation-dsc-onboarding.md)
- [Información general sobre la configuración de estado deseado de Windows PowerShell](/powershell/scripting/dsc/overview/overview)
- [Cmdlets de Azure Automation State Configuration](/powershell/module/azurerm.automation/#automation)
- [Precio de Azure Automation State Configuration](https://azure.microsoft.com/pricing/details/automation/)
