---
title: Solución de problemas de errores de copia de seguridad de bases de datos de SAP HANA
description: En este artículo se describe cómo se solucionan los errores comunes que pueden producirse al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA.
ms.topic: troubleshooting
ms.date: 11/7/2019
ms.openlocfilehash: 6520f106011b632da2725f456aeb278c7748ddc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79459317"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure

En este artículo se proporciona información para la solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure Virtual Machines. Para más información sobre los escenarios de copia de seguridad de SAP HANA actualmente compatibles, consulte [Compatibilidad con los escenarios](sap-hana-backup-support-matrix.md#scenario-support).

## <a name="prerequisites-and-permissions"></a>Requisitos previos y permisos

Consulte las secciones sobre los [requisitos previos](tutorial-backup-sap-hana-db.md#prerequisites) y [Qué hace el script de registro previo](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) antes de configurar las copias de seguridad.

## <a name="common-user-errors"></a>Errores de usuario comunes

### <a name="usererrorhanainternalrolenotpresent"></a>UserErrorHANAInternalRoleNotPresent

| **Mensaje de error**      | <span style="font-weight:normal">La copia de seguridad de Azure no tiene los privilegios de rol necesarios para llevar a cabo la copia de seguridad</span>    |
| ---------------------- | ------------------------------------------------------------ |
| **Causas posibles:**    | Es posible que se haya sobrescrito el rol.                          |
| **Acción recomendada** | Para resolver el problema, ejecute el script desde el panel **Discover DB** (Detectar BD) o descárguelo [aquí](https://aka.ms/scriptforpermsonhana). También puede agregar el rol "SAP_INTERNAL_HANA_SUPPORT " al usuario de copia de seguridad de la carga de trabajo (AZUREWLBACKUPHANAUSER). |

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

| Mensaje de error      | <span style="font-weight:normal">No se pudo conectar con el sistema HANA</span>                        |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | La instancia de SAP HANA puede estar fuera de servicio.<br/>No se establecen los permisos necesarios para que la copia de seguridad de Azure interactúe con la base de datos de HANA. |
| **Acción recomendada** | Compruebe si la base de datos de SAP HANA está activa. Si la base de datos está en funcionamiento, compruebe si se establecieron todos los permisos necesarios. Si falta alguno de los permisos, ejecute el [script de registro previo](https://aka.ms/scriptforpermsonhana) para agregar los permisos que faltan. |

### <a name="usererrorhanainstancenameinvalid"></a>UserErrorHanaInstanceNameInvalid

| Mensaje de error      | <span style="font-weight:normal">La instancia de SAP HANA especificada no es válida ni se puede encontrar</span>  |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | No se puede realizar una copia de seguridad de varias instancias de SAP HANA en una máquina virtual de Azure única. |
| **Acción recomendada** | Ejecute el [script de registro previo](https://aka.ms/scriptforpermsonhana) en la instancia de SAP HANA de la que quiere hacer una copia de seguridad. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft. |

### <a name="usererrorhanaunsupportedoperation"></a>UserErrorHanaUnsupportedOperation

| Mensaje de error      | <span style="font-weight:normal">No se admite la operación de SAP HANA especificada</span>              |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | La copia de seguridad de Azure para SAP HANA no es compatible con la copia de seguridad incremental ni con las acciones realizadas en clientes nativos de SAP HANA (Studio/Cockpit/DBA Cockpit). |
| **Acción recomendada** | Para más información, consulte [aquí](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support). |

### <a name="usererrorhanapodoesnotsupportbackuptype"></a>UserErrorHANAPODoesNotSupportBackupType

| Mensaje de error      | <span style="font-weight:normal">Esta base de datos de SAP HANA no admite el tipo de copia de seguridad solicitada</span>  |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | La copia de seguridad de Azure no es compatible con la copia de seguridad incremental ni con la copia de seguridad mediante instantáneas. |
| **Acción recomendada** | Para más información, consulte [aquí](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support). |

### <a name="usererrorhanalsnvalidationfailure"></a>UserErrorHANALSNValidationFailure

| Mensaje de error      | <span style="font-weight:normal">La cadena de registros de copia de seguridad se interrumpió</span>                                    |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | Es posible que el destino de la copia de seguridad de registros se haya actualizado desde BackInt al sistema de archivos o puede que el ejecutable de BackInt se haya modificado. |
| **Acción recomendada** | Desencadene una copia de seguridad completa para resolver el problema.                   |

### <a name="usererrorincomaptiblesrctargetsystsemsforrestore"></a>UserErrorIncomaptibleSrcTargetSystsemsForRestore

| Mensaje de error      | <span style="font-weight:normal">Los sistemas de origen y destino para la restauración no son compatibles</span>    |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | El sistema de destino para la restauración no es compatible con el origen. |
| **Acción recomendada** | Consulte la nota SAP [1642148](https://launchpad.support.sap.com/#/notes/1642148) para información sobre los tipos de restauración compatibles actualmente. |

### <a name="usererrorsdctomdcupgradedetected"></a>UserErrorSDCtoMDCUpgradeDetected

| Mensaje de error      | <span style="font-weight:normal">Actualización de SDC a MDC detectada</span>                                   |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | La instancia de SAP HANA se actualizó de SDC a MDC. Las copias de seguridad presentarán un error después de la actualización. |
| **Acción recomendada** | Siga los pasos que aparecen en la sección [Actualización de SAP HANA 1.0 a 2.0](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) para resolver el problema. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Mensaje de error      | <span style="font-weight:normal">Se detectó una configuración de tipo Backint no válida</span>                       |
| ------------------ | ------------------------------------------------------------ |
| **Causas posibles:**    | Los parámetros de BackInt están incorrectamente especificados para Azure Backup. |
| **Acción recomendada** | Compruebe si se establecieron estos parámetros (BackInt):<br/>\* [catalog_backup_using_backint:true]<br/>\* [enable_accumulated_catalog_backup:false]<br/>\* [parallel_data_backup_backint_channels:1]<br/>\* [log_backup_timeout_s:900)]<br/>\* [backint_response_timeout:7200]<br/>Si los parámetros basados en BackInt están presentes en el host, quítelos. Si los parámetros no están presentes en el nivel del host, pero se han modificado manualmente en el nivel de la base de datos, reviértalos a los valores apropiados como se describió anteriormente. O bien, ejecute [Detener la protección y conservar los datos de copia de seguridad](https://docs.microsoft.com/azure/backup/sap-hana-db-manage#stop-protection-for-an-sap-hana-database) en Azure Portal y, después, seleccione **Reanudar copia de seguridad**. |

## <a name="restore-checks"></a>Comprobaciones de restauración

### <a name="single-container-database-sdc-restore"></a>Restauración de una base de datos de contenedor único (SDC)

Ocúpese de las entradas mientras se realiza la restauración de una base de datos de contenedor único (SDC) para HANA a otra máquina de SDC. El nombre de la base de datos debe estar en minúscula con "sdc" anexado entre corchetes. La instancia de HANA se mostrará en mayúsculas.

Supongamos que hay una copia de seguridad de una instancia "H21" de HANA de SDC. En la página de elementos de copia de seguridad aparecerá el nombre del elemento de copia de seguridad como **"h21(sdc)"** . Si intenta restaurar esta base de datos en otra SDC de destino, por ejemplo, H11, es necesario proporcionar las entradas siguientes.

![Nombre de base de datos SDC restaurada](media/backup-azure-sap-hana-database/hana-sdc-restore.png)

Tenga en cuenta los siguientes puntos:

- De manera predeterminada, el nombre de la base de datos restaurada se rellenará con el nombre del elemento de copia de seguridad. En este caso, h21(sdc).
- Si selecciona el destino como H11, el nombre de la base de datos restaurada NO CAMBIARÁ de manera automática. **Se debe editar para cambiarlo a h11(sdc)** . Con respecto a SDC, el nombre de la base de datos restaurada será el identificador de la instancia de destino en minúsculas más "sdc" entre corchetes.
- Como SDC solo puede tener una base de datos única, también debe hacer clic en la casilla para permitir reemplazar los datos existentes de la base de datos por los datos del punto de recuperación.
- Linux distingue mayúsculas de minúsculas. De este modo, tiene que tener cuidado y respetar su uso.

### <a name="multiple-container-database-mdc-restore"></a>Restauración de base de datos de varios contenedores (MDC)

En las bases de datos de varios contenedores para HANA, la configuración estándar es SYSTEMDB + 1 o más bases de datos de inquilino. Restaurar toda una instancia de SAP HANA significa restaurar tanto SYSTEMDB como las bases de datos de inquilino. Primero se restaura SYSTEMDB y, luego, la base de datos de inquilino. Básicamente, restaurar la base de datos del sistema significa reemplazar la información del sistema del destino seleccionado. Esta restauración también reemplaza la información relacionada con BackInt en la instancia de destino. Así pues, una vez que la base de datos del sistema se restaura en una instancia de destino, vuelva a ejecutar el script de registro previo. Solo entonces las restauraciones de bases de datos de inquilino subsiguientes se realizarán correctamente.

## <a name="upgrading-from-sap-hana-10-to-20"></a>Actualización de SAP HANA 1.0 a 2.0

Si va a proteger bases de datos SAP HANA 1.0 y quiere actualizarlas a la versión 2.0, siga los pasos que se describen a continuación:

- [Detenga la protección](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) con la conservación de datos para la base de datos SDC antigua.
- Realice la actualización. Después de la finalización, HANA es ahora MDC con una base de datos de sistema y una base de datos de inquilino.
- Vuelva a ejecutar el [script de registro previo](https://aka.ms/scriptforpermsonhana) con los detalles correctos de (sid y mdc).
- Vuelva a registrar la extensión para la misma máquina en Azure Portal (Copia de seguridad -> Ver detalles -> seleccione la máquina virtual de Azure pertinente -> Volver a registrar).
- Haga clic en Rediscover DBs (Volver a detectar bases de datos) para la misma máquina virtual. Esta acción debería mostrar las nuevas bases de datos en el paso 2 con los detalles correctos (SYSTEMDB y base de datos de inquilino, no SDC).
- Configure la copia de seguridad de estas nuevas bases de datos.

## <a name="upgrading-without-an-sid-change"></a>Actualización sin un cambio de identificador de seguridad

Las actualizaciones del sistema operativo o SAP HANA que no causan un cambio de SID se pueden controlar tal como se describe a continuación:

- [Detenga la protección](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) con la conservación de datos para la base de datos.
- Realice la actualización.
- Vuelva a ejecutar el [script de registro previo](https://aka.ms/scriptforpermsonhana). Normalmente, hemos detectado que el proceso de actualización elimina los roles necesarios. La ejecución del script de registro previo le ayudará a comprobar todos los roles necesarios.
- [Reanude la protección](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) de la base de datos nuevamente.

## <a name="re-registration-failures"></a>Errores de repetición del registro

Compruebe uno o varios de los siguientes síntomas antes de desencadenar la operación de repetición del registro:

- Se producen errores en todas las operaciones (tales como copia de seguridad, restauración y configuración de copia de seguridad) en la máquina virtual con uno de los códigos de error siguientes: **WorkloadExtensionNotReachable, UserErrorWorkloadExtensionNotInstalled, WorkloadExtensionNotPresent, WorkloadExtensionDidntDequeueMsg**.
- Si el área **Estado de copia de seguridad** del elemento de copia de seguridad se muestra como **no accesible**, descarte todas las demás causas que podrían dar lugar al mismo estado:

  - Falta de permiso para realizar operaciones relacionadas con la copia de seguridad en la máquina virtual
  - La máquina virtual está apagada, por lo que no se pueden realizar copias de seguridad
  - Problemas de red

Estos síntomas pueden surgir por uno o varios de los siguientes motivos:

- Se ha eliminado o desinstalado una extensión del portal.
- Se ha restaurado la máquina virtual retrocediendo en el tiempo a través de la restauración del disco o discos en contexto.
- Se ha apagado la máquina virtual durante un largo período, por lo que la configuración de la extensión en ella ha caducado.
- Se ha eliminado la máquina virtual, y se ha creado otra con el mismo nombre y en el mismo grupo de recursos que la máquina virtual eliminada.

En los escenarios anteriores, se recomienda desencadenar una operación de nuevo registro en la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

- Revise las [preguntas frecuentes](https://docs.microsoft.com/azure/backup/sap-hana-faq-backup-azure-vm) sobre cómo hacer copias de seguridad de las bases de datos SAP HANA en máquinas virtuales de Azure.
