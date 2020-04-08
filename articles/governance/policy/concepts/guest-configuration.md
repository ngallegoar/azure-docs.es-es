---
title: Información sobre cómo auditar el contenido de máquinas virtuales
description: Obtenga información sobre cómo Azure Policy usa Guest Configuration para auditar la configuración dentro de las máquinas virtuales.
ms.date: 11/04/2019
ms.topic: conceptual
ms.openlocfilehash: cc2ba11f75da5f993b99c90e5d0cc1030003203e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257263"
---
# <a name="understand-azure-policys-guest-configuration"></a>Información sobre Guest Configuration de Azure Policy

Además de auditar y [corregir](../how-to/remediate-resources.md) los recursos de Azure, Azure Policy puede auditar la configuración en una máquina virtual. La validación se realiza mediante el cliente y la extensión Guest Configuration. La extensión, a través del cliente, valida la configuración como:

- La configuración del sistema operativo
- Configuración de la aplicación o presencia
- Configuración del entorno

En este momento, la configuración de invitado de Azure Policy solo realiza la auditoría de la configuración dentro de la máquina. No aplica configuraciones.

## <a name="extension-and-client"></a>Extensión y cliente

Para auditar la configuración dentro de una máquina, se habilita una [extensión de máquina virtual](../../../virtual-machines/extensions/overview.md). La extensión descarga la asignación de directiva aplicable y la definición de configuración correspondiente.

### <a name="limits-set-on-the-extension"></a>Límites establecidos en la extensión

Para limitar que la extensión afecte a las aplicaciones que se ejecutan en la máquina, no se permite que la configuración de invitado supere el 5 % del uso de la CPU. Esta limitación existe tanto para definiciones integradas como personalizadas.

## <a name="register-guest-configuration-resource-provider"></a>Registrar proveedor de recursos de Guest Configuration

Para poder usar Guest Configuration debe registrar el proveedor de recursos. Puede registrarse mediante el portal o a través de PowerShell. El proveedor de recursos se registra automáticamente si la asignación de una directiva de configuración de invitado se realiza a través del portal.

### <a name="registration---portal"></a>Registro: Portal

Para registrar el proveedor de recursos para Guest Configuration a través de Azure Portal, siga estos pasos:

1. Inicie Azure Portal y haga clic en **Todos los servicios**. Busque y seleccione **Suscripciones**.

1. Busque y haga clic en la suscripción que desea habilitar para Guest Configuration.

1. En el menú izquierdo de la página **Suscripción**, haga clic en **Proveedores de recursos**.

1. Filtre o desplácese hasta que encuentre **Microsoft.GuestConfiguration**, después haga clic en **Registrar** en la misma fila.

### <a name="registration---powershell"></a>Registro: PowerShell

Para registrar el proveedor de recursos para Guest Configuration a través de PowerShell, ejecute el siguiente comando:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

## <a name="validation-tools"></a>Herramientas de validación

Dentro de la máquina, el cliente de Guest Configuration usa herramientas locales para ejecutar la auditoría.

En la tabla siguiente se muestra una lista herramienta locales usada en cada sistemas operativos compatibles:

|Sistema operativo|Herramienta de validación|Notas|
|-|-|-|
|Windows|[Desired State Configuration de Windows PowerShell](/powershell/scripting/dsc/overview/overview) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| La extensión Guest Configuration instala Ruby y Python. |

### <a name="validation-frequency"></a>Frecuencia de validación

El cliente de Guest Configuration busca contenido nuevo cada 5 minutos. Una vez que se recibe una asignación de invitado, se comprueban los valores en un intervalo de 15 minutos. Los resultados se envían al proveedor de recursos de Guest Configuration tan pronto como finaliza la auditoría. Cuando se produce una directiva del tipo [desencadenador evaluación](../how-to/get-compliance-data.md#evaluation-triggers), el estado de la máquina se escribe en el proveedor de recursos de Guest Configuration. Esta actualización hace que Azure Policy evalúe las propiedades de Azure Resource Manager. Una evaluación de Azure Policy a petición recupera el valor más reciente del proveedor de recursos de la configuración de invitado. Sin embargo, no desencadena una nueva auditoría de la configuración en la máquina.

## <a name="supported-client-types"></a>Tipos de cliente admitidos

En la tabla siguiente se muestra una lista de sistemas operativos compatibles en imágenes de Azure:

|Publicador|Nombre|Versiones|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|Credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter, 2019 Datacenter|
|Microsoft|Cliente Windows|Windows 10|
|OpenLogic|CentOS|7.3, 7.4, 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5, 7.6|
|Suse|SLES|12 SP3|

> [!IMPORTANT]
> La configuración de invitado puede auditar los nodos que ejecutan un sistema operativo compatible. Si desea auditar máquinas virtuales que utilizan una imagen personalizada, debe duplicar la definición **DeployIfNotExists** y modificar la sección **If** para incluir las propiedades de la imagen.

### <a name="unsupported-client-types"></a>Tipos de cliente no admitidos

No se admite ninguna versión de Windows Server Nano Server.

## <a name="guest-configuration-extension-network-requirements"></a>Requisitos de red de la extensión de configuración de invitado

Para comunicarse con el proveedor de recursos de la configuración de invitado en Azure, las máquinas requieren acceso de salida a los centros de datos Azure en el puerto **443**. Si utiliza una red virtual privada en Azure que no permite el tráfico saliente, las excepciones deben configurarse con las reglas del [grupo de seguridad de red](../../../virtual-network/manage-network-security-group.md#create-a-security-rule).
La [etiqueta del servicio](../../../virtual-network/service-tags-overview.md) "GuestAndHybridManagement" se puede usar para hacer referencia al servicio de Guest Configuration.

## <a name="guest-configuration-definition-requirements"></a>Requisitos de definición de Guest Configuration

Cada auditoría que ejecuta la configuración de invitado requiere dos definiciones de directiva, una definición **DeployIfNotExists** y otra **AuditIfNotExists**. La definición **DeployIfNotExists** se utiliza para preparar la máquina con el agente de la configuración de invitado y otros componentes para admitir las [herramientas de validación](#validation-tools).

La definición de directiva **DeployIfNotExists** valida y corrige los siguientes elementos:

- Valide que a la máquina se le ha asignado una configuración para evaluar. Si actualmente no hay ninguna asignación, obtiene la asignación y prepara la máquina:
  - Autenticando la máquina con una [identidad administrada](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Instalando la versión más reciente de la extensión **Microsoft.GuestConfiguration**
  - Instalando las [herramientas de validación](#validation-tools) y dependencias, si es necesario

Si la asignación **DeployIfNotExists** no es conforme, se puede usar una [tarea de corrección](../how-to/remediate-resources.md#create-a-remediation-task).

Una vez que la asignación **DeployIfNotExists** es compatible, la asignación de la directiva **AuditIfNotExists** usa las herramientas de validación local para determinar si la asignación de configuración es compatible o no compatible. La herramienta de validación proporciona los resultados al cliente de Guest Configuration. El cliente envía los resultados a la extensión de Guest, que hace que estén disponibles a través del proveedor de recursos de Guest Configuration.

Azure Policy usa la propiedad **complianceStatus** de los proveedores de recursos de Guest Configuration para notificar el cumplimiento en el nodo **Compliance**. Para más información, vea [Obtención de datos de cumplimiento](../how-to/get-compliance-data.md).

> [!NOTE]
> La directiva **DeployIfNotExists** es necesaria para que la directiva **AuditIfNotExists** devuelva resultados. Sin la directiva **DeployIfNotExists**, **AuditIfNotExists** muestra "0 de 0" recursos como estado.

Se incluyen todas las directivas integradas para Guest Configuration en una iniciativa para agrupar las definiciones para su uso en las asignaciones. La iniciativa integrada denominada _\[Versión preliminar\]: La configuración de seguridad de la contraseña de la auditoría dentro de máquinas Linux y Windows_ contiene 18 directivas. Hay seis pares **DeployIfNotExists** y **AuditIfNotExists** para Windows y tres pares para Linux. La lógica de [definición de directiva](definition-structure.md#policy-rule) valida que solo se evalúa el sistema operativo de destino.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Auditoría de la configuración del sistema operativo siguiendo las líneas de base del sector

Una de las iniciativas disponibles en Azure Policy proporciona la capacidad de auditar la configuración del sistema operativo dentro de las máquinas virtuales siguiendo una "línea de base" de Microsoft. La definición, _\[Versión preliminar\]: Auditar las VM Windows que no coinciden con la configuración de línea de base de seguridad de Azure_ incluye un conjunto completo de reglas de auditoría basadas en la configuración de la directiva de grupo de Active Directory.

La mayoría de las opciones de configuración están disponibles como parámetros. Esta funcionalidad le permite personalizar lo que se audita para alinear la directiva con sus requisitos organizativos o asignar la directiva a información de terceros, como los estándares normativos del sector.

Algunos parámetros admiten un intervalo de valores enteros. Por ejemplo, el parámetro Duración máxima de la contraseña se puede establecer mediante un operador de intervalo para ofrecer flexibilidad a los propietarios de las máquinas. Podría auditar que la configuración efectiva de la directiva de grupo que exige que el usuario cambie sus contraseñas no deba superar los 70 días ni ser inferior a un día. Como se describe en la burbuja de información del parámetro, para que esta directiva empresarial sea el valor de auditoría efectivo, establezca el valor en "1,70".

Si asigna la directiva mediante una plantilla de implementación de Azure Resource Manager, puede usar un archivo de parámetros para administrar esta configuración desde el control de código fuente. El uso de una herramienta como Git para administrar los cambios en las directivas de auditoría con comentarios en cada inserción en el repositorio documenta la evidencia de por qué la asignación debe ser una excepción al valor esperado.

#### <a name="applying-configurations-using-guest-configuration"></a>Aplicación de configuraciones mediante la configuración de invitado

La última característica de Azure Policy establece la configuración dentro de las máquinas. La definición _Configurar la zona horaria de las máquinas Windows_ realiza cambios en la máquina mediante la configuración de la zona horaria.

Al asignar definiciones que empiezan por _Configurar_, también debe asignar la definición _Implementar los requisitos previos para habilitar la directiva de configuración de invitado en máquinas virtuales Windows._ . Puede combinar estas definiciones en una iniciativa si así lo decide.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Asignación de directivas a máquinas fuera de Azure

Las directivas de auditoría disponibles para la configuración de invitado incluyen el tipo de recurso **Microsoft.HybridCompute/machines**. Todas las máquinas incorporadas a [Azure Arc para servidores](../../../azure-arc/servers/overview.md) que se encuentran en el ámbito de asignación de la directiva se incluyen automáticamente.

### <a name="multiple-assignments"></a>Asignaciones múltiples

Actualmente, las directivas de configuración de invitado solo admiten la asignación de la misma asignación de invitado una vez por cada máquina, incluso si la asignación de directiva usa parámetros diferentes.

## <a name="built-in-resource-modules"></a>Módulos de recursos integrados

Al instalar la extensión de configuración de invitado, el módulo de PowerShell "GuestConfiguration" se incluye con la versión más reciente de los módulos de recursos de DSC. Este módulo se puede descargar desde la Galería de PowerShell con el vínculo "Descarga manual" de la página del módulo [GuestConfiguration/](https://www.powershellgallery.com/packages/GuestConfiguration/). El formato de archivo ".nupkg" se puede cambiar de nombre a ".zip" para descomprimirlo y revisarlo.

## <a name="client-log-files"></a>Archivos de registro de cliente

La extensión de la configuración de invitado escribe archivos de registro en las siguientes ubicaciones:

Windows: `C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

Linux: `/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`

Donde `<version>` se refiere al número de versión actual.

### <a name="collecting-logs-remotely"></a>Recopilar registros de forma remota

El primer paso para solucionar problemas de configuraciones o módulos de Configuración de invitado debe ser usar el cmdlet `Test-GuestConfigurationPackage` siguiendo los pasos para [crear una directiva de auditoría de Configuración de invitado para Windows](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows).
Si no se realiza correctamente, la recopilación de registros de cliente puede ayudar a diagnosticar problemas.

#### <a name="windows"></a>Windows

Para usar la funcionalidad de comando de ejecución de VM de Azure para capturar información de archivos de registro en máquinas Windows, el siguiente script de PowerShell de ejemplo puede ser útil. Para obtener más información, consulte [Ejecución de scripts de PowerShell en la máquina virtual Windows con el comando Ejecutar](../../../virtual-machines/windows/run-command.md).

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Para usar la funcionalidad de comando de ejecución de VM de Azure VM para capturar información de archivos de registro en máquinas Linux, el siguiente script Bash de ejemplo puede ser útil. Para obtener más información, consulte [Ejecución de scripts de Shell en la VM Linux con el comando Ejecutar.](../../../virtual-machines/linux/run-command.md)

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

## <a name="guest-configuration-samples"></a>Ejemplos de configuración de invitado

Hay disponibles orígenes de datos para las iniciativas integradas de configuración de invitado de Policy en las siguientes ubicaciones:

- [Definiciones de directivas integradas: configuración de invitado](../samples/built-in-policies.md#guest-configuration)
- [Iniciativas integradas: configuración de invitado](../samples/built-in-initiatives.md#guest-configuration)
- [Repositorio de GitHub de ejemplos de Azure Policy](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md).
- Revise la [estructura de definición de Azure Policy](definition-structure.md).
- Vea la [Descripción de los efectos de directivas](effects.md).
- Obtenga información acerca de cómo se pueden [crear directivas mediante programación](../how-to/programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](../how-to/get-compliance-data.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](../how-to/remediate-resources.md).
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.
