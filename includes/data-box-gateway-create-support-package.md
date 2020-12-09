---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: f002fec531a0355e803a1545990fc0b13b535742
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96581235"
---
Si experimenta problemas con el dispositivo, puede crear un paquete de soporte técnico de los registros del sistema. El servicio de soporte técnico de Microsoft usa este paquete para solucionar los problemas. Siga estos pasos para crear un paquete de soporte técnico:

1. [Conéctese a la interfaz de PowerShell del dispositivo](#connect-to-the-powershell-interface).
2. Use el comando `Get-HcsNodeSupportPackage` para crear un paquete de soporte técnico. El uso del cmdlet es de esta manera:

    ```powershell
    Get-HcsNodeSupportPackage [-Path] <string> [-Zip] [-ZipFileName <string>] [-Include {None | RegistryKeys | EtwLogs
            | PeriodicEtwLogs | LogFiles | DumpLog | Platform | FullDumps | MiniDumps | ClusterManagementLog | ClusterLog |
            UpdateLogs | CbsLogs | StorageCmdlets | ClusterCmdlets | ConfigurationCmdlets | KernelDump | RollbackLogs |
            Symbols | NetworkCmdlets | NetworkCmds | Fltmc | ClusterStorageLogs | UTElement | UTFlag | SmbWmiProvider |
            TimeCmds | LocalUILogs | ClusterHealthLogs | BcdeditCommand | BitLockerCommand | DirStats | ComputeRolesLogs |
            ComputeCmdlets | DeviceGuard | Manifests | MeasuredBootLogs | Stats | PeriodicStatLogs | MigrationLogs |
            RollbackSupportPackage | ArchivedLogs | Default}] [-MinimumTimestamp <datetime>] [-MaximumTimestamp <datetime>]
            [-IncludeArchived] [-IncludePeriodicStats] [-Credential <pscredential>]  [<CommonParameters>]
    ```

    El cmdlet recopila registros de su dispositivo y copia esos registros en una red o un recurso compartido local especificados.

    Los parámetros usados son los siguientes:

    - `-Path`: especifica la red o la ruta de acceso local donde copiar el paquete de soporte técnico. (necesario)
    - `-Credential`: especifica las credenciales para acceder a la ruta de acceso protegida.
    - `-Zip`: especifica que se genere un archivo ZIP.
    - `-Include`: especifica los componentes que se incluirán en el paquete de soporte técnico. Si no se especifica, se da por hecho el valor `Default`.
    - `-IncludeArchived`: especifica que se incluyan los registros archivados en el paquete de soporte técnico.
    - `-IncludePeriodicStats`: especifica que se incluyan registros estadísticos periódicos en el paquete de soporte técnico.

    
