---
title: Límites y cuotas del servicio
description: Obtenga información sobre las restricciones, los límites y las cuotas de Azure Batch predeterminados y cómo solicitar un aumento de la cuota.
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 8ca08d43f07633b58cf6f7067c1a8fcd58350678
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92107545"
---
# <a name="batch-service-quotas-and-limits"></a>Límites y cuotas del servicio Batch

Al igual que en otros servicios de Azure, existen límites en determinados recursos asociados con el servicio Batch. Muchos de ellos son cuotas predeterminadas que Azure aplica en el nivel de cuenta o suscripción.

Tenga en cuenta estas cuotas al diseñar y escalar verticalmente sus cargas de trabajo de Batch. Por ejemplo, si su grupo no alcanza el número objetivo de nodos de proceso especificado, es posible que se haya alcanzado el límite de cuota de núcleos de la cuenta de Batch.

Se pueden ejecutar varias cargas de trabajo de Batch en una sola cuenta de Batch, o bien distribuir las cargas de trabajo entre cuentas de Batch que se encuentren en la misma suscripción, pero en diferentes regiones de Azure.

Si planea ejecutar cargas de trabajo de producción en Batch, es posible que tenga que aumentar el valor predeterminado de una o varias de las cuotas. Si desea aumentar una cuota, puede abrir una [solicitud de soporte técnico al cliente](#increase-a-quota) en línea sin ningún costo.

## <a name="resource-quotas"></a>Cuotas de recursos

Una cuota es un límite de crédito, no una garantía de capacidad. Si tiene necesidades de capacidad a gran escala, póngase en contacto con el soporte técnico de Azure.

Tenga en cuenta también que las cuotas no son valores garantizados. Las cuotas pueden variar a partir de los cambios del servicio Batch o una solicitud de usuario para cambiar un valor de cuota.

[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="cores-quotas-in-user-subscription-mode"></a>Cuotas de núcleos en el modo de suscripción de usuario

Si ha creado una [cuenta de Batch](accounts.md) con el modo de asignación de grupo establecido en **suscripción de usuario**, las cuotas se aplican de manera diferente. En este modo, las máquinas virtuales de Batch y otros recursos se crean directamente en la suscripción cuando se crea un grupo. Las cuotas de núcleos de Azure Batch no se aplican a las cuentas creadas en este modo. En su lugar, se aplican las cuotas en la suscripción para núcleos de proceso regionales y otros recursos.

Para más información sobre estas cuotas, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>Límites de tamaño de grupo

El servicio Batch establece los límites de tamaño del grupo. A diferencia de las [cuotas de recursos](#resource-quotas), estos valores no se pueden cambiar. Solo los grupos con la comunicación entre nodos e imágenes personalizadas tienen restricciones diferentes de la cuota estándar.

| **Recurso** | **Límite máximo** |
| --- | --- |
| **Nodos de proceso en el [grupo con la comunicación entre nodos habilitada](batch-mpi.md)**  ||
| Modo de asignación de grupo de servicio Batch | 100 |
| Modo de asignación de grupo de suscripción Batch | 80 |
| **Nodos de proceso en el [grupo creado con un recurso de imagen administrada](batch-custom-images.md)** <sup>1</sup> ||
| Nodos dedicados | 2000 |
| Nodos de prioridad baja | 1000 |

<sup>1</sup> Para los grupos que no tienen habilitada la comunicación entre nodos.

## <a name="other-limits"></a>Otros límites

El servicio Batch establece límites adicionales. A diferencia de [las cuotas de recursos](#resource-quotas), estos valores no se pueden cambiar.

| **Recurso** | **Límite máximo** |
| --- | --- |
| [Tareas simultáneas](batch-parallel-node-tasks.md) por nodo de proceso | 4 × número de núcleos de nodo |
| [Aplicaciones](batch-application-packages.md) por cuenta de Batch | 20 |
| Paquetes de aplicación por aplicación | 40 |
| Paquetes de aplicación por grupo | 10 |
| Duración máxima de la tarea | 180 días<sup>1</sup> |
| [Montajes](virtual-file-mount.md) por nodo de ejecución | 10 |

<sup>1</sup> La duración máxima de una tarea, desde el momento en que se agrega al trabajo hasta que se completa, es de 180 días. Las tareas completadas se mantienen durante siete días; los datos de las tareas no completadas dentro de la duración máxima no están accesibles.

## <a name="view-batch-quotas"></a>Visualización de las cuotas de Batch

Para ver las cuotas de la cuenta de Batch en [Azure Portal](https://portal.azure.com):

1. Seleccione **Cuentas de Batch** y, después, seleccione la cuenta de Batch que le interesa.
1. Seleccione **Cuotas** en el menú de la cuenta de Batch.
1. Vea las cuotas aplicadas actualmente a la cuenta de Batch.

    ![Cuotas de la cuenta de Batch][account_quotas]

## <a name="increase-a-quota"></a>Aumento de la cuota

Puede solicitar un aumento de la cuota para la cuenta de Batch o la suscripción mediante [Azure Portal](https://portal.azure.com). El tipo de aumento de cuota depende del modo de asignación de grupo de su cuenta de Batch. Para solicitar un aumento de cuota, debe incluir la serie de VM para la que quiere aumentar la cuota. Cuando se aplica el aumento de cuota, se aplica a todas las series de VM.

1. Seleccione el icono **Ayuda y soporte técnico** en el panel del portal o el signo de interrogación ( **?** ) en la esquina superior derecha del portal.
1. Seleccione **Nueva solicitud de soporte técnico** > **Básico**.
1. En **Aspectos básicos**:
   
    1. **Tipo de incidencia** > **Límites de servicio y suscripción (cuotas)**
   
    1. Seleccione su suscripción.
   
    1. **Tipo de cuota** > **Batch**
      
       Seleccione **Next** (Siguiente).
    
1. En **Details** (Detalles):
      
    1. En **Proporcionar detalles**, especifique la ubicación, el tipo de cuota y la cuenta de Batch.
    
       ![Aumento de la cuota de Batch][quota_increase]

       Entre los tipos de cuota, se incluyen los siguientes:

       * **Por cuenta de Batch**  
         Valores específicos de una única cuenta de Batch, incluidos los núcleos dedicados y de baja prioridad, así como el número de trabajos y grupos.
        
       * **Por región**  
         Valores que se aplican a todas las cuentas de Batch de una región. Incluye el número de cuentas de Batch por región y suscripción.

       La cuota de prioridad baja es un valor único en todas las series de VM. Si necesita SKU restringidas, debe seleccionar **Núcleos de baja prioridad** e incluir las familias de VM que se solicitarán.

    1. Seleccione una de las opciones en **Gravedad** según su [impacto en el negocio](https://aka.ms/supportseverity).

       Seleccione **Next** (Siguiente).

1. En **Información de contacto**:
   
    1. Seleccione un valor en **Método de contacto preferido**.
   
    1. Compruebe y especifique los detalles de contacto necesarios.
   
       Seleccione **Crear** para enviar la solicitud de soporte técnico.

Una vez que haya enviado la solicitud de soporte técnico, el servicio de soporte técnico de Azure se comunicará con usted. Las solicitudes de cuota se pueden completar en unos pocos minutos o en hasta dos días laborables.

## <a name="related-quotas-for-vm-pools"></a>Cuotas relacionadas para grupos de máquinas virtuales

Los grupos de Batch en la configuración de máquina virtual implementada en una red virtual de Azure asignan automáticamente recursos de red de Azure adicionales. Se necesitan los siguientes recursos por cada 50 nodos de grupo en una red virtual:

- Un [grupo de seguridad de red](../virtual-network/network-security-groups-overview.md#network-security-groups)
- Una [dirección IP pública](../virtual-network/public-ip-addresses.md)
- Un [equilibrador de carga](../load-balancer/load-balancer-overview.md)

Estos recursos se asignan en la suscripción que contiene la red virtual proporcionada al crear el grupo de Batch. Estos recursos están limitados por las [cuotas de recursos](../azure-resource-manager/management/azure-subscription-service-limits.md) de la suscripción. Si tiene previsto realizar grandes implementaciones de grupos en una red virtual, compruebe las cuotas de suscripción para estos recursos. Si es necesario, solicite un aumento en Azure Portal seleccionando **Ayuda y soporte técnico**.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una cuenta de Azure Batch con Azure Portal](batch-account-create-portal.md)
* Conozca el [flujo de trabajo y los recursos principales del servicio Batch](batch-service-workflow-features.md), como grupos, nodos, trabajos y tareas.
* Conozca los [límites, las cuotas y las restricciones de suscripción y servicios de Azure](../azure-resource-manager/management/azure-subscription-service-limits.md).

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png