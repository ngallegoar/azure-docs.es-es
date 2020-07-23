---
title: Registros de auditoría para eventos de Azure Data Box y Azure Data Box Heavy | Microsoft Docs
description: Describe los registros de auditoría completos de Data Box que se recopilan en las distintas fases del pedido de Azure Data Box y Azure Data Box Heavy.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 07/10/2020
ms.author: alkohli
ms.openlocfilehash: 50dbe9ab649a708fb36b1c9e4fe89bccadc7ea90
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209176"
---
# <a name="audit-logs-for-your-azure-data-box-and-azure-data-box-heavy"></a>Registros de auditoría para Azure Data Box y Azure Data Box Heavy

Los registros son documentos inmutables y con marca de tiempo de eventos discretos que se produjeron a lo largo del tiempo. Los registros contienen información de diagnóstico, auditoría y seguridad sobre el dispositivo.  

Un pedido de Data Box o Data Box Heavy se pasa por los pasos siguientes durante su funcionamiento: pedido, configuración, copia de datos, devolución, carga en Azure y comprobación, y eliminación de datos. Todos los eventos se auditan y registran para cada uno de estos pasos.

Este artículo contiene información sobre los registros de auditoría de Data Box, incluidos los tipos y ubicación de los registros, así como la información recopilada. 

La información de este artículo se aplica tanto a Data Box como a Data Box Heavy. En las secciones posteriores, todas las referencias a Data Box también se aplican a Data Box Heavy. En este artículo no se describen los registros recopilados del servicio Data Box que se ejecuta en Azure. 


## <a name="about-audit-logs"></a>Acerca de los registros de auditoría 

En su instancia de Data Box se recopilan los siguientes registros:

- **Registros del sistema**: dado que Data Box es un dispositivo basado en Windows, se registran todos los eventos de hardware, software y sistema. Un conjunto de estos eventos se recopila y se incluye en los registros de auditoría del sistema. 

- **Seguridad**: dado que Data Box es un dispositivo basado en Windows, se registran todos los eventos de seguridad. Un conjunto de estos eventos se recopila y se incluye en los registros de auditoría de seguridad. 

- **Aplicación**: estos registros son específicos solo para Data Box. Estos registros contienen todos los eventos generados en el dispositivo en respuesta a los servicios de Data Box en ejecución.

En las siguientes secciones se detallan cada uno de estos registros.

### <a name="system-logs"></a>Registros del sistema

Los siguientes id. de eventos del registro del sistema se recopilan como registros de auditoría del sistema en su instancia de Data Box:

|Nombre del proveedor de eventos     |Id. de evento recopilado   |Descripción del evento   |
|-------------------|----------|----------------|
|Microsoft-Windows-Kernel-General|12  |Hora UTC en que se reinició el sistema operativo.   |
|                                |13  |Hora UTC en que se apagó el sistema operativo. |
|    |                              |
|Microsoft-Windows-Kernel-Power  |41  |El sistema se reinició sin un cierre correcto.| 
|    |                              |
|Microsoft-Windows-BitLocker-Driver|All|    |

### <a name="security-logs"></a>Registros de seguridad

Los siguientes id. de eventos del registro de seguridad se recopilan como registros de auditoría de seguridad en su instancia de Data Box:

|Nombre del proveedor de eventos                   |Id. de evento recopilado    |Descripción del evento       |
|--------------------------------------|------------|----------|
|Microsoft-Windows-Security-Auditing   |4624        |Inicio de sesión correcto. |
|                                      |4625        |Error de inicio de sesión en la cuenta. Nombre de usuario desconocido o contraseña incorrecta. |
|                                     

### <a name="application-logs"></a>Registros de aplicación

Los siguientes id. de eventos del registro de aplicaciones se recopilan como parte de los registros de auditoría de paquetes en su instancia de Data Box.     

- **Microsoft-Azure-DataBox-OOBE-Auditing**: contiene los eventos que se producen en la interfaz de usuario local. 
- **Microsoft-Azure-DataBox-Reprovision-Audit**: contiene los eventos relacionados con volver a aprovisionar el dispositivo de Data Box. El proceso de volver a aprovisionar la instancia de Data Box se produce cuando se restablece el dispositivo a través de la interfaz de usuario local. Elija esta opción si quiere borrar los datos copiados al quitar los recursos compartidos existentes y volver a crearlos como parte de volver a provisionar o restablecer el dispositivo.
- **Microsoft-Azure-DataBox-HcsMgmt-Audit**: contiene eventos relacionados únicamente con el paso **Preparar para enviar** antes de que el dispositivo se envíe de vuelta al centro de datos de Azure. 
- **Microsoft-Azure-DataBox-IfxAudit**: contiene los mensajes registrados por diferentes entidades del producto acerca de los trabajos; estos registros incluyen más información sobre lo que está ocurriendo en algunos de los flujos.

Esta es una tabla que resume los distintos proveedores de eventos y los id. de evento correspondientes que se recopilarán en cada caso.

|Nombre del proveedor de eventos    |Id. de evento    | Notas |
|-----------------|-----------------|-------------------|
|Microsoft-Azure-DataBox-OOBE-Auditing |4624        |Inicio de sesión correcto.|
|                                      |4625        |Error de inicio de sesión en la cuenta. Nombre de usuario desconocido o contraseña incorrecta.|
|                                     |4634        |Evento de cierre de sesión.|
|                                   |  | |
|Microsoft-Azure-DataBox-Reprovision-Audit    |65001       |Evento de reaprovisionamiento correcto.|
|                                                  |65002       |Error de evento de reaprovisionamiento.|
|                                                  |                 |         |
|Microsoft-Azure-DataBox-HcsMgmt-Audit        |65003       |Evento del estado de Preparación para el envío NotStarted, InProgress, Failed, Canceled, Succeeded, ScanCompletedWithIssues, SucceededWithWarnings          |
|                                                  |                 |     |
|Microsoft-Azure-DataBox-IfxAudit    |All |Todos los eventos se registran con la API de registro de auditoría en el código |

Este es un ejemplo del registro de auditoría del marco de instrumentación (IFX):

|     Tarea/trabajo/API                              |     Eventos registrados                                                                                                              | 
|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|     Limpieza                                   |     Se registran los eventos relacionados con el inicio, finalización o error de un trabajo de limpieza. |                                              
|     Preparación del dispositivo para el envío del cliente    |     Se registran los eventos relacionados con el inicio, finalización o error del trabajo para preparar el dispositivo para su envío. |
|     Aprovisionamiento                                 |     Se registran los eventos relacionados con el inicio, finalización o error de un trabajo de aprovisionamiento de dispositivos.|
|     Trabajo de paquete de auditoría                       |     Se registran los eventos relacionados con el inicio, finalización o error de un trabajo de paquete de auditoría que crea registros de la cadena de custodia.|
|     Sobrescritura de disco                          |     Se registra un error al sobrescribir el disco.|
|     Habilitar o deshabilitar la instancia remota de PowerShell     |     Se registran los eventos relacionados con la habilitación o deshabilitación de la instancia remota de PowerShell en el dispositivo. |
|     Obtener detalles de la fase de instalación               |     Los eventos relacionados con la instalación de software en el dispositivo en fases se registran en el centro de datos de Azure.|
|     Desbloquear o bloquear el volumen de BitLocker           |     Los eventos se registran para indicar el estado de BitLocker de los volúmenes *basevolume* y *hcsdata*.|
|     Corregir el disco                              |     Se registran los eventos relacionados con errores de los discos físicos que no se pudieron borrar, así como los eventos de cuando todos los discos físicos del dispositivo se borran correctamente. |
|     Habilitar o deshabilitar el usuario local               |     Se registran los eventos relacionados con la habilitación o deshabilitación de las cuentas de usuario locales para StorSimpleAdmin y PodSupportAdminUser.| 
|     Restablecimiento de contraseña                          |     Se registran los eventos relacionados con el restablecimiento de contraseñas correcto o erróneo para el usuario StorSimpleAdmin local. |


Además de los registros de auditoría de IFX, también se recopilan los registros de auditoría de la cadena de custodia para Data Box. Estos registros no se pueden ver en tiempo real, sino solo después de que se complete el trabajo y se borren los datos de los discos de Data Box. Estos registros contienen un subconjunto de la información contenida en los registros de auditoría de IFX.

Para obtener más información sobre los registros de auditoría de la cadena de custodia, consulte [Obtener la cadena de registros de custodia después de la eliminación de datos](data-box-logs.md#get-chain-of-custody-logs-after-data-erasure).

<!-- write a few lines about order history and link out to the detailed section on order history-->

## <a name="access-audit-logs"></a>Acceso a registros de auditoría

Estos registros se almacenan en Azure y no se puede acceder a ellos directamente. Si necesita acceder a estos registros, envíe una incidencia de soporte técnico. Para obtener más información, consulte [Ponerse en contacto con el soporte técnico de Microsoft](data-box-disk-contact-microsoft-support.md). 

Una vez que se haya enviado la incidencia de Soporte técnico, Microsoft descargará estos registros y le proporcionará acceso a ellos.


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [solucionar problemas relacionados con el almacenamiento de blobs de Azure Data Box](data-box-troubleshoot.md).
