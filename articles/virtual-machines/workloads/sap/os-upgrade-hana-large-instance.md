---
title: Actualización del sistema operativo para SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Realizar la actualización del sistema operativo para SAP HANA en Azure (instancias grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/04/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8485f3474da18e052bc0eab6c053be084ef884a2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192423"
---
# <a name="operating-system-upgrade"></a>Actualización del sistema operativo
Este documento describe los detalles acerca de las actualizaciones del sistema operativo en las instancias grandes HANA.

>[!NOTE]
>La actualización del sistema operativo es responsabilidad de los clientes. El servicio de soporte técnico de operaciones de Microsoft puede guiarle a las áreas clave que debe vigilar durante este proceso. Debe consultar con el proveedor del sistema operativo antes de planear una actualización.

Durante el aprovisionamiento de la unidad HLI, el equipo de operaciones de Microsoft instala el sistema operativo.
Con el tiempo, se le pide realizar mantenimiento al sistema operativo (por ejemplo: aplicación de revisiones, optimizaciones, actualizaciones, etc.) en la unidad HLI.

Además, necesita abrir una incidencia de soporte técnico para ponerse en contacto con el equipo de operaciones de Microsoft y consultar con ellos antes de realizar cambios importantes en el sistema operativo (por ejemplo, la actualización de SP1 a SP2).

El vale incluye:

* El id. de suscripción de HLI.
* El nombre del servidor.
* El nivel de revisión que se va a aplicar.
* La fecha en la que está planeando este cambio. 

Recomendamos que abra esta incidencia al menos una semana antes de la fecha de actualización deseada para que el equipo de Operaciones pueda comprobar si es necesaria una actualización de firmware en la hoja del servidor.


Para la matriz de compatibilidad de las distintas versiones de SAP HANA con las diferentes versiones de Linux, consulte la [Nota de SAP 2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Problemas conocidos

A continuación, se muestran algunos problemas habituales durante la actualización:
- En la clase SKU de tipo II, Software Foundation Server (SFS) se quita después de actualizar el SO. Necesita reinstalar la versión de SFS compatible después de actualizar el SO.
- Reversión de los controladores de la tarjeta Ethernet (ENIC y FNIC) a la versión anterior. Después de la actualización, necesita volver a instalar la versión compatible de los controladores.

## <a name="sap-hana-large-instance-type-i-recommended-configuration"></a>Configuración recomendada de SAP HANA (instancias grandes) (tipo I)

Con el tiempo, la configuración del sistema operativo puede desviarse de la configuración recomendada debido a la aplicación de revisiones, las actualizaciones del sistema y los cambios realizados por los clientes. Además, Microsoft identifica las actualizaciones necesarias para los sistemas existentes con el fin de garantizar que están configuradas de forma óptima para conseguir mejor rendimiento y resistencia. En las instrucciones siguientes se describen las recomendaciones dirigidas al rendimiento de la red, la estabilidad del sistema y el rendimiento óptimo de HANA.

### <a name="compatible-enicfnic-driver-versions"></a>Versiones compatibles del controlador eNIC/fNIC
  Para conseguir el rendimiento de red y la estabilidad del sistema adecuados, se recomienda asegurarse de tener instalada la versión correcta específica del sistema operativo de los controladores eNIC y fNIC, tal y como se muestra en la siguiente tabla de compatibilidad. Los servidores se entregan a los clientes con versiones compatibles. Tenga en cuenta que, en algunos casos, durante la aplicación de revisiones del sistema operativo o del kernel, los controladores pueden revertir a sus versiones predeterminadas. Asegúrese de que la versión del controlador correcta ejecuta operaciones posteriores a la aplicación de revisiones del sistema operativo o del kernel.
       
      
  |  Fabricante del sistema operativo    |  Versión del paquete del sistema operativo     |  Versión de firmware  |  Controlador eNIC |  Controlador fNIC | 
  |---------------|-------------------------|--------------------|--------------|--------------|
  |   SuSE        |  SLES 12 SP2            |   3.1.3h           |  2.3.0.40    |   1.6.0.34   |
  |   SuSE        |  SLES 12 SP3            |   3.1.3h           |  2.3.0.44    |   1.6.0.36   |
  |   SuSE        |  SLES 12 SP4            |   3.2.3i           |  2.3.0.47    |   2.0.0.54   |
  |   SuSE        |  SLES 12 SP2            |   3.2.3i           |  2.3.0.45    |   1.6.0.37   |
  |   SuSE        |  SLES 12 SP3            |   3.2.3i           |  2.3.0.45    |   1.6.0.37   |
  |   Red Hat     |  RHEL 7.2               |   3.1.3h           |  2.3.0.39    |   1.6.0.34   |
 

### <a name="commands-for-driver-upgrade-and-to-clean-old-rpm-packages"></a>Comandos para actualizar controladores y limpiar paquetes RPM antiguos

#### <a name="command-to-check-existing-installed-drivers"></a>Comando para comprobar los controladores instalados existentes
```
rpm -qa | grep enic/fnic 
```
#### <a name="delete-existing-enicfnic-rpm"></a>Eliminación del RPM de eNIC/fNIC existente
```
rpm -e <old-rpm-package>
```
#### <a name="install-the-recommended-enicfnic-driver-packages"></a>Instalación de los paquetes de controladores de eNIC/fNIC recomendados
```
rpm -ivh <enic/fnic.rpm> 
```

#### <a name="commands-to-confirm-the-installation"></a>Comandos para confirmar la instalación
```
modinfo enic
modinfo fnic
```

### <a name="suse-hlis-grub-update-failure"></a>Error de actualización de GRUB de unidades HLI de SuSE
HANA (instancias grandes) de SAP en Azure (tipo 1) puede encontrase en un estado de no arranque tras la actualización. Con el procedimiento siguiente se soluciona este problema.
#### <a name="execution-steps"></a>Pasos de ejecución


*   Ejecute el comando `multipath -ll`.
*   Obtenga el identificador de LUN cuyo tamaño sea aproximadamente 50G o use el comando: `fdisk -l | grep mapper`
*   Actualice el archivo `/etc/default/grub_installdevice` con la línea `/dev/mapper/<LUN ID>`. Ejemplo: /dev/mapper/3600a09803830372f483f495242534a56
>[!NOTE]
>El identificador de LUN varía de un servidor a otro.


### <a name="disable-edac"></a>Deshabilitación de EDAC 
   El módulo de detección y corrección de errores (EDAC) ayuda a detectar y corregir errores de memoria. Sin embargo, el hardware subyacente de SAP HANA en Azure (instancias grandes) (tipo I) ya realiza la misma función. Tener la misma característica habilitada en los niveles de hardware y sistema operativo (SO) puede producir conflictos y provocar apagados ocasionales no planeados del servidor. Por lo tanto, se recomienda deshabilitar el módulo del sistema operativo.

#### <a name="execution-steps"></a>Pasos de ejecución

* Compruebe si el módulo EDAC está habilitado. Si se devuelve una salida en el comando siguiente, significa que el módulo está habilitado. 
```
lsmod | grep -i edac 
```
* Para deshabilitar los módulos, anexe las líneas siguientes al archivo. `/etc/modprobe.d/blacklist.conf`.
```
blacklist sb_edac
blacklist edac_core
```
Para que los cambios tengan lugar, se requiere un reinicio. Ejecute el comando `lsmod` y compruebe que el módulo no existe en la salida.


### <a name="kernel-parameters"></a>Parámetros de kernel
   Asegúrese de que se aplica el valor correcto para `transparent_hugepage`, `numa_balancing`, `processor.max_cstate`, `ignore_ce` y `intel_idle.max_cstate`.

* intel_idle.max_cstate=1
* processor.max_cstate=1
* transparent_hugepage=never
* numa_balancing=disable
* mce=ignore_ce


#### <a name="execution-steps"></a>Pasos de ejecución

* Agregue estos parámetros a la línea `GRB_CMDLINE_LINUX` del archivo. `/etc/default/grub`.
```
intel_idle.max_cstate=1 processor.max_cstate=1 transparent_hugepage=never numa_balancing=disable mce=ignore_ce
```
* Cree un archivo GRUB.
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```
* Reinicie el sistema.


## <a name="next-steps"></a>Pasos siguientes
- Consulte la sección de [copia de seguridad y restauración](hana-overview-high-availability-disaster-recovery.md) para obtener información sobre la copia de seguridad del SO de clase SKU de tipo I.
- Consulte [Copia de seguridad y restauración del sistema operativo para SKU de tipo II](os-backup-type-ii-skus.md) para la clase SKU de tipo II.
