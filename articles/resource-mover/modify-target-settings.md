---
title: Modificación de la configuración de destino al mover máquinas virtuales de Azure entre regiones con Azure Resource Mover
description: Obtenga información sobre la modificación de la configuración de destino al mover máquinas virtuales de Azure entre regiones con Azure Resource Mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: ca22def6bc152d03c3992ed7e94ac2b5ccf179e9
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604485"
---
# <a name="modify-target-settings"></a>Modificación de la configuración de destino

En este artículo se describe cómo modificar la configuración de destino cuando se mueven recursos entre regiones de Azure con [Azure Resource Mover](overview.md).


## <a name="modify-vm-settings"></a>Modificación de la configuración de máquina virtual

Al mover máquinas virtuales de Azure y recursos asociados, puede modificar la configuración de destino. 

- Se recomienda cambiar la configuración de destino después de validar la colección de movimiento.
- Se recomienda modificar la configuración antes de preparar los recursos, ya que es posible que algunas propiedades de destino no estén disponibles para su edición una vez completada la preparación.

Pero:
- Si va a mover el recurso de origen, normalmente puede modificar la configuración de destino hasta que inicie el proceso de movimiento.
- Si asigna un recurso existente en la región de origen, puede modificar la configuración de destino hasta que se complete la confirmación del movimiento.

### <a name="settings-you-can-modify"></a>Opciones de configuración que se pueden modificar

Las opciones de configuración que se pueden modificar se resumen en la tabla.

**Recurso** | **Opciones** 
--- | --- | --- 
**Nombre de VM** | Opciones:<br/><br/> - Crear una nueva máquina virtual con el mismo nombre en la región de destino.<br/><br/> - Crear una nueva máquina virtual con un nombre diferente en la región de destino.<br/><br/> - Usar una máquina virtual existente en la región de destino.<br/><br/> Si crea una nueva máquina virtual, con la excepción de los valores que modifique, a la nueva máquina virtual de destino se le asignará la misma configuración que la de la máquina virtual de origen.
**Zona de disponibilidad de máquina virtual** | La zona de disponibilidad en la que se colocará la máquina virtual de destino. Esto se puede marcar como **NA** si no desea cambiar la configuración de origen o si no desea colocar la máquina virtual en una zona de disponibilidad.
**SKU de la máquina virtual** | El [tipo de máquina virtual](https://azure.microsoft.com/pricing/details/virtual-machines/series/) (disponible en la región de destino) que se usará para la máquina virtual de destino.<br/><br/> La máquina virtual de destino seleccionada no debe ser más pequeña que la máquina virtual de origen.
**Recursos de redes** | Opciones para redes virtuales (Vnet)/grupos de seguridad de red/interfaces de red:<br/><br/> - Crear un nuevo recurso con el mismo nombre en la región de destino.<br/><br/> - Crear un nuevo recurso con un nombre diferente en la región de destino.<br/><br/> - Usar un recurso de red existente en la región de destino.<br/><br/> Si crea un nuevo recurso de destino, con la excepción de los valores que modifique, se ale signará la misma configuración que la del recurso de origen.
**Nombre de la dirección IP pública** | Especifique el nombre.
**SKU de la dirección IP pública** | Especifique la [SKU](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#sku).
**Zona de la dirección IP pública** | Especifique la [zona](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#standard) para las direcciones IP públicas estándar.<br/><br/> Si desea que tenga redundancia de zona, especifíquela como **Con redundancia de zona**.
**Nombre del equilibrador de carga** | Especifique el nombre.
**SKU del equilibrador de carga** | Básico o Estándar. Es recomendable usar Estándar.
**Zona del equilibrador de carga** | Especifique una zona para el equilibrador de carga. <br/><br/> Si desea que tenga redundancia de zona, especifíquela como **Con redundancia de zona**.
**Dependencias de recursos** | Opciones para cada dependencia:<br/><br/>- El recurso usa recursos dependientes del origen que se moverán a la región de destino.<br/><br/> - El recurso usa distintos recursos dependientes ubicados en la región de destino. En este caso, puede elegir entre cualquier recurso similar de la región de destino.

### <a name="edit-vm-target-settings"></a>Edición de la configuración de destino de VM

Si no desea que los recursos de la región de origen dependan del destino, tiene un par de opciones más:

- Cree un recurso en la región de destino. A menos que especifique una configuración diferente, el nuevo recurso tendrá la misma configuración que el recurso de origen.
- Use un recurso existente en la región de destino.

El comportamiento exacto depende del tipo de recurso. [Obtenga más información](modify-target-settings.md) sobre cómo modificar la configuración de destino.

Puede modificar la configuración de destino de un recurso mediante la entrada **Configuración de destino** en la colección del traslado de recursos. 

Para modificar un valor de configuración: 

1. En la página **Entre regiones** > columna **Configuración de destino**, haga clic en el vínculo de la entrada del recurso.
2. En **Parámetros de configuración**, puede crear una nueva máquina virtual en la región de destino.
3. Asigne una nueva zona de disponibilidad, un conjunto de disponibilidad o una SKU a la máquina virtual de destino. **Zona de disponibilidad** y **SKU**.

Solo se realizan cambios para el recurso que se edita. Los recursos dependientes deben actualizarse por separado.


## <a name="modify-sql-settings"></a>Modificación de la configuración de SQL

Al mover recursos de Azure SQL Database, puede modificar la configuración de destino para el traslado. 

- Para una base de datos SQL:
    - Se recomienda modificar la configuración de destino antes de prepararla para el traslado.
    - Puede modificar la configuración de la base de datos de destino y la redundancia de zona para la base de datos.
- Para grupos elásticos:
    -  Puede modificar la configuración de destino en cualquier momento antes de iniciar el traslado.
    - Puede modificar el grupo elástico de destino y la redundancia de zona para el grupo. 

### <a name="sql-settings-you-can-modify"></a>Opciones de SQL que puede modificar

**Configuración** | **SQL database** | **Grupo elástico**
--- | --- | ---
**Nombre** | Cree una nueva base de datos con el mismo nombre en la región de destino.<br/><br/> Cree una nueva base de datos con un nombre diferente en la región de destino.<br/><br/> Use una base de datos existente en la región de destino. | Cree un nuevo grupo elástico con el mismo nombre en la región de destino.<br/><br/> Cree un nuevo grupo elástico con un nombre diferente en la región de destino.<br/><br/> Use un grupo elástico existente en la región de destino.
**Redundancia de zona** | Para mover de una región que admite la redundancia de zona a una región que no la admite, escriba **Deshabilitar** en la configuración de zona.<br/><br/> Para mover de una región que no admite la redundancia de zona a una región que sí la admite, escriba **Habilitar** en la configuración de zona. | Para mover de una región que admite la redundancia de zona a una región que no la admite, escriba **Deshabilitar** en la configuración de zona.<br/><br/> Para mover de una región que no admite la redundancia de zona a una región que sí la admite, escriba **Habilitar** en la configuración de zona.

### <a name="edit-sql-target-settings"></a>Edición de la configuración de destino de SQL

Modifique la configuración de destino de un recurso de Azure SQL Database como se indica a continuación: 

1. En **Entre regiones**, para el recurso que desea modificar, haga clic en la entrada **Configuración de destino**.
2. En **Parámetros de configuración**, especifique la configuración de destino resumida en la tabla anterior.

## <a name="next-steps"></a>Pasos siguientes

[Traslade una máquina virtual de Azure](tutorial-move-region-virtual-machines.md) a otra región.
