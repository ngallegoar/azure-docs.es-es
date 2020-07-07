---
title: Escalabilidad automática de pods de AKS con métricas de Azure Application Gateway
description: En este artículo se proporcionan instrucciones sobre cómo escalar los pods de back-end de AKS mediante métricas de Application Gateway y el adaptador de métricas de Azure Kubernetes.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 5e0533a44db269229b2f26fa8d2f2b4f84f4d0b4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85125470"
---
# <a name="autoscale-your-aks-pods-using-application-gateway-metrics-beta"></a>Escalado automático de los pods de AKS mediante métricas de Application Gateway (beta)

A medida que aumenta el tráfico entrante, es fundamental escalar verticalmente las aplicaciones en función de la demanda.

En el siguiente tutorial, se explica cómo puede usar la métrica `AvgRequestCountPerHealthyHost` de Application Gateway para escalar verticalmente la aplicación. `AvgRequestCountPerHealthyHost` mide el promedio de solicitudes enviadas a un grupo de back-end específico y la combinación de configuración HTTP de back-end.

Vamos a usar los dos componentes siguientes:

* [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter): usaremos el adaptador de métricas para exponer las métricas de Application Gateway a través del servidor de métricas. El adaptador de métricas de Azure Kubernetes es un proyecto de código abierto en Azure, similar al controlador de entrada de Application Gateway. 
* [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler): usaremos HPA para usar las métricas de Application Gateway y seleccionar como destino una implementación para el escalado.

## <a name="setting-up-azure-kubernetes-metric-adapter"></a>Configuración del adaptador de métricas de Azure Kubernetes

1. En primer lugar, crearemos una entidad de servicio de Azure AAD y le asignaremos el acceso `Monitoring Reader` a través del grupo de recursos de Application Gateway. 

    ```azurecli
        applicationGatewayGroupName="<application-gateway-group-id>"
        applicationGatewayGroupId=$(az group show -g $applicationGatewayGroupName -o tsv --query "id")
        az ad sp create-for-rbac -n "azure-k8s-metric-adapter-sp" --role "Monitoring Reader" --scopes applicationGatewayGroupId
    ```

1. Ahora, implementaremos el [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter) mediante la entidad de servicio de AAD creada anteriormente.

    ```bash
    kubectl create namespace custom-metrics
    # use values from service principal created above to create secret
    kubectl create secret generic azure-k8s-metrics-adapter -n custom-metrics \
        --from-literal=azure-tenant-id=<tenantid> \
        --from-literal=azure-client-id=<clientid> \
        --from-literal=azure-client-secret=<secret>
    kubectl apply -f kubectl apply -f https://raw.githubusercontent.com/Azure/azure-k8s-metrics-adapter/master/deploy/adapter.yaml -n custom-metrics
    ```

1. Se creará un recurso `ExternalMetric` con el nombre `appgw-request-count-metric`. Este recurso indicará al adaptador de métricas que exponga la métrica `AvgRequestCountPerHealthyHost` del recurso `myApplicationGateway` en el grupo de recursos `myResourceGroup`. Puede usar el campo `filter` para establecer como destino un grupo de back-end específico y la configuración HTTP de back-end en Application Gateway.

    ```yaml
    apiVersion: azure.com/v1alpha2
    kind: ExternalMetric
    metadata:
    name: appgw-request-count-metric
    spec:
        type: azuremonitor
        azure:
            resourceGroup: myResourceGroup # replace with your application gateway's resource group name
            resourceName: myApplicationGateway # replace with your application gateway's name
            resourceProviderNamespace: Microsoft.Network
            resourceType: applicationGateways
        metric:
            metricName: AvgRequestCountPerHealthyHost
            aggregation: Average
            filter: BackendSettingsPool eq '<backend-pool-name>~<backend-http-setting-name>' # optional
    ```

Ahora puede hacer una solicitud al servidor de métricas para ver si la nueva métrica se está exponiendo:
```bash
kubectl get --raw "/apis/external.metrics.k8s.io/v1beta1/namespaces/default/appgw-request-count-metric"
# Sample Output
# {
#   "kind": "ExternalMetricValueList",
#   "apiVersion": "external.metrics.k8s.io/v1beta1",
#   "metadata":
#     {
#       "selfLink": "/apis/external.metrics.k8s.io/v1beta1/namespaces/default/appgw-request-count-metric",
#     },
#   "items":
#     [
#       {
#         "metricName": "appgw-request-count-metric",
#         "metricLabels": null,
#         "timestamp": "2019-11-05T00:18:51Z",
#         "value": "30",
#       },
#     ],
# }
```

## <a name="using-the-new-metric-to-scale-up-the-deployment"></a>Uso de la nueva métrica para escalar verticalmente la implementación

Una vez que podamos exponer `appgw-request-count-metric` a través del servidor de métricas, podremos usar [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler) para escalar verticalmente nuestra implementación de destino.

En el ejemplo siguiente, se destinará una implementación de ejemplo `aspnet`. Escalaremos verticalmente los pods cuando `appgw-request-count-metric` sea mayor que 200 por pod hasta un máximo de `10` pods.

Reemplace el nombre de la implementación de destino y aplique la siguiente configuración de escalado automático:
```yaml
apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: deployment-scaler
spec:
  scaleTargetRef:
    apiVersion: extensions/v1beta1
    kind: Deployment
    name: aspnet # replace with your deployment's name
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: External
    external:
      metricName: appgw-request-count-metric
      targetAverageValue: 200
```

Pruebe la configuración mediante una herramienta de prueba de carga como apache bench:
```bash
ab -n10000 http://<applicaiton-gateway-ip-address>/
```

## <a name="next-steps"></a>Pasos siguientes
- [**Cómo solucionar problemas del controlador de entrada**](ingress-controller-troubleshoot.md): Solucione cualquier problema con el controlador de entrada.