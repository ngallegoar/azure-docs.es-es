---
title: Instalación de Istio en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo instalar y usar Istio para crear una malla de servicio en un clúster de Azure Kubernetes Service (AKS).
author: paulbouwer
ms.topic: article
ms.date: 10/02/2020
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: c42e9c31397e9313898d7029366bc8de169d368e
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94683835"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Instalación y uso de Istio en Azure Kubernetes Service (AKS)

[Istio][istio-github] es una malla de servicio de código abierto que proporciona un conjunto clave de funcionalidades en los microservicios de un clúster de Kubernetes. Estas características incluyen la administración del tráfico, la seguridad e identidad de servicio, el cumplimiento de directivas y la observación. Para más información sobre Istio, vea la documentación del sitio oficial [¿Qué es Istio?][istio-docs-concepts].

En este artículo se explica cómo instalar Istio. El binario de cliente `istioctl` de Istio se instala en el equipo cliente y, a continuación, los componentes de Istio se instalan en un clúster de Kubernetes en AKS.

> [!NOTE]
> Las siguientes instrucciones son aplicables a la versión `1.7.3` de Istio.
>
> El equipo de Istio ha probado las versiones de Istio `1.7.x` con la versión de Kubernetes `1.16+`. Encontrará otras versiones de Istio en [GitHub: Istio > Releases][istio-github-releases] (GitHub: Istio > Versiones), información sobre cada una de las versiones en [Istio News][istio-release-notes] (Novedades de Istio) y las versiones compatibles de Kubernetes en [Istio General FAQ][istio-faq] (Istio: Preguntas frecuentes generales).

En este artículo aprenderá a:

> [!div class="checklist"]
> * Descargar e instalar el binario de cliente istioctl de Istio
> * Instalar Istio en AKS
> * Validar la instalación de Istio
> * Acceder a los complementos
> * Desinstalar Istio de AKS

## <a name="before-you-begin"></a>Antes de empezar

En los pasos detallados en este artículo se da por hecho que creó un clúster de AKS (Kubernetes `1.16` y versiones posteriores, con RBAC de Kubernetes habilitado) y que estableció una conexión `kubectl` con el clúster. Si necesita ayuda con cualquiera de estos elementos, consulte el [inicio rápido de AKS][aks-quickstart].

Asegúrese de leer la documentación de [rendimiento y escalabilidad de Istio](https://istio.io/docs/concepts/performance-and-scalability/) para comprender los requisitos de recursos adicionales para ejecutar Istio en el clúster de AKS. Los requisitos de memoria y núcleos varían en función de la carga de trabajo concreta. Elija el número de nodos y el tamaño de la máquina virtual adecuados para la instalación.

En este artículo, las instrucciones de instalación de Istio se dividen en varios pasos discretos. El resultado final presenta la misma estructura que las [instrucciones][istio-install-istioctl] de instalación oficiales de Istio.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/istio/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [macOS - download and install client binary](includes/servicemesh/istio/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/istio/install-client-binary-windows.md)]

::: zone-end

## <a name="install-the-istio-operator-on-aks"></a>Instalación de Istio Operator en AKS

Istio proporciona un [operador][istio-install-operator] para administrar la instalación y las actualizaciones de los componentes de Istio en el clúster de AKS. Instalaremos Istio Operator con el archivo binario `istioctl` del cliente.

```bash
istioctl operator init
```

La imagen en su pantalla debería ser similar a la siguiente salida para confirmar que se ha instalado Istio Operator.

```console
Using operator Deployment image: docker.io/istio/operator:1.7.3
✔ Istio operator installed
✔ Installation complete
```

Istio Operator se instala en el espacio de nombres `istio-operator`. Consulte el espacio de nombres.

```bash
kubectl get all -n istio-operator
```

Debería ver los siguientes componentes implementados.

```console
NAME                                  READY   STATUS    RESTARTS   AGE
pod/istio-operator-6d7958b7bf-wxgdc   1/1     Running   0          2m43s

NAME                     TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
service/istio-operator   ClusterIP   10.0.8.57    <none>        8383/TCP   2m43s

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/istio-operator   1/1     1            1           2m43s

NAME                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/istio-operator-6d7958b7bf   1         1         1       2m43s
```

Puede obtener más información sobre el patrón de Operator y cómo puede ayudarle a automatizar tareas complejas a través de [kubernetes.io][kubernetes-operator].


### <a name="install-istio-components"></a>Instalar componentes de Istio

Ahora que hemos instalado correctamente Istio Operator en nuestro clúster de AKS, es hora de instalar los componentes de Istio. 

Usaremos el [perfil de configuración de Istio][istio-configuration-profiles] `default` para compilar la [especificación de Istio Operator][istio-control-plane].

Puede ejecutar el siguiente comando `istioctl` para ver la configuración del perfil `default` de configuración de Istio.

```bash
istioctl profile dump default
```

> [!NOTE]
> Actualmente, Istio debe programarse para su ejecución en nodos de Linux. Si tiene nodos de Windows Server en el clúster, debe asegurarse de que los pods de Istio solo se programan para ejecutarse en nodos de Linux. Usaremos [selectores de nodo][kubernetes-node-selectors] para garantizar que los pods se programan en los nodos correctos.

> [!CAUTION]
> Las características [CNI][istio-feature-cni] de Istio se encuentran actualmente en [fase alfa][istio-feature-stages], por lo que debe reflexionar antes de habilitarlas. 

Cree un archivo llamado `istio.aks.yaml` con el siguiente contenido. Este archivo contendrá los detalles de [especificación de Istio Operator][istio-control-plane] para la configuración de Istio.

```yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
metadata:
  namespace: istio-system
  name: istio-control-plane
spec:
  # Use the default profile as the base
  # More details at: https://istio.io/docs/setup/additional-setup/config-profiles/
  profile: default
  # Enable the addons that we will want to use
  addonComponents:
    grafana:
      enabled: true
    prometheus:
      enabled: true
    tracing:
      enabled: true
    kiali:
      enabled: true
  values:
    global:
      # Ensure that the Istio pods are only scheduled to run on Linux nodes
      defaultNodeSelector:
        beta.kubernetes.io/os: linux
    kiali:
      dashboard:
        auth:
          strategy: anonymous 
```

Cree el espacio de nombres `istio-system` e implemente la especificación de Istio Operator en dicho espacio de nombres. Istio Operator estará a la espera de la especificación de Istio Operator y la usará para instalar y configurar Istio en el clúster de AKS.

```bash
kubectl create ns istio-system

kubectl apply -f istio.aks.yaml 
```

En este punto, ya tendrá implementado Istio en su clúster de AKS. Para asegurarse de que ha implementado correctamente Istio, vaya a la siguiente sección para [validar la instalación de Istio](#validate-the-istio-installation).

## <a name="validate-the-istio-installation"></a>Validar la instalación de Istio

Consulte el espacio de nombres `istio-system` en el que se instalaron los componentes de Istio y de complemento mediante Istio Operator:

```bash
kubectl get all -n istio-system
```

Debería ver los siguientes componentes:

- `istio*`: los componentes de Istio.
- `jaeger-*`, `tracing` y `zipkin`: complementos de seguimiento.
- `prometheus`: complemento de métricas.
- `grafana`: complementos de panel de supervisión y análisis.
- `kiali`: complemento de panel de malla de servicio.

```console
NAME                                        READY   STATUS    RESTARTS   AGE
pod/grafana-7cf9794c74-mpfbp                1/1     Running   0          5m53s
pod/istio-ingressgateway-86b5dbdcb9-ndrp5   1/1     Running   0          5m57s
pod/istio-tracing-c98f4b8fc-zqklg           1/1     Running   0          82s
pod/istiod-6965c56995-4ph9h                 1/1     Running   0          6m15s
pod/kiali-7b44985d68-p87zh                  1/1     Running   0          81s
pod/prometheus-6868989549-5ghzz             1/1     Running   0          81s

NAME                                TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                                                      AGE
service/grafana                     ClusterIP      10.0.226.39    <none>         3000/TCP                                                     5m54s
service/istio-ingressgateway        LoadBalancer   10.0.143.56    20.53.72.254   15021:32166/TCP,80:31684/TCP,443:31302/TCP,15443:30863/TCP   5m57s
service/istiod                      ClusterIP      10.0.211.228   <none>         15010/TCP,15012/TCP,443/TCP,15014/TCP,853/TCP                6m16s
service/jaeger-agent                ClusterIP      None           <none>         5775/UDP,6831/UDP,6832/UDP                                   82s
service/jaeger-collector            ClusterIP      10.0.7.62      <none>         14267/TCP,14268/TCP,14250/TCP                                82s
service/jaeger-collector-headless   ClusterIP      None           <none>         14250/TCP                                                    82s
service/jaeger-query                ClusterIP      10.0.52.172    <none>         16686/TCP                                                    82s
service/kiali                       ClusterIP      10.0.71.179    <none>         20001/TCP                                                    82s
service/prometheus                  ClusterIP      10.0.171.151   <none>         9090/TCP                                                     82s
service/tracing                     ClusterIP      10.0.195.137   <none>         80/TCP                                                       82s
service/zipkin                      ClusterIP      10.0.136.111   <none>         9411/TCP                                                     82s

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/grafana                1/1     1            1           5m54s
deployment.apps/istio-ingressgateway   1/1     1            1           5m58s
deployment.apps/istio-tracing          1/1     1            1           83s
deployment.apps/istiod                 1/1     1            1           6m16s
deployment.apps/kiali                  1/1     1            1           83s
deployment.apps/prometheus             1/1     1            1           82s

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/grafana-7cf9794c74                1         1         1       5m54s
replicaset.apps/istio-ingressgateway-86b5dbdcb9   1         1         1       5m58s
replicaset.apps/istio-tracing-c98f4b8fc           1         1         1       83s
replicaset.apps/istiod-6965c56995                 1         1         1       6m16s
replicaset.apps/kiali-7b44985d68                  1         1         1       82s
replicaset.apps/prometheus-6868989549             1         1         1       82s

NAME                                                       REFERENCE                         TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/istio-ingressgateway   Deployment/istio-ingressgateway   7%/80%    1         5         1          5m57s
horizontalpodautoscaler.autoscaling/istiod                 Deployment/istiod                 1%/80%    1         5         1          6m16s
```

También puede obtener conclusiones adicionales sobre la instalación al observar los registros de Istio Operator.

```bash
kubectl logs -n istio-operator -l name=istio-operator -f
```

Si `istio-ingressgateway` muestra una dirección IP externa de `<pending>`, espere unos minutos hasta que las redes de Azure hayan asignado una dirección IP.

Todos los pods deben mostrar un estado de `Running`. Si los pods no tienen estos estados, espere un minuto o dos hasta que los tengan. Si algún pod notifica un problema, use el comando [kubectl describe pod][kubectl-describe] para revisar su salida y su estado.

## <a name="accessing-the-add-ons"></a>Acceso a complementos

Istio Operator instaló una serie de complementos que ofrecen funcionalidad adicional. Las aplicaciones web de los complementos **no** se exponen de forma pública a través de una dirección IP externa. 

Para acceder a las interfaces de usuario de los complementos, use el comando `istioctl dashboard`. Este comando usa [kubectl port-forward][kubectl-port-forward] y un puerto aleatorio para crear una conexión segura entre la máquina cliente y el pod correspondiente en el clúster de AKS. A continuación, abre automáticamente la aplicación web del complemento en el explorador predeterminado.

### <a name="grafana"></a>Grafana

Los paneles de análisis y supervisión para Istio los proporciona [Grafana][grafana]. Recuerde usar las credenciales que creó a través del secreto de Grafana cuando se le solicite. Para abrir el panel de Grafana de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard grafana
```

### <a name="prometheus"></a>Prometheus

Las métricas para Istio las proporciona [Prometheus][prometheus]. Para abrir el panel de Prometheus de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard prometheus
```

### <a name="jaeger"></a>Jaeger

El seguimiento de Istio lo proporciona [Jaeger][jaeger]. Para abrir el panel de Jaeger de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard jaeger
```

### <a name="kiali"></a>Kiali

[Kiali][kiali] proporciona un panel de observación de malla de servicio. Recuerde usar las credenciales que creó a través del secreto de Kiali cuando se le solicite. Para abrir el panel de Kiali de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard kiali
```

### <a name="envoy"></a>Envoy

Hay disponible una interfaz simple a los proxies de [Envoy][envoy]. Proporciona información de configuración y métricas de un proxy de Envoy que se ejecuta en un pod especificado. Para abrir la interfaz de Envoy de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard envoy <pod-name>.<namespace>
```

## <a name="uninstall-istio-from-aks"></a>Desinstalar Istio de AKS

> [!WARNING]
> Eliminar Istio de un sistema en ejecución puede provocar problemas de tráfico entre sus servicios. Asegúrese de haber hecho los aprovisionamientos necesarios para que su sistema funcione correctamente sin Istio antes de continuar.

### <a name="remove-istio"></a>Eliminación de Istio

Para quitar Istio del clúster de AKS, elimine el recurso `IstioOperator` denominado `istio-control-plane` que se agregó anteriormente. Istio Operator reconocerá que se ha quitado la especificación de Istio Operator y, a continuación, eliminará todos los componentes de Istio asociados.

```bash
kubectl delete istiooperator istio-control-plane -n istio-system
```

Puede ejecutar lo siguiente para comprobar que se eliminaron todos los componentes de Istio.

```bash
kubectl get all -n istio-system
```

### <a name="remove-istio-operator"></a>Eliminación de Istio Operator

Una vez que Istio se ha desinstalado correctamente, también puede quitar Istio Operator.

```bash
istioctl operator remove
```

Y, por último, quite los espacios de nombres `istio-`.

```bash
kubectl delete ns istio-system
kubectl delete ns istio-operator
```

## <a name="next-steps"></a>Pasos siguientes

Para explorar más opciones de instalación y configuración para Istio, consulte las siguientes guías oficiales de Istio:

- [Istio - Installation Guides][istio-installation-guides] (Istio - Guías de instalación)

También puede seguir otros escenarios mediante:

- [Ejemplo de la aplicación BookInfo de Istio][istio-bookinfo-example]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-faq]: https://istio.io/faq/general/
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/news/
[istio-installation-guides]: https://istio.io/docs/setup/install/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-istioctl]: https://istio.io/docs/setup/install/istioctl/
[istio-install-operator]: https://istio.io/latest/docs/setup/install/operator/
[istio-configuration-profiles]: https://istio.io/docs/setup/additional-setup/config-profiles/
[istio-control-plane]: https://istio.io/docs/reference/config/istio.operator.v1alpha1/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/

[istio-feature-stages]: https://istio.io/about/feature-stages/
[istio-feature-sds]: https://istio.io/docs/tasks/traffic-management/ingress/secure-ingress-sds/
[istio-feature-cni]: https://istio.io/docs/setup/additional-setup/cni/

[kubernetes-operator]: https://kubernetes.io/docs/concepts/extend-kubernetes/operator/
[kubernetes-feature-sa-projected-volume]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-node-selectors]: ./concepts-clusters-workloads.md#node-selectors
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[envoy]: https://www.envoyproxy.io/

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
