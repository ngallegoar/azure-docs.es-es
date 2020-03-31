---
title: Actualización del agente Dependency Agent en Azure Monitor para VM
description: En este artículo se describe cómo actualizar el agente Dependency Agent de Azure Monitor para VM mediante la línea de comandos, el Asistente para la instalación y otros métodos.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: c55bee9880c4134f2e304a7fc5176225477fe5f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480766"
---
# <a name="how-to-upgrade-the-azure-monitor-for-vms-dependency-agent"></a>Actualización del agente Dependency Agent en Azure Monitor para VM

Después de la implementación inicial del agente Dependency Agent en Azure Monitor para VM, se publican actualizaciones que incluyen correcciones de errores o compatibilidad con nuevas características o funcionalidad.  Este artículo le ayuda a comprender los métodos disponibles y a realizar la actualización manualmente o a través de la automatización.

## <a name="upgrade-options"></a>Opciones de actualización 

El agente Dependency Agent para Windows y Linux puede actualizarse a la versión más reciente de forma manual o automática según el escenario de implementación y el entorno en que se está ejecutando la máquina. Los métodos siguientes pueden usarse para actualizar el agente.

|Entorno |Método de instalación |Método de actualización |
|------------|--------------------|---------------|
|Azure VM | Extensión de VM del agente de Dependency Agent para [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) y [Linux](../../virtual-machines/extensions/agent-dependency-linux.md). | El agente se actualiza automáticamente de forma predeterminada a menos que configure la plantilla de Azure Resource Manager para no realizar la actualización; para ello, debe establecer la propiedad *autoUpgradeMinorVersion* a **false**. La actualización de una versión secundaria en la que la actualización automática está deshabilitada y la actualización de una versión principal siguen el mismo método: desinstalar y volver a instalar la extensión. |
| Imágenes personalizadas de VM de Azure | Instalación manual del agente Dependency Agent para Windows o Linux | La actualización de las VM a la versión más reciente del agente debe realizarse desde la línea de comandos que ejecuta el paquete del instalador de Windows o el paquete de scripts de shell instalable y autoextraíble de Linux.|
| VM ajenas a Azure | Instalación manual del agente Dependency Agent para Windows o Linux | La actualización de las VM a la versión más reciente del agente debe realizarse desde la línea de comandos que ejecuta el paquete del instalador de Windows o el paquete de scripts de shell instalable y autoextraíble de Linux. |

## <a name="upgrade-windows-agent"></a>Actualización del agente de Windows 

Para actualizar el agente en una VM de Windows a la última versión que no se instaló con la extensión de VM de Dependency Agent, puede realizar el proceso desde el símbolo del sistema, el script u otra solución de automatización, o mediante el Asistente para la instalación InstallDependencyAgent-Windows.exe.  

Puede descargar la versión más reciente del agente de Windows [aquí](https://aka.ms/dependencyagentwindows).

### <a name="using-the-setup-wizard"></a>Uso del Asistente para la instalación

1. Inicie sesión en el equipo con una cuenta con derechos administrativos.

2. Ejecute **InstallDependencyAgent-Windows.exe** para iniciar el Asistente para la instalación.
   
3. Siga las instrucciones del **Asistente para la instalación de Dependency Agent** para desinstalar la versión anterior de Dependency Agent y, a continuación, instale la versión más reciente.


### <a name="from-the-command-line"></a>Desde la línea de comandos

1. Inicie sesión en el equipo con una cuenta con derechos administrativos.

2. Ejecute el siguiente comando:

    ```dos
    InstallDependencyAgent-Windows.exe /S /RebootMode=manual
    ```

    El parámetro `/RebootMode=manual` impide que la actualización reinicie automáticamente el equipo si algunos procesos usan archivos de la versión anterior y tienen un bloqueo. 

3. Para confirmar que la actualización se realizó correctamente, consulte `install.log` para obtener información detallada sobre la instalación. El directorio de registro es *%Programfiles%\Microsoft Dependency Agent\logs*.

## <a name="upgrade-linux-agent"></a>Actualización del agente de Linux 

La actualización desde versiones anteriores del agente Dependency Agent en Linux se admite y se realiza siguiendo el mismo comando que una nueva instalación.

Puede descargar la versión más reciente del agente de Windows [aquí](https://aka.ms/dependencyagentlinux).

1. Inicie sesión en el equipo con una cuenta con derechos administrativos.

2. Ejecute el siguiente comando como raíz`sh InstallDependencyAgent-Linux64.bin -s`. 

Si Dependency Agent no se inicia, compruebe los registros para obtener información detallada del error. En los agentes de Linux, el directorio de registro es */var/opt/microsoft/dependency-agent/log*. 

## <a name="next-steps"></a>Pasos siguientes

Si desea detener la supervisión de las máquinas virtuales durante un período de tiempo o quitar Azure Monitor para VM por completo, consulte [Deshabilitación de la supervisión de las máquinas virtuales en Azure Monitor para VM](vminsights-optout.md).
