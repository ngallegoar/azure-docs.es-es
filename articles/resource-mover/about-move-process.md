---
title: Acerca del proceso de traslado en Azure Resource Mover
description: Más información sobre el proceso de traslado de recursos entre regiones con Azure Resource Mover
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: overview
ms.date: 09/09/2020
ms.author: raynew
ms.openlocfilehash: 5261904dd1ee7f280209015d8f756a055dfab57e
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95522959"
---
# <a name="about-the-move-process"></a>Acerca del proceso de traslado

[Azure Resource Mover](overview.md) le ayuda a trasladar los recursos de Azure entre regiones de Azure. En este artículo se resumen los componentes que usa Resource Mover y se describe el proceso de traslado. 


## <a name="components"></a>Componentes

Estos componentes se usan durante el traslado de región.

**Componente** | **Detalles**
--- | ---
**Resource Mover** |  Resource Mover se coordina con los [proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md) para orquestar el traslado de recursos entre regiones. Resource Mover analiza las dependencias de los recursos y mantiene y administra el estado de los recursos durante el proceso de traslado. 
**Colección de movimiento** |  Una colección de movimiento es un objeto de [Azure Resource Manager](../azure-resource-manager/management/overview.md).<br/><br/> La colección de movimiento se crea durante el proceso de traslado de región, para cada combinación emparejada de las regiones de origen y de destino de una suscripción. La colección almacena metadatos e información de configuración de los recursos que se quieren trasladar.<br/><br/>Los recursos agregados a una colección de movimiento deben estar en la misma suscripción, pero pueden estar en distintos grupos de recursos. 
**Recurso de traslado** | Cuando se agrega un recurso a una colección de movimiento, Resource Mover realiza un seguimiento de él como un recurso de traslado.<br/><br/> Resource Mover mantiene información de todos los recursos de traslado en la colección de movimiento y mantiene una relación uno a uno entre el recurso de origen y el de destino. 
**Dependencias** | Resource Mover valida los recursos que se agregan a una colección y comprueba si los recursos tienen dependencias que no están en la colección de movimiento.<br/><br/> Después de identificar las dependencias de un recurso, puede agregarlas a la colección de movimiento y trasladarlas también, o bien puede seleccionar otros recursos existentes en la región de destino. Se deben resolver todas las dependencias antes de iniciar el traslado. 



## <a name="move-region-process"></a>Proceso de traslado de región 

![Diagrama que muestra los pasos del traslado](./media/about-move-process/move-steps.png)

A cada recurso de traslado se le aplican los pasos resumidos.

**Step** | **Detalles** | **Estado y problemas**
--- | --- | --- 
**Paso 1: Selección de recursos** | Seleccione un recurso. El recurso se agrega a la colección de movimiento. | El estado de los recursos se cambia a *La acción "Preparar" está pendiente*.<br/><br/> Si el recurso tiene dependencias, *Validar dependencias* indica que es necesario agregar recursos dependientes a la colección de movimiento.
**Paso 2: Validación de dependencias** | Inicie el proceso de validación.<br/><br/> Si la validación muestra que los recursos dependientes están pendientes, agréguelos a la colección de movimiento. <br/><br/> Asegúrese de agregar todos los recursos dependientes, incluso si no desea trasladarlos. Más adelante, podrá especificar que los recursos que va a trasladar deben usar distintos recursos en la región de destino.<br/><br/> Vuelva a validar, hasta que no haya dependencias pendientes. | Una vez que se agregan todas las dependencias y la validación se realiza correctamente, el estado de los recursos se cambia a *La acción "Preparar" está pendiente*, sin problemas detectados.
**Paso 3: Preparación** | Inicie el proceso de preparación. Los pasos de preparación dependen de los recursos que se van a trasladar:<br/><br/> - **Recursos sin estado**: los recursos sin estado solo tienen información de configuración. Estos recursos no necesitan una replicación continua de los datos para trasladarlos. Algunos ejemplos son las redes virtuales de Azure, los adaptadores de red, los equilibradores de carga y los grupos de seguridad de red. Para este tipo de recurso, el proceso de preparación genera una plantilla de Azure Resource Manager.<br/><br/> - **Recursos con estado**: los recursos con estado tienen tanto información de configuración como datos que se deben trasladar. Entre los ejemplos se incluyen las máquinas virtuales de Azure y las bases de datos de Azure SQL. El proceso de preparación es diferente para cada recurso. Podría incluir la replicación del recurso de origen en la región de destino. | Al iniciarse, se cambia el estado de los recursos a *La acción "Preparar" está en curso*.<br/><br/> Una vez finalizada la preparación, el estado del recurso cambia a *La acción "Iniciar movimiento" está pendiente*, sin problemas detectados.<br/><br/> Un proceso incorrecto cambia el estado a *Error en la acción "Preparar"* .
**Paso 4: Inicio del traslado** | Inicie el proceso de traslado. El método de traslado depende del tipo de recurso:<br/><br/> - **Sin estado**: normalmente, para los recursos sin estado, el proceso de traslado implementa una plantilla importada en la región de destino. La plantilla se basa en la configuración del recurso de origen y en cualquier edición manual que realice en la configuración de destino.<br/><br/> - **Con estado**: en el caso de los recursos con estado, el proceso de traslado puede implicar la creación del recurso o la habilitación de una copia en la región de destino.<br/><br/>  Solo en el caso de los recursos con estado, el inicio de un traslado podría provocar un tiempo de inactividad en los recursos de origen. Por ejemplo, máquinas virtuales y SQL. | Al iniciar el traslado, el estado cambia a *La acción "Iniciar movimiento" está en curso*.<br/><br/> Un inicio de traslado correcto cambia el estado de los recursos a *La acción "Confirmar movimiento" está pendiente*, sin problemas detectados. <br/><br/> Un proceso de traslado incorrecto cambia el estado a *Error en la acción "Iniciar movimiento"* .
**Paso 5, opción 1: Descarte del traslado** | Después del traslado inicial, puede decidir si desea continuar con un traslado completo. Si no lo desea, puede descartar el traslado y Resource Mover elimina los recursos creados en el destino. El proceso de replicación de los recursos con estado continúa después del proceso de descarte. Esta opción es útil para realizar pruebas. | Al descartar los recursos, se cambia el estado a *La acción "Descartar movimiento" está en curso*.<br/><br/> Un descarte correcto cambia el estado a *La acción "Iniciar movimiento" está pendiente*, sin problemas detectados.<br/><br/> Un error en el descarte cambia el estado a *Error en la acción "Descartar movimiento"* . 
**Paso 5, opción 2: Confirmación del traslado** | Después del traslado inicial, si desea continuar con un traslado completo, compruebe los recursos de la región de destino y, cuando esté listo, confirme el traslado.<br/><br/> Solo para los recursos con estado, la confirmación puede dar lugar a que los recursos de origen, como las máquinas virtuales o SQL, sean inaccesibles. | Si confirma el traslado, el estado del recurso cambia a *La acción "Confirmar movimiento" está en curso**.<br/><br/> Una vez confirmado correctamente, el estado del recurso muestra *Commit move completed* (La acción "Confirmar movimiento" ha finalizado), sin problemas detectados.<br/><br/> Una confirmación con error cambia el estado a *Error en la acción "Confirmar movimiento"* .
**Paso 6: Eliminación en el origen** | Después de confirmar el traslado y comprobar los recursos en la región de destino, puede eliminar el recurso de origen. | Una vez confirmado el traslado, los recursos tendrán el estado *La acción "Eliminar origen" está pendiente*.


## <a name="move-region-states"></a>Estados del traslado de región

El proceso de traslado tiene varios estados y problemas que pueden surgir durante cada estado. Se resumen en el siguiente diagrama de flujo.

![Diagrama de flujo de los estados y problemas posibles](./media/about-move-process/process.png)


### <a name="dependency-analysis"></a>Análisis de dependencias

A medida que avanza por el proceso de traslado, es posible que se le pida que valide las dependencias si:
- Un recurso utiliza recursos dependientes que no están en la colección de movimiento.
- Un recurso dependiente de la colección de movimiento tiene sus propias dependencias, que no están en la colección de movimiento.
- Ha modificado la configuración de destino del recurso y debe volver a validar las dependencias.


### <a name="remove-resources"></a>Eliminación de recursos

Si no desea trasladar un recurso, puede eliminarlo de la colección de movimiento. En general, el recurso se elimina de la colección, junto con las acciones u objetos asociados, como la replicación o las plantillas almacenadas. Lo que ocurre exactamente cuando se elimina un recurso depende del tipo de recurso y del estado del recurso cuando se elimina. [Learn more](remove-move-resources.md).

## <a name="move-impact"></a>Impacto del traslado

En la tabla se resume a qué afecta un traslado entre regiones.

**Comportamiento** | **Entre regiones**
--- | --- | --- 
**Data** | Se trasladan los datos y los metadatos del recurso.<br/><br/> Los metadatos se almacenan de forma temporal para realizar un seguimiento del estado de las dependencias y las operaciones de los recursos.
**Recurso** | El recurso de origen permanece intacto para asegurarse de que las aplicaciones continúan funcionando y, opcionalmente, se puede eliminar después del traslado.<br/><br/> Se crea un recurso en la región de destino.
Proceso de movimiento | Proceso de varios pasos que requiere intervención manual y supervisión.
**Pruebas**: | Las pruebas del traslado son importantes, ya que las aplicaciones deben continuar funcionando según lo previsto en la región de destino después del traslado.
**Tiempo de inactividad**: |  No se espera ninguna pérdida de datos, sino cierto tiempo de inactividad para trasladar los recursos.



## <a name="next-steps"></a>Pasos siguientes

[Traslado](tutorial-move-region-virtual-machines.md) de máquinas virtuales de Azure entre regiones.
[Traslado](tutorial-move-region-sql.md) de recursos de Azure SQL Database a otra región.