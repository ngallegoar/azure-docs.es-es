---
title: Visualización de las valoraciones de actualizaciones de Azure Automation
description: En este artículo se explica cómo ver las valoraciones de actualizaciones para las implementaciones de Update Management.
services: automation
ms.subservice: update-management
ms.date: 01/21/2020
ms.topic: conceptual
ms.openlocfilehash: 321146364897d46a403bdfd6789fcb219179d88c
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830606"
---
# <a name="view-update-assessments"></a>Visualización de valoraciones de las actualizaciones

En la cuenta de Azure Automation, seleccione **Update Management** para ver el estado de sus máquinas.

Esta vista proporciona información sobre las máquinas, las actualizaciones que faltan, las implementaciones de actualizaciones y las implementaciones de actualizaciones programadas. En la columna **CUMPLIMIENTO**, puede ver la última vez que se evaluó la máquina. En la columna **PREPARACIÓN DE ACTUALIZACIONES DEL AGENTE**, puede ver el estado del agente de actualización. Si hay un problema, seleccione el vínculo para dirigirse a la documentación de solución de problemas, que le ayudará a corregirlo.

Para ejecutar una búsqueda de registros que devuelva información sobre la máquina, la actualización o la implementación, seleccione el elemento correspondiente de la lista. Se abre el panel Búsqueda de registros con una consulta para el elemento seleccionado.

![Vista predeterminada de Update Management](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Visualización de actualizaciones que faltan

Haga clic en **Actualizaciones que faltan** para ver la lista de actualizaciones que faltan en las máquinas. Se muestran todas las actualizaciones, y se pueden seleccionar. Se muestran los datos sobre el número de máquinas que requieren la actualización, los detalles del sistema operativo y un vínculo para obtener más información. El panel Búsqueda de registros también muestra más información sobre las actualizaciones.

![Actualizaciones que faltan](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="work-with-update-classifications"></a>Trabajo con las clasificaciones de actualizaciones

En las siguientes tablas, se muestran las clasificaciones de las actualizaciones de Update Management admitidas, junto con una definición de cada una de ellas.

### <a name="windows"></a>Windows

|clasificación  |Descripción  |
|---------|---------|
|Actualizaciones críticas     | Actualizaciones para problemas específicos que resuelven un error crítico no relacionado con la seguridad.        |
|Actualizaciones de seguridad     | Actualizaciones para problemas específicos del producto relacionados con la seguridad.        |
|Paquetes acumulativos de actualizaciones     | Conjunto de revisiones que se empaquetan para facilitar la implementación.        |
|Feature Packs     | Nuevas características del producto que se distribuyen fuera de una versión del producto.        |
|Service Packs     | Conjuntos de revisiones que se aplican a una aplicación.        |
|Actualizaciones de definiciones     | Actualizaciones de archivos de definiciones de virus o de otra índole.        |
|Herramientas     | Utilidades o características que ayudan a realizar una o más tareas.        |
|Actualizaciones     | Actualizaciones de aplicaciones o archivos instalados actualmente.        |

### <a name="linux"></a><a name="linux-2"></a>Linux

|clasificación  |Descripción  |
|---------|---------|
|Actualizaciones críticas y de seguridad     | Actualizaciones para un problema específico o un problema de un producto específico relacionado con la seguridad.         |
|Otras actualizaciones     | Todas las demás actualizaciones que ni son críticas por naturaleza ni son actualizaciones de seguridad.        |

En Linux, Update Management puede distinguir entre actualizaciones críticas y de seguridad en la nube y mostrar al mismo tiempo datos de evaluación. (Esta granularidad es posible gracias al enriquecimiento de los datos en la nube). Para aplicar revisiones, Update Management se basa en los datos de clasificación disponibles en la máquina. A diferencia de otras distribuciones, CentOS no dispone de esta información en las versiones RTM del producto. Si tiene máquinas de CentOS configuradas para devolver datos de seguridad para el siguiente comando, Update Management puede aplicar revisiones basadas en clasificaciones:

```bash
sudo yum -q --security check-update
```

Actualmente no hay ningún método compatible para habilitar la disponibilidad de datos de clasificación nativos en CentOS. En este momento, solo se proporciona soporte técnico a clientes que han habilitado esta funcionalidad por su cuenta.

Para clasificar las actualizaciones de la versión 6 de Red Hat Enterprise, debe instalar el complemento de seguridad de yum. En Red Hat Enterprise Linux 7, el complemento ya forma parte de yum, por lo que no es necesario instalar nada. Para más información, consulte el siguiente [artículo de conocimientos](https://access.redhat.com/solutions/10021) de Red Hat.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información general, vea [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
