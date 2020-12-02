---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 95422ace63c9f79583de5a2c86d777ad4d0a8834
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027676"
---
Las entradas del documento `deploymentconfig.json` se asignan a los parámetros de [AksWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | Descripción |
| ----- | ----- | ----- |
| `computeType` | N/D | El destino de proceso. Para AKS, el valor debe ser `aks`. |
| `autoScaler` | N/D | Contiene elementos de configuración para la escalabilidad automática. Consulte la tabla del escalador automático. |
| &emsp;&emsp;`autoscaleEnabled` | `autoscale_enabled` | Indica si se debe habilitar la escalabilidad automática para el servicio web. Si `numReplicas` = `0`, `True`; de lo contrario, `False`. |
| &emsp;&emsp;`minReplicas` | `autoscale_min_replicas` | Número mínimo de contenedores que se van a usar al escalar automáticamente este servicio web. El valor predeterminado es `1`. |
| &emsp;&emsp;`maxReplicas` | `autoscale_max_replicas` | Número máximo de contenedores que se van a usar al escalar automáticamente este servicio web. El valor predeterminado es `10`. |
| &emsp;&emsp;`refreshPeriodInSeconds` | `autoscale_refresh_seconds` | Frecuencia con la que el escalador automático intenta escalar este servicio web. El valor predeterminado es `1`. |
| &emsp;&emsp;`targetUtilization` | `autoscale_target_utilization` | Uso objetivo (en un porcentaje de 100) que el escalador automático debe intentar mantener para este servicio web. El valor predeterminado es `70`. |
| `dataCollection` | N/D | Contiene elementos de configuración para la colección de datos. |
| &emsp;&emsp;`storageEnabled` | `collect_model_data` | Indica si se debe habilitar la recopilación de datos del modelo para el servicio web. El valor predeterminado es `False`. |
| `authEnabled` | `auth_enabled` | Indica si se debe habilitar o no la autenticación mediante clave para el servicio web. `tokenAuthEnabled` y `authEnabled` no pueden ser `True`. El valor predeterminado es `True`. |
| `tokenAuthEnabled` | `token_auth_enabled` | Indica si se debe habilitar o no la autenticación por tokens para el servicio web. `tokenAuthEnabled` y `authEnabled` no pueden ser `True`. El valor predeterminado es `False`. |
| `containerResourceRequirements` | N/D | Contenedor de las entidades de CPU y memoria. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Número de núcleos de CPU que se asigna a este servicio web. El valor predeterminado es `0.1`. |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Cantidad de memoria (en GB) que se va a asignar a este servicio web. El valor predeterminado es `0.5`. |
| `appInsightsEnabled` | `enable_app_insights` | Indica si se debe habilitar el registro de Application Insights para el servicio web. El valor predeterminado es `False`. |
| `scoringTimeoutMs` | `scoring_timeout_ms` | Tiempo de espera que se aplicará a las llamadas de puntuación al servicio web. El valor predeterminado es `60000`. |
| `maxConcurrentRequestsPerContainer` | `replica_max_concurrent_requests` | Número máximo de solicitudes simultáneas por nodo para este servicio web. El valor predeterminado es `1`. |
| `maxQueueWaitMs` | `max_request_wait_time` | Tiempo máximo que permanecerá en la cola una solicitud (en milisegundos) antes de que se devuelva un error 503. El valor predeterminado es `500`. |
| `numReplicas` | `num_replicas` | Número de contenedores que se asignarán a este servicio web. Ningún valor predeterminado. Si no se establece este parámetro, el escalador automático se habilita de forma predeterminada. |
| `keys` | N/D | Contiene elementos de configuración para las claves. |
| &emsp;&emsp;`primaryKey` | `primary_key` | Clave de autenticación principal que se usará para este servicio web |
| &emsp;&emsp;`secondaryKey` | `secondary_key` | Clave de autenticación secundaria que se usará para este servicio web |
| `gpuCores` | `gpu_cores` | El número de núcleos de GPU (por réplica de contenedor) que se van a asignar a este servicio web. El valor predeterminado es 1. Solo admite valores de números enteros. |
| `livenessProbeRequirements` | N/D | Contiene elementos de configuración para los requisitos del sondeo de ejecución. |
| &emsp;&emsp;`periodSeconds` | `period_seconds` | Frecuencia (en segundos) en que se ejecutará el sondeo de ejecución. El valor predeterminado es de 10 segundos. El valor mínimo es 1. |
| &emsp;&emsp;`initialDelaySeconds` | `initial_delay_seconds` | Número de segundos después de que se haya iniciado el contenedor antes de que se inicien los sondeos de ejecución. El valor predeterminado es 310 |
| &emsp;&emsp;`timeoutSeconds` | `timeout_seconds` | Número de segundos tras los cuales el sondeo de ejecución agota el tiempo de espera. El valor predeterminado es de 2 segundos. El valor mínimo es 1 |
| &emsp;&emsp;`successThreshold` | `success_threshold` | Número mínimo de valores correctos consecutivos para que el sondeo de ejecución se considere correcto después de que se haya producido un error. De manera predeterminada, su valor es 1. El valor mínimo es 1. |
| &emsp;&emsp;`failureThreshold` | `failure_threshold` | Cuando se inicie un pod y se produzca un error en el sondeo de ejecución, Kubernetes probará las veces especificadas en failureThreshold antes de abandonarlo. El valor predeterminado es 3. El valor mínimo es 1. |
| `namespace` | `namespace` | Espacio de nombres de Kubernetes en el que está implementado el servicio web. Hasta 63 caracteres alfanuméricos en minúsculas (de la "a" a la "z" y de "0" a "9") y guiones ("-"). El primer y el último carácter no pueden ser un guion. |

El siguiente elemento JSON es un ejemplo de la configuración de implementación que se puede usar con la CLI:

```json
{
    "computeType": "aks",
    "autoScaler":
    {
        "autoscaleEnabled": true,
        "minReplicas": 1,
        "maxReplicas": 3,
        "refreshPeriodInSeconds": 1,
        "targetUtilization": 70
    },
    "dataCollection":
    {
        "storageEnabled": true
    },
    "authEnabled": true,
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    }
}
```