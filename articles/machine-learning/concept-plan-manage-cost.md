---
title: Planificación y administración de costos
titleSuffix: Azure Machine Learning
description: Planeación y administración de los costos de Azure Machine Learning con análisis de costos en Azure Portal. Al compilar modelos de aprendizaje automático, aprenda más sugerencias de ahorro de costos para reducir su gasto.
author: sdgilley
ms.author: sgilley
ms.custom: subject-cost-optimization
ms.reviewer: nigup
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: 67b703f0079e26d01330d52d170f99699480fad6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195779"
---
# <a name="plan-and-manage-costs-for-azure-machine-learning"></a>Planeamiento y administración de los costos de Azure Machine Learning

En este artículo se describe cómo puede planear y administrar los costos de Azure Machine Learning. En primer lugar, use la calculadora de precios de Azure para ayudar a planear los costos antes de agregar recursos. Después, a medida que agregue los recursos de Azure, revise los costos estimados. Por último, use sugerencias de ahorro de costos a medida que entrena el modelo con clústeres de proceso de Azure Machine Learning administrados.

Después de comenzar a usar los recursos de Azure Machine Learning, use las características de administración de costos para establecer presupuestos y supervisar los costos. Revise los costos previstos e identifique las tendencias de gasto para determinar las áreas interesantes para actuar.

Tenga en cuenta que los costos de Azure Machine Learning son solo una parte de los costos mensuales de la factura de Azure. Si usa otros servicios de Azure, se le facturarán todos los servicios y recursos de Azure usados en la suscripción de Azure, incluidos los servicios de terceros. En este artículo se explica cómo planear y administrar los costos de Azure Machine Learning. Una vez que esté familiarizado con la administración de los costos de Azure Machine Learning, puede aplicar métodos similares para administrar los costos de todos los servicios de Azure que se usan en la suscripción.

Al entrenar los modelos de aprendizaje automático, use clústeres de proceso de Azure Machine Learning administrados para aprovechar las ventajas de las sugerencias para el ahorro de costos:

* Configurar los clústeres de entrenamiento para el escalado automático
* Establecer cuotas en las áreas de trabajo y la suscripción
* Establecer directivas de finalización en la ejecución del entrenamiento
* Usar máquinas virtuales de prioridad baja
* Usar una instancia de Azure Reserved Virtual Machine Instances

## <a name="prerequisites"></a>Prerrequisitos

El análisis de costos es compatible con varios tipos de cuenta de Azure. Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](../cost-management-billing/costs/understand-cost-mgt-data.md) (Información sobre los datos de Cost Management). Para ver los datos de costos, se necesita al menos acceso de lectura en la cuenta de Azure. 

Para más información acerca de cómo asignar acceso a los datos de Azure Cost Management, consulte [Asignación de acceso a los datos](../cost-management-billing/costs/assign-access-acm-data.md).

## <a name="estimate-costs"></a>cálculo de los costos

Use la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/) para calcular los costos antes de crear los recursos en una cuenta de Azure Machine Learning. A la izquierda, seleccione **IA y Machine Learning** y, a continuación, seleccione **Azure Machine Learning** para comenzar.  

En la captura de pantalla siguiente se muestra la estimación de costos mediante la calculadora:

:::image type="content" source="media/concept-plan-manage-cost/capacity-calculator-cost-estimate.png" alt-text="Estimación de costos en la calculadora de Azure":::

A medida que agregue nuevos recursos al área de trabajo, vuelva a esta calculadora y agregue el mismo recurso aquí para actualizar las estimaciones de costos.

Aunque la edición Enterprise está en versión preliminar, no hay ningún suplemento por el aprendizaje automático. Cuando la edición Enterprise esté disponible con carácter general, tendrá un suplemento por el aprendizaje automático (para entrenamiento e inferencia).  Para obtener más información, consulte [Precios de Azure Machine Learning](https://azure.microsoft.com/pricing/details/machine-learning/).

## <a name="get-cost-alerts"></a>Obtención de alertas sobre los costos

Cree [presupuestos](../cost-management/tutorial-acm-create-budgets.md) para administrar los costos y cree [alertas](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales. Las alertas se basan en el gasto comparado con los umbrales de presupuesto y costo. Los presupuestos y las alertas se crean para las suscripciones y los grupos de recursos de Azure, por lo que son útiles como parte de una estrategia general de supervisión de costos. Sin embargo, los presupuestos y las alertas pueden tener una funcionalidad limitada para administrar los costos de servicios individuales de Azure porque están diseñados para realizar un seguimiento de los costos en un nivel más alto.

## <a name="monitor-costs"></a>Supervisión de costos

A medida que se usan recursos con Azure Machine Learning, se incurre en costos. Los costos de la unidad de uso de recursos de Azure varían en función de intervalos de tiempo (segundos, minutos, horas y días) o en función del uso de unidades de solicitud. Tan pronto como empieza el uso de Azure Machine Learning, se incurre en costos. Vea estos costos en el panel [Análisis de costos](../cost-management/quick-acm-cost-analysis.md) de Azure Portal.

Vea los costos en gráficos y tablas para diferentes intervalos de tiempo. Algunos ejemplos son: por día, actual, mes anterior y año. Vea también los costos comparados con los presupuestos y los costos previstos. Cambiar a vistas más largas en el tiempo ayuda a identificar las tendencias de gasto y ver dónde podría haber ocurrido un gasto excesivo. Si ha creado presupuestos, consulte dónde se superaron.  

No verá un área de servicio independiente para Machine Learning.  En su lugar, verá los distintos recursos que ha agregado a las áreas de trabajo de Machine Learning.

## <a name="use-amlcompute"></a>Uso de AmlCompute

Como los datos cambian constantemente, necesita entrenar y volver a entrenar modelos rápidos y simplificados para mantener su precisión. Sin embargo, el entrenamiento continuo conlleva un costo, especialmente en el caso de modelos de aprendizaje profundo en GPU. 

Los usuarios de Azure Machine Learning pueden usar el clúster de proceso de Azure Machine Learning administrado, también denominado AmlCompute. AmlCompute admite una variedad de opciones de GPU y CPU. La instancia de AmlCompute se hospeda internamente en nombre de su suscripción por Azure Machine Learning, pero proporciona la misma seguridad de nivel empresarial, cumplimiento y gobernanza en la escala de nube de IaaS de Azure.

Dado que estos grupos de proceso se encuentran dentro de la infraestructura de IaaS de Azure, puede implementar, escalar y administrar el entrenamiento con los mismos requisitos de seguridad y cumplimiento que el resto de la infraestructura.  Estas implementaciones se producen en su suscripción y obedecen sus reglas de gobernanza. Más información sobre [Proceso de Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute).

## <a name="configure-training-clusters-for-autoscaling"></a>Configuración de clústeres de entrenamiento para el escalado automático

El escalado automático de clústeres en función de los requisitos de la carga de trabajo ayuda a reducir los costos de manera que solo use lo que necesita. Los clústeres de AmlCompute están diseñados para escalarse automáticamente de forma dinámica en función de los requisitos de la carga de trabajo. El clúster se puede escalar hasta el número máximo de nodos aprovisionados y dentro de la cuota designada para la suscripción. A medida que se complete cada ejecución, el clúster liberará los nodos y se escalará automáticamente al número mínimo de nodos designado.

Además de establecer el número mínimo y máximo de nodos, ajuste la cantidad de tiempo que el nodo está inactivo antes de la reducción vertical. De forma predeterminada, el tiempo de inactividad antes de la reducción vertical se establece en 120 segundos.

+ Si realiza una experimentación menos iterativa, reduzca este tiempo para ahorrar costos. 
+ Si realiza experimentación de desarrollo y pruebas de gran iteración, es posible que tenga que aumentarlo para no pagar por el escalado y la reducción vertical constante después de cada cambio en el entorno o el script de entrenamiento.

Los clústeres de AmlCompute se pueden configurar para los cambios en los requisitos de carga de trabajo de Azure Portal, mediante la [clase SDK de AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py), [la CLI de AmlCompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute), con las [API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/machinelearningservices/resource-manager/Microsoft.MachineLearningServices/stable).

```azure cli
az ml computetarget create amlcompute --name testcluster --vm-size Standard_NC6 --min-nodes 0 --max-nodes 5 --idle-seconds-before-scaledown 300
```

## <a name="set-quotas-on-resources"></a>Establecimiento de cuotas en recursos

Al igual que otros recursos de proceso de Azure, AmlCompute incluye una [configuración de cuota (o límite)](how-to-manage-quotas.md#azure-machine-learning-compute). Esta cuota se establece para cada familia de máquinas virtuales (por ejemplo, serie Dv2, serie NCv3) y varía en función de la región de cada suscripción. Las suscripciones se inician con valores predeterminados pequeños para empezar a trabajar, pero esta opción le permite controlar la cantidad de recursos de Amlcompute disponibles para poner en marcha en su suscripción. 

Configure también la [cuota de nivel de área de trabajo por familia de máquinas virtuales](/how-to-manage-quotas.md#workspace-level-quota) para cada área de trabajo de una suscripción. Esto le permite tener un control más detallado sobre los costos que cada área de trabajo podría provocar potencialmente y restringir determinadas familias de máquinas virtuales. 

Para establecer cuotas en el nivel de área de trabajo, empiece en [Azure Portal](https://portal.azure.com).  Seleccione cualquier área de trabajo de la suscripción y seleccione **Usos y cuotas** en el panel izquierdo. A continuación, seleccione la pestaña **Configurar cuotas** para ver las cuotas. Necesita privilegios en el ámbito de la suscripción para establecer esta cuota, ya que se trata de una configuración que afecta a varias áreas de trabajo.

## <a name="set-run-auto-termination-policies"></a>Establecimiento de directivas de finalización automática de ejecución 

Configure las ejecuciones de entrenamiento para limitar su duración o para terminarlas con antelación de producirse ciertas condiciones, en especial cuando se usa el ajuste de hiperparámetros integrado de Azure Machine Learning o las capacidades de aprendizaje automático automatizado. 

Estas son algunas de las opciones que tiene:
* Defina un parámetro llamado `max_run_duration_seconds` en RunConfiguration para controlar la duración máxima a la que se puede extender una ejecución en el proceso que elija (proceso local o en la nube remota).
* Para el [ajuste de hiperparámetros](how-to-tune-hyperparameters.md#early-termination), defina una directiva de finalización temprana de una directiva de bandidos, una directiva de mediana de detención o una directiva de selección de truncamiento. Además, también puede usar parámetros como `max_total_runs` o `max_duration_minutes` para controlar aún más los distintos barridos de hiperparámetros.
* En el caso del [aprendizaje automático automatizado](how-to-configure-auto-train.md#exit), establezca directivas de terminación similares mediante la marca `enable_early_stopping`. Use también propiedades como `iteration_timeout_minutes` y `experiment_timeout_minutes` para controlar la duración máxima de una ejecución o para todo el experimento.

## <a name="use-low-priority-vms"></a>Uso de máquinas virtuales de prioridad baja

Azure le permite usar un exceso de capacidad no utilizada como máquinas virtuales de prioridad baja en conjuntos de escalado de máquinas virtuales, Batch y el servicio de Machine Learning. Estas asignaciones son reemplazables pero tienen un precio reducido en comparación con las máquinas virtuales dedicadas. En general, se recomienda usar máquinas virtuales de prioridad baja para cargas de trabajo de Batch o en las que las interrupciones se puedan recuperar a través de reenvíos (para la inferencia de por lotes) o a través de reinicios (para un entrenamiento de aprendizaje profundo con puntos de comprobación).

Las máquinas virtuales de prioridad baja tienen una cuota única independiente del valor de cuota dedicada, que es por familia de máquinas virtuales. Obtenga [más información sobre las cuotas de AmlCompute](how-to-manage-quotas.md).

Establezca la prioridad de la máquina virtual de cualquiera de estas maneras:

* En Studio, elija **Prioridad baja** al crear una máquina virtual.

* Con el SDK de Python, establezca el atributo `vm_priority` en la configuración de aprovisionamiento.  

    ```python
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                               vm_priority='lowpriority',
                                                               max_nodes=4)
    ```

* Con la CLI, establezca `vm-priority`:

    ```azurecli-interactive
    az ml computetarget create amlcompute --name lowpriocluster --vm-size Standard_NC6 --max-nodes 5 --vm-priority lowpriority
    ```

 Las máquinas virtuales de prioridad baja no funcionan para las instancias de proceso, ya que necesitan admitir experiencias interactivas de cuaderno. 

## <a name="use-reserved-instances"></a>Uso de instancias reservadas

Una instancia de Azure Reserved Virtual Machine Instances proporciona otra manera de ahorrar enormes recursos de proceso mediante el compromiso durante periodos de un año o tres años. Estos descuentos van hasta el 72 % de los precios de pago por uso y se aplican directamente a su factura mensual de Azure.

Proceso de Azure Machine Learning admite las instancias reservadas de forma inherente. Por lo tanto, si ha adquirido una instancia reservada de un año o de tres años, aplicaremos automáticamente ese descuento de instancia reservada en el proceso administrado que se usa en Azure Machine Learning sin requerir ninguna configuración adicional por su parte.


## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre la administración de costos con los [análisis de costos](../cost-management-billing/costs/quick-acm-cost-analysis.md).
* Más información sobre [proceso de Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute).