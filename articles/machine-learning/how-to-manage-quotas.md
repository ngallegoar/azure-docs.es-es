---
title: Administración de recursos y cuotas
titleSuffix: Azure Machine Learning
description: Obtenga información sobre las cuotas de los recursos de Azure Machine Learning y cómo solicitar más cuota.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: jmartens
author: nishankgu
ms.author: nigup
ms.date: 10/13/2020
ms.topic: conceptual
ms.custom: troubleshooting,contperfq4, contperfq2
ms.openlocfilehash: a81af14992c8557c245ab3a1073f031a6c505084
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019399"
---
# <a name="manage--increase-quotas-for-resources-with-azure-machine-learning"></a>Administración y aumento de las cuotas de los recursos con Azure Machine Learning

Azure utiliza límites y cuotas para evitar salirse del presupuesto debido a fraudes y para respetar las restricciones de capacidad de Azure. Tenga en cuenta estos límites a medida que escala para cargas de trabajo de producción. En este artículo, aprenderá lo siguiente:

> [!div class="checklist"]
> + Límites predeterminados de los recursos de Azure relacionados con [Azure Machine Learning](overview-what-is-azure-ml.md).
> + Consulte sus cuotas y límites.
> + Cree cuotas de nivel de área de trabajo.
> + Solicite aumentos de cuota.
> + Puntos de conexión privados y cuotas de DNS.

Además de administrar cuotas, también puede aprender a [planear y administrar los costos de Azure Machine Learning](concept-plan-manage-cost.md).

## <a name="special-considerations"></a>Consideraciones especiales

+ Una cuota es un límite de crédito, no una garantía de capacidad. Si tiene necesidades de capacidad a gran escala, [póngase en contacto con el soporte técnico de Azure para incrementar la cuota](#request-quota-increases).

+ La cuota se comparte en todos los servicios de las suscripciones, incluso en Azure Machine Learning. Calcule el uso en todos los servicios al evaluar la capacidad.
    + El Proceso de Azure Machine Learning es una excepción, ya que tiene una cuota independiente de la cuota de procesos principales. 

+ Los límites predeterminados varían según el tipo de categoría de la oferta, por ejemplo, evaluación gratuita, pago por uso y series de máquina virtual (VM), como Dv2, F, G, etc.

## <a name="default-resource-quotas"></a>Cuotas de recursos predeterminadas

En esta sección, obtendrá información acerca de los límites de cuota predeterminados y máximos para los siguientes recursos:

+ Máquinas virtuales
+ Proceso de Azure Machine Learning
+ Canalizaciones de Azure Machine Learning
+ Container Instances
+ Almacenamiento

> [!IMPORTANT]
> Los límites están sujetos a cambios. El más reciente siempre se puede encontrar en el [documento](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits/) de la cuota de nivel de servicio para todo Azure.

### <a name="virtual-machines"></a>Máquinas virtuales
Cada suscripción de Azure tiene un límite en el número de máquinas virtuales que pueden tenerse entre todos los servicios. Los núcleos de las máquinas virtuales tienen un límite regional total y un límite de serie por tamaño (Dv2, F, etc.). Ambos límites se aplican por separado.

Por ejemplo, considere una suscripción con un límite total de núcleos de máquinas virtuales de Este de EE. UU. de 30, un límite de núcleos de serie A de 30 y un límite de núcleos de serie D de 30. Esta suscripción podría volver a implementar 30 máquinas virtuales A1, 30 máquinas virtuales D1 o una combinación de las dos que no superen los 30 núcleos.

Los límites de las máquinas virtuales no pueden generarse por encima del valor que se muestra en la siguiente tabla.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="azure-machine-learning-compute"></a>Proceso de Azure Machine Learning
El [proceso de Azure Machine Learning](concept-compute-target.md#azure-machine-learning-compute-managed) tiene un límite de cuota predeterminado tanto sobre el número de núcleos como el número de recursos de proceso único permitido por región en una suscripción. Esta cuota es independiente de la cuota de núcleos de máquinas virtuales de la sección anterior.

[Solicite un aumento de la cuota](#request-quota-increases) para aumentar los límites de esta sección hasta el **límite máximo** que se muestra en la tabla.

Recursos disponibles:
+ Los **núcleos dedicados por región** tienen un límite predeterminado de 24 a 300 según el tipo de oferta de la suscripción.  El número de núcleos dedicados por suscripción se puede aumentar para cada familia de máquinas virtuales. Las familias de máquinas virtuales especializadas, como las series NCv2, NCv3 o ND, comienzan con un valor predeterminado de cero núcleos.

+ Los **núcleos de baja prioridad por región** tienen un límite predeterminado de 100 a 3000, según el tipo de oferta de la suscripción. El número de núcleos de baja prioridad por suscripción se puede aumentar y es un valor único en todas las familias de máquinas virtuales.

+ Los **clústeres por región** tienen un límite predeterminado de 200. Se comparten entre un clúster de entrenamiento y una instancia de proceso (que se considera un clúster de un solo nodo con fines de cuota).

En la siguiente tabla se muestran los límites adicionales que no pueden superarse.

| **Recurso** | **Límite máximo** |
| --- | --- |
| Número de áreas de trabajo por grupo de recursos | 800 |
| Número de nodos en un solo recurso de Proceso de Azure Machine Learning (AmlCompute) | 100 nodos |
| Número de procesos MPI de GPU por nodo | 1-4 |
| Número de trabajos de GPU por nodo | 1-4 |
| Vigencia del trabajo | 21 días<sup>1</sup> |
| Duración de un trabajo en un nodo de prioridad baja | 7 días<sup>2</sup> |
| Número de servidores de parámetro por nodo | 1 |

<sup>1</sup> La duración máxima se refiere a la duración entre el inicio y la finalización de una ejecución. Las ejecuciones completadas se mantienen de forma indefinida. Los datos de las ejecuciones no completadas dentro de la duración máxima no son accesibles.
<sup>2</sup> Los trabajos en un nodo de prioridad baja pueden anularse en el momento que exista una restricción de capacidad. Le recomendamos que implemente los puntos de comprobación en el trabajo.

### <a name="azure-machine-learning-pipelines"></a>Canalizaciones de Azure Machine Learning
Las [canalizaciones de Azure Machine Learning](concept-ml-pipelines.md) tienen los siguientes límites.

| **Recurso** | **Límite** |
| --- | --- |
| Pasos en una canalización | 30,000 |
| Número de áreas de trabajo por grupo de recursos | 800 |

### <a name="container-instances"></a>Instancias de contenedor

Para más información, consulte [Límites de Container Instances](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#container-instances-limits).

### <a name="storage"></a>Almacenamiento
Las cuentas de almacenamiento de Azure tienen un límite de 250 cuentas de almacenamiento por región y suscripción. Esto incluye las cuentas de almacenamiento Estándar y Premium.

En el caso de que quisiera aumentar el límite, envíe una solicitud a través del [Soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). El equipo de Azure Storage revisará su caso y podría aprobar hasta 250 cuentas de almacenamiento por región.


## <a name="workspace-level-quota"></a>Cuota de nivel de área de trabajo

Utilice las cuotas de nivel de área de trabajo para administrar la asignación de destino de proceso de Azure Machine Learning entre varias [áreas de trabajo](concept-workspace.md) en la misma suscripción.

De manera predeterminada, todas las áreas de trabajo tienen la misma cuota que la cuota de nivel de suscripción de familias de máquinas virtuales. Sin embargo, puede establecer una cuota máxima para familias de máquinas virtuales individuales en áreas de trabajo de una suscripción. Esto le permite compartir la capacidad y evitar problemas de contención de recursos:

1. Vaya a cualquier área de trabajo de la suscripción.
1. En el panel izquierdo, seleccione **Uso y cuotas**.
1. Seleccione la pestaña **Configurar cuotas** para ver las cuotas.
1. Expanda una familia de máquinas virtuales.
1. Establezca un límite de cuota en cualquier área de trabajo que aparezca en esa familia de máquinas virtuales.

No puede establecer un valor negativo o un valor mayor que la cuota de nivel de suscripción.

[![Cuota de nivel de área de trabajo de Azure Machine Learning](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)

> [!NOTE]
> Necesita permisos de nivel de suscripción para establecer la cuota en el nivel de área de trabajo.

## <a name="view-your-usage-and-quotas"></a>Visualización del uso y las cuotas

Para ver la cuota de varios recursos de Azure Portal, como las máquinas virtuales, el almacenamiento y la red, utiliza Azure Portal:

1. En el panel izquierdo, seleccione **Todos los servicios** y, después, seleccione **Suscripciones** en la categoría General.

2. En la lista de suscripciones, seleccione la suscripción cuya cuota busca.

3. Seleccione **Uso y cuotas** para ver los límites y el uso actuales de la cuota. Use los filtros para seleccionar el proveedor y las ubicaciones. 

La cuota de Proceso de Azure Machine Learning de la suscripción se administra de forma independiente de otras cuotas de Azure. 

1. En **Azure Portal**, vaya al área de trabajo de Azure Machine Learning.

2. En el panel izquierdo, en la **sección Soporte técnico y solución de problemas**, seleccione **Uso y cuotas** para ver el uso y los límites de cuota actuales.

3. Seleccione una suscripción para ver los límites de cuota. No olvide filtrar por la región en que esté interesado.

4. Puede alternar entre una vista de nivel de suscripción y una vista de nivel de área de trabajo.

## <a name="request-quota-increases"></a>Solicitud de aumento de cuota

Para aumentar el límite o la cuota por encima del límite predeterminado, [abra una solicitud de soporte técnico al cliente en línea sin cargo alguno](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/).

Los límites no se pueden subir por encima del valor del límite máximo que se muestra en las siguientes tablas. Si no hay ningún límite máximo, no puede ajustar el límite del recurso.

Al solicitar un aumento de cuota, seleccione el servicio al que solicita elevar la cuota. Por ejemplo, Azure Machine Learning, Container Instances, Storage, etc. En el caso de Proceso de Azure Machine Learning, puede hacer clic en el botón **Solicitar cuota** al visualizar la cuota según los pasos anteriores.

> [!NOTE]
> Las [suscripciones de evaluación gratuita](https://azure.microsoft.com/offers/ms-azr-0044p) no son aptas para aumentar el límite ni la cuota. Si tiene una [suscripción de evaluación gratuita](https://azure.microsoft.com/offers/ms-azr-0044p), puede actualizar a una suscripción de [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/). Para obtener más información, consulte [Actualización de evaluación gratuita de Azure a pago por uso](../billing/billing-upgrade-azure-subscription.md) y [Free Trial subscription FAQ](https://azure.microsoft.com/free/free-account-faq) (Preguntas más frecuentes de la suscripción de evaluación gratuita).

## <a name="private-endpoint-and-private-dns-quota-increases"></a>El punto de conexión privado y la cuota de DNS privada aumentan

Existen limitaciones en cuanto al número de puntos de conexión privados y zonas DNS privadas que pueden crearse en una suscripción.

Aunque Azure Machine Learning crea recursos en su suscripción (cliente), existen algunas situaciones en las que se crean recursos en una suscripción propiedad de Microsoft.

 En los siguientes casos, puede que necesite solicitar una asignación de cuota en la suscripción propiedad de Microsoft:

* __Área de trabajo habilitada para Private Link con una clave administrada por el cliente (CMK)__
* __Azure Container Registry para el área de trabajo detrás de la red virtual__
* __Adjunte un clúster de Azure Kubernetes Service habilitado por un vínculo privado en el área de trabajo__.

Para solicitar una asignación para dichos casos, siga estos pasos:

1. [Cree una solicitud de soporte técnico de Azure](/azure/azure-portal/supportability/how-to-create-azure-support-request#create-a-support-request) y seleccione las siguientes opciones en la sección __Básicos__:

    | Campo | Número de selección |
    | ----- | ----- |
    | Tipo de problema | Técnicos |
    | Servicio | Mis servicios. Seleccione __Machine Learning__ en la lista desplegable. |
    | Tipo de problema | Configuración del área de trabajo, SDK y CLI |
    | Subtipo de problema | Problemas al aprovisionar o administrar las áreas de trabajo |

2. En la sección __Detalles__ y utilice el campo __Descripción__ para proporcionar la región de Azure que quiere utilizar y el escenario que planea utilizar. Si necesita solicitar aumentos de cuota para varias suscripciones, enumere también los id. de suscripción de este campo.

3. Para crear la solicitud, seleccione __Crear__.

## <a name="next-steps"></a>Pasos siguientes

+ [Planeamiento y administración de los costos de Azure Machine Learning](concept-plan-manage-cost.md)
