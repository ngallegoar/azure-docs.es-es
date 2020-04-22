---
title: Guía para controlar el comportamiento de apagado de Windows en Azure Lab Services | Microsoft Docs
description: Pasos para apagar automáticamente una máquina virtual con Windows inactiva y quitar el comando de apagado de Windows.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.topic: article
ms.date: 3/30/2020
ms.author: spelluru
ms.openlocfilehash: 7b839df5940ab26e5c1a99a1bda1fbd2545f8cc4
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113121"
---
# <a name="guide-to-controlling-windows-shutdown-behavior"></a>Guía para controlar el comportamiento de apagado de Windows

Azure Lab Services proporciona varios controles de costos para garantizar que las máquinas virtuales con Windows (VM) no se ejecutan de forma inesperada:
 - [Establecimiento de la programación](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
 - [Establecimiento de cuotas para los usuarios](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users)
 - [Habilitación del apagado automático al desconectar](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

Incluso con estos controles de costos, hay situaciones en las que una máquina virtual con Windows puede seguir ejecutándose inesperadamente. Como resultado, puede deducir lo siguiente de la cuota del alumno:

- **La ventana de RDP se quedó abierta**
  
    Cuando un alumno se conecta a la máquina virtual mediante RDP, puede dejar la ventana de RDP abierta por accidente.  Siempre que la ventana de RDP permanezca abierta, la configuración **Apagar automáticamente al desconectar** nunca surtirá efecto, ya que solo se desencadena después de desconectar la sesión de RDP.

- **El comando de apagado de Windows se usa para apagar la máquina virtual**
  
    Un alumno puede usar el comando de apagado de Windows u otros mecanismos de apagados incluidos en Windows para apagar la máquina virtual en lugar de usar el [botón de detención de Azure Lab Services](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-classroom-lab#start-or-stop-the-vm).  Cuando esto sucede, desde el punto de vista de Azure Lab Services, la máquina virtual sigue en uso.
    
Para evitar que se produzcan estas situaciones, en esta guía se explican los pasos para apagar automáticamente una máquina virtual de Windows inactiva y para quitar el comando de apagado de Windows del menú **Inicio**.  

> [!NOTE]
> Una máquina virtual también puede deducirse inesperadamente de la cuota cuando el alumno inicia la máquina virtual, pero nunca se conecta a ella mediante RDP.  Actualmente, esta guía *no* aborda este escenario.  En su lugar, se debe recordar a los alumnos que se conecten inmediatamente a la máquina virtual mediante RDP después de iniciarla. En caso contrario, deben detener la máquina virtual.

## <a name="automatic-rdp-disconnect-and-shutdown-for-idle-vm"></a>Desconexión y apagado automáticos de RDP para las máquinas virtuales inactivas

Windows proporciona la configuración de  **directiva de grupo local** que puede usar para establecer un límite de tiempo tras el cual se desconectará de forma automática la sesión de RDP cuando está inactiva.  Una sesión se define como inactiva cuando *no* registra ninguna entrada del mouse o teclado.  Las actividades de larga duración que no requieren de entradas del mouse o teclado hacen que la máquina virtual esté en un estado de inactividad.  Esto incluye la ejecución de una consulta larga, el streaming de vídeos, las compilaciones, etc.  En función de las necesidades de la clase, puede configurar el límite de tiempo de inactividad para que sea lo suficientemente largo como para considerar estos tipos de actividades.  Por ejemplo, puede establecer el límite de tiempo de inactividad en 1 hora o más si es necesario.

A continuación se describe la experiencia del alumno al configurar el **límite de sesión inactiva** junto con la opción [**apagar automáticamente al desconectarse**](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect):
 1. El alumno se conecta a la máquina virtual con Windows mediante RDP.
 2. Cuando el alumno deja abierta la ventana de RDP y la máquina virtual está inactiva durante el **límite de sesión inactiva** que haya especificado (por ejemplo, 5 minutos), el alumno verá el siguiente cuadro de diálogo:

    ![Cuadro de diálogo que indica que expiró el límite de tiempo de inactividad](../media/how-to-windows-shutdown/idle-time-expired.png)

1. Si el alumno *no* hace clic en **Aceptar**, la sesión de RDP se desconectará automáticamente después de 2 minutos.
2. Después de desconectar la sesión de RDP, cuando haya alcanzado el intervalo de tiempo especificado para la opción **Apagar automáticamente al desconectarse**, Azure Lab Services apaga la máquina virtual automáticamente.

### <a name="set-rdp-idle-session-time-limit-on-the-template-vm"></a>Establecimiento del límite de tiempo de sesión inactiva de RDP en la plantilla de la máquina virtual

Para definir el límite de tiempo de inactividad de la sesión de RDP, puede conectarse a la máquina virtual de la plantilla y ejecutar el siguiente script de PowerShell.

```powershell
# The MaxIdleTime is in milliseconds; by default, this script sets MaxIdleTime to 15 minutes.
$maxIdleTime = 15 * 60 * 1000

Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -Name "MaxIdleTime" -Value $maxIdleTime -Force
```
O bien, puede optar por seguir estos pasos manuales mediante la VM de plantilla:

1. Presione la tecla Windows, escriba **gpedit** y, a continuación, seleccione **Editar directiva de grupo (panel de control)** .

1. Vaya a **Configuración del equipo > Plantillas administrativas > Componentes de Windows > Servicios de Escritorio remoto > Host de sesión de Escritorio remoto > Límites de tiempo de sesión**.  

    ![Editor de directivas de grupo local](../media/how-to-windows-shutdown/group-policy-idle.png)
   
1. En el panel del lado derecho, haga clic con el botón secundario en **Establecer el límite de tiempo para las sesiones activas, pero en inactividad, de Servicios de Escritorio remoto** y luego haga clic en **Editar**.

1. Especifique las siguientes opciones de configuración y, a continuación, haga clic en **Aceptar**:
   1. Seleccione **Habilitado**.
   1. En **Opciones**, especifique el **Límite de sesión inactiva**.

    ![Límite de sesión inactiva](../media/how-to-windows-shutdown/edit-idle-time-limit.png)

1. Por último, para combinar este comportamiento con la configuración **Apagar automáticamente al desconectarse**, debe seguir los pasos en el artículo de procedimientos: [Habilitación del apagado automático de las máquinas virtuales al desconectarse](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect).

> [!WARNING]
> Después de configurar este valor mediante PowerShell para modificar la configuración del registro, de forma directa o manual, mediante el editor de directiva de grupo, primero tiene que reiniciar la máquina virtual para que la configuración surta efecto.  Además, si configura el valor mediante el registro, el editor de directiva de grupo no siempre se actualiza para reflejar los cambios en la configuración del registro. De todas formas, la configuración del registro sigue teniendo efecto según lo esperado y verá que la sesión de RDP está desconectada cuando está inactiva durante el período de tiempo que haya especificado.

## <a name="remove-windows-shutdown-command-from-start-menu"></a>Eliminación del comando de apagado de Windows del menú Inicio

La configuración de la **directiva de grupo local** de Windows también permite quitar el comando de apagado del menú **Inicio**.

Para quitar el comando de apagado, puede conectarse a la máquina virtual de la plantilla y ejecutar el siguiente script de PowerShell.

```powershell
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" -Name "HidePowerOptions" -Value 1 -Force
```

O bien, puede optar por seguir estos pasos manuales mediante la VM de plantilla:

1. Presione la tecla Windows, escriba **gpedit** y, a continuación, seleccione **Editar directiva de grupo (panel de control)** .

1. Vaya a **Configuración del equipo > Plantillas administrativas > Menú Inicio y barra de tareas**.  

    ![Editor de directivas de grupo local](../media/how-to-windows-shutdown/group-policy-shutdown.png)

1. Haga clic con el botón secundario en **Quitar y evitar el acceso a los comandos Apagar, Reiniciar, Suspender e Hibernar** y haga clic en **Editar**.

1. Seleccione la opción **Habilitada** y haga clic en **Aceptar**:
 
   ![Configuración de apagado](../media/how-to-windows-shutdown/edit-shutdown.png)

1. Tenga en cuenta que el comando Apagar ya no aparece en el menú **Inicio** de Windows, solo aparece el comando **Desconectar**.

    ![Comando Apagar](../media/how-to-windows-shutdown/start-menu.png)

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo sobre cómo preparar una máquina virtual de plantilla de Windows: [Guía para configurar una máquina de plantillas de Windows en Azure Lab Services](how-to-prepare-windows-template.md)