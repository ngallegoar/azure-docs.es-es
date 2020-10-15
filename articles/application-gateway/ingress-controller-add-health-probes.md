---
title: Adición de sondeos de estado a pods de AKS
description: En este artículo se proporciona información sobre cómo agregar sondeos de estado (preparación o ejecución) a los pods de AKS con una instancia de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 8c8b8b0090877db7abc8fae0e44f928e8b10dcf5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84808007"
---
# <a name="add-health-probes-to-your-service"></a>Adición de sondeos de estado a servicios
De forma predeterminada, el controlador de entrada aprovisionará un sondeo HTTP GET para los pods expuestos.
Las propiedades de sondeo pueden personalizarse agregando un [sondeo de preparación o ejecución](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) a sus especificaciones de `deployment`/`pod`.

## <a name="with-readinessprobe-or-livenessprobe"></a>Con `readinessProbe` o `livenessProbe`
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aspnetapp
spec:
  replicas: 3
  template:
    metadata:
      labels:
        service: site
    spec:
      containers:
      - name: aspnetapp
        image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 3
          timeoutSeconds: 1
```

Referencia de la API de Kubernetes:
* [Sondeo de contenedores](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
* [Acción de HttpGet](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#httpgetaction-v1-core)

> [!NOTE]
> * `readinessProbe` y `livenessProbe` se admiten cuando se configuran con `httpGet`.
> * Actualmente no se admite el sondeo en un puerto distinto al que se expone en el pod.
> * No se admiten `HttpHeaders`, `InitialDelaySeconds`, `SuccessThreshold`.

##  <a name="without-readinessprobe-or-livenessprobe"></a>Sin `readinessProbe` o `livenessProbe`
Si no se proporcionan los sondeos anteriores, el controlador de entrada supondrá que se puede acceder al servicio en la `Path` especificada para la anotación de `backend-path-prefix` o la `path` especificada en la definición de `ingress` del servicio.

## <a name="default-values-for-health-probe"></a>Valores predeterminados para el sondeo de estado
En el caso de las propiedades que no se pueden inferir mediante el sondeo de preparación o ejecución, se establecen los valores predeterminados.

| Propiedad de sondeo de Application Gateway | Valor predeterminado |
|-|-|
| `Path` | / |
| `Host` | localhost |
| `Protocol` | HTTP |
| `Timeout` | 30 |
| `Interval` | 30 |
| `UnhealthyThreshold` | 3 |