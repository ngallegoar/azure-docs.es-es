---
title: Instalación de Istio en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo instalar y usar Istio para crear una malla de servicio en un clúster de Azure Kubernetes Service (AKS).
author: paulbouwer
ms.topic: article
ms.date: 02/19/2020
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: f0fe4ab46bfe5c0c0c2ea67aa2e2694321628be5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136370"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Instalación y uso de Istio en Azure Kubernetes Service (AKS)

[Istio][istio-github] es una malla de servicio de código abierto que proporciona un conjunto clave de funcionalidades en los microservicios de un clúster de Kubernetes. Estas características incluyen la administración del tráfico, la seguridad e identidad de servicio, el cumplimiento de directivas y la observación. Para más información sobre Istio, vea la documentación del sitio oficial [¿Qué es Istio?][istio-docs-concepts].

En este artículo se explica cómo instalar Istio. El binario de cliente `istioctl` de Istio se instala en el equipo cliente y, a continuación, los componentes de Istio se instalan en un clúster de Kubernetes en AKS.

> [!NOTE]
> Las siguientes instrucciones son aplicables a la versión `1.4.0` de Istio.
>
> El equipo de Istio ha probado las versiones de Istio `1.4.x` y las ha comparado con las versiones de Kubernetes `1.13`, `1.14`, `1.15`. Encontrará otras versiones de Istio en [GitHub: Istio > Releases][istio-github-releases] (GitHub: Istio > Versiones), información sobre cada una de las versiones en [Istio News][istio-release-notes] (Novedades de Istio) y las versiones compatibles de Kubernetes en [Istio General FAQ][istio-faq] (Istio: Preguntas frecuentes generales).

En este artículo aprenderá a:

> [!div class="checklist"]
> * Descargar e instalar el binario de cliente istioctl de Istio
> * Instalar Istio en AKS
> * Validar la instalación de Istio
> * Acceder a los complementos
> * Desinstalar Istio de AKS

## <a name="before-you-begin"></a>Antes de empezar

En los pasos detallados en este artículo se da por hecho que ha creado un clúster de AKS (Kubernetes `1.13` y versiones posteriores, con RBAC habilitado) y que ha establecido una conexión `kubectl` con el clúster. Si necesita ayuda con cualquiera de estos elementos, consulte el [inicio rápido de AKS][aks-quickstart].

Asegúrese de leer la documentación de [rendimiento y escalabilidad de Istio](https://istio.io/docs/concepts/performance-and-scalability/) para comprender los requisitos de recursos adicionales para ejecutar Istio en el clúster de AKS. Los requisitos de memoria y núcleos varían en función de la carga de trabajo concreta. Elija el número de nodos y el tamaño de la máquina virtual adecuados para la instalación.

En este artículo, las instrucciones de instalación de Istio se dividen en varios pasos discretos. El resultado final presenta la misma estructura que las [instrucciones][istio-install-istioctl] de instalación oficiales de Istio.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/istio/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [MacOS - download and install client binary](includes/servicemesh/istio/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/istio/install-client-binary-windows.md)]

::: zone-end

## <a name="install-the-istio-components-on-aks"></a>Instalar los componentes de Istio en AKS

Instalaremos [Grafana][grafana] y [Kiali][kiali] como parte de nuestra instalación de Istio. Grafana proporciona paneles de análisis y supervisión, y Kiali proporciona un panel de observación de la malla de servicio. En la configuración, cada uno de estos componentes requiere credenciales que deben proporcionarse como un [secreto][kubernetes-secrets].

Antes de que podamos instalar los componentes de Istio, debemos crear los secretos para Grafana y Kiali. Estos secretos deben instalarse en el espacio de nombres `istio-system` que se usará en Istio, por lo que también es necesario crear el espacio de nombres. Es necesario usar la opción `--save-config` al crear el espacio de nombres a través de `kubectl create`, para que el instalador de Istio pueda ejecutar `kubectl apply` en este objeto en el futuro.

```console
kubectl create namespace istio-system --save-config
```

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - create secrets for Grafana and Kiali](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash check for CRDs](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell check for CRDs](includes/servicemesh/istio/install-create-secrets-powershell.md)]

::: zone-end

### <a name="install-istio-components"></a>Instalar componentes de Istio

Ahora que hemos creado correctamente los secretos de Grafana y Kiali en nuestro clúster de AKS, es hora de instalar los componentes de Istio. 

El método de instalación de [Helm][helm] para Istio dejará de usarse en el futuro. El nuevo método aprovecha el archivo binario de cliente `istioctl`, los [perfiles de configuración de Istio][istio-configuration-profiles] y las nuevas [especificaciones y API de plano de control de Istio][istio-control-plane]. Este nuevo método es el que usaremos para instalar Istio.

> [!NOTE]
> Actualmente, Istio debe programarse para su ejecución en nodos de Linux. Si tiene nodos de Windows Server en el clúster, debe asegurarse de que los pods de Istio solo se programan para ejecutarse en nodos de Linux. Usaremos [selectores de nodo][kubernetes-node-selectors] para garantizar que los pods se programan en los nodos correctos.

> [!CAUTION]
> Las características [SDS (servicio de detección de secretos)][istio-feature-sds] y [CNI][istio-feature-cni] de Istio se encuentran actualmente en fase [alfa][istio-feature-stages], por lo que debe considerarse su habilitación. Además, la función de Kubernetes [Service Account Token Volume Projection][kubernetes-feature-sa-projected-volume] (un requisito de SDS) no está habilitada en las versiones actuales de AKS.
Cree un archivo llamado `istio.aks.yaml` con el siguiente contenido. Este archivo contendrá los detalles de [especificación de plano de control de Istio][istio-control-plane] para la configuración de Istio.

```yaml
apiVersion: install.istio.io/v1alpha2
kind: IstioControlPlane
spec:
  # Use the default profile as the base
  # More details at: https://istio.io/docs/setup/additional-setup/config-profiles/
  profile: default
  values:
    global:
      # Ensure that the Istio pods are only scheduled to run on Linux nodes
      defaultNodeSelector:
        beta.kubernetes.io/os: linux
      # Enable mutual TLS for the control plane
      controlPlaneSecurityEnabled: true
      mtls:
        # Require all service to service communication to have mtls
        enabled: false
    grafana:
      # Enable Grafana deployment for analytics and monitoring dashboards
      enabled: true
      security:
        # Enable authentication for Grafana
        enabled: true
    kiali:
      # Enable the Kiali deployment for a service mesh observability dashboard
      enabled: true
    tracing:
      # Enable the Jaeger deployment for tracing
      enabled: true
```

Instale Istio con el comando `istioctl apply` y el archivo de especificaciones de plano de control de Istio `istio.aks.yaml` anterior de la siguiente manera:

```console
istioctl manifest apply -f istio.aks.yaml --logtostderr --set installPackagePath=./install/kubernetes/operator/charts
```

El instalador implementará una serie de [CRD][kubernetes-crd] y, a continuación, administrará las dependencias para instalar todos los objetos pertinentes definidos para esta configuración de Istio. Debe ver algo parecido al siguiente fragmento de salida.

```console
Applying manifests for these components:
- Tracing
- EgressGateway
- NodeAgent
- Grafana
- Policy
- Citadel
- CertManager
- IngressGateway
- Injector
- Prometheus
- PrometheusOperator
- Kiali
- Telemetry
- Galley
- Cni
- Pilot
- Base
- CoreDNS
NodeAgent is waiting on a prerequisite...
Telemetry is waiting on a prerequisite...
Galley is waiting on a prerequisite...
Cni is waiting on a prerequisite...
Grafana is waiting on a prerequisite...
Policy is waiting on a prerequisite...
Citadel is waiting on a prerequisite...
EgressGateway is waiting on a prerequisite...
Tracing is waiting on a prerequisite...
Kiali is waiting on a prerequisite...
PrometheusOperator is waiting on a prerequisite...
IngressGateway is waiting on a prerequisite...
Prometheus is waiting on a prerequisite...
CertManager is waiting on a prerequisite...
Injector is waiting on a prerequisite...
Pilot is waiting on a prerequisite...
Applying manifest for component Base
Waiting for CRDs to be applied.
CRDs applied.
Finished applying manifest for component Base
Prerequisite for Tracing has completed, proceeding with install.
Prerequisite for Injector has completed, proceeding with install.
Prerequisite for Telemetry has completed, proceeding with install.
Prerequisite for Policy has completed, proceeding with install.
Prerequisite for PrometheusOperator has completed, proceeding with install.
Prerequisite for NodeAgent has completed, proceeding with install.
Prerequisite for IngressGateway has completed, proceeding with install.
Prerequisite for Kiali has completed, proceeding with install.
Prerequisite for EgressGateway has completed, proceeding with install.
Prerequisite for Galley has completed, proceeding with install.
Prerequisite for Grafana has completed, proceeding with install.
Prerequisite for Cni has completed, proceeding with install.
Prerequisite for Citadel has completed, proceeding with install.
Applying manifest for component Tracing
Prerequisite for Prometheus has completed, proceeding with install.
Prerequisite for Pilot has completed, proceeding with install.
Prerequisite for CertManager has completed, proceeding with install.
Applying manifest for component Kiali
Applying manifest for component Prometheus
Applying manifest for component IngressGateway
Applying manifest for component Policy
Applying manifest for component Telemetry
Applying manifest for component Citadel
Applying manifest for component Galley
Applying manifest for component Pilot
Applying manifest for component Injector
Applying manifest for component Grafana
Finished applying manifest for component Kiali
Finished applying manifest for component Tracing
Finished applying manifest for component Prometheus
Finished applying manifest for component Citadel
Finished applying manifest for component Policy
Finished applying manifest for component IngressGateway
Finished applying manifest for component Injector
Finished applying manifest for component Galley
Finished applying manifest for component Pilot
Finished applying manifest for component Grafana
Finished applying manifest for component Telemetry

Component IngressGateway installed successfully:
================================================

serviceaccount/istio-ingressgateway-service-account created
deployment.apps/istio-ingressgateway created
gateway.networking.istio.io/ingressgateway created
sidecar.networking.istio.io/default created
poddisruptionbudget.policy/ingressgateway created
horizontalpodautoscaler.autoscaling/istio-ingressgateway created
service/istio-ingressgateway created

...
```

En este punto, ya tendrá implementado Istio en su clúster de AKS. Para asegurarse de que ha implementado correctamente Istio, vaya a la siguiente sección para [validar la instalación de Istio](#validate-the-istio-installation).

## <a name="validate-the-istio-installation"></a>Validar la instalación de Istio

En primer lugar, confirme que se han creado los servicios esperados. Use el comando [kubectl get svc][kubectl-get] para ver los servicios en ejecución. Consulte el espacio de nombres `istio-system` donde se instalaron los componentes de Istio mediante el gráfico de Helm `istio`:

```console
kubectl get svc --namespace istio-system --output wide
```

La salida del ejemplo siguiente muestra los servicios que ahora deben estar en ejecución:

- `istio-*` servicios
- Servicios de seguimiento de complementos `jaeger-*`, `tracing` y `zipkin`
- Servicio de métricas de complementos `prometheus`
- Servicio de panel de supervisión y análisis de complementos `grafana`
- Servicio de panel de malla de servicio de complementos `kiali`

Si `istio-ingressgateway` muestra una dirección IP externa de `<pending>`, espere unos minutos hasta que las redes de Azure hayan asignado una dirección IP.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                      AGE   SELECTOR
grafana                  ClusterIP      10.0.116.147   <none>           3000/TCP                                                                                                                     92s   app=grafana
istio-citadel            ClusterIP      10.0.248.152   <none>           8060/TCP,15014/TCP                                                                                                           94s   app=citadel
istio-galley             ClusterIP      10.0.50.100    <none>           443/TCP,15014/TCP,9901/TCP,15019/TCP                                                                                         93s   istio=galley
istio-ingressgateway     LoadBalancer   10.0.36.213    20.188.221.111   15020:30369/TCP,80:31368/TCP,443:30045/TCP,15029:32011/TCP,15030:31212/TCP,15031:32411/TCP,15032:30009/TCP,15443:30010/TCP   93s   app=istio-ingressgateway
istio-pilot              ClusterIP      10.0.23.222    <none>           15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                       93s   istio=pilot
istio-policy             ClusterIP      10.0.59.250    <none>           9091/TCP,15004/TCP,15014/TCP                                                                                                 93s   istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.123.219   <none>           443/TCP                                                                                                                      93s   istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.216.9     <none>           9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                       89s   istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                   96s   app=jaeger
jaeger-collector         ClusterIP      10.0.221.24    <none>           14267/TCP,14268/TCP,14250/TCP                                                                                                95s   app=jaeger
jaeger-query             ClusterIP      10.0.46.154    <none>           16686/TCP                                                                                                                    95s   app=jaeger
kiali                    ClusterIP      10.0.174.97    <none>           20001/TCP                                                                                                                    94s   app=kiali
prometheus               ClusterIP      10.0.245.226   <none>           9090/TCP                                                                                                                     94s   app=prometheus
tracing                  ClusterIP      10.0.249.95    <none>           9411/TCP                                                                                                                     95s   app=jaeger
zipkin                   ClusterIP      10.0.154.89    <none>           9411/TCP                                                                                                                     94s   app=jaeger
```

A continuación, confirme que se han creado los pods necesarios. Use el comando [kubectl get pods][kubectl-get] y vuelva a consultar el espacio de nombres `istio-system`:

```console
kubectl get pods --namespace istio-system
```

La salida de ejemplo siguiente muestra los pods que se ejecutan:

- los pods `istio-*`
- el pod de métricas de complementos `prometheus-*`
- pod del panel de supervisión y análisis de complementos `grafana-*`
- el pod del panel de malla de servicio de complementos `kiali`

```console
NAME                                          READY   STATUS    RESTARTS   AGE
grafana-6bc97ff99-k9sk4                       1/1     Running   0          92s
istio-citadel-6b5c754454-tb8nf                1/1     Running   0          94s
istio-galley-7d6d78d7c5-zshsd                 2/2     Running   0          94s
istio-ingressgateway-85869c5cc7-x5d76         1/1     Running   0          95s
istio-pilot-787d6995b5-n5vrj                  2/2     Running   0          94s
istio-policy-6cf4fbc8dc-sdsg5                 2/2     Running   2          94s
istio-sidecar-injector-5d5b978668-wrz2s       1/1     Running   0          94s
istio-telemetry-5498db684-6kdnw               2/2     Running   1          94s
istio-tracing-78548677bc-74tx6                1/1     Running   0          96s
kiali-59b7fd7f68-92zrh                        1/1     Running   0          95s
prometheus-7c7cf9dbd6-rjxcv                   1/1     Running   0          94s
```

Todos los pods deben mostrar un estado de `Running`. Si los pods no tienen estos estados, espere un minuto o dos hasta que los tengan. Si algún pod notifica un problema, use el comando [kubectl describe pod][kubectl-describe] para revisar su salida y su estado.

## <a name="accessing-the-add-ons"></a>Acceso a complementos

En los pasos de instalación anteriores, Istio instaló una serie de complementos que ofrecen funcionalidad adicional. Las aplicaciones web de los complementos **no** se exponen de forma pública a través de una dirección IP externa. 

Para acceder a las interfaces de usuario de los complementos, use el comando `istioctl dashboard`. Este comando utiliza [kubectl port-forward][kubectl-port-forward] y un puerto aleatorio para crear una conexión segura entre la máquina cliente y el pod correspondiente en el clúster de AKS. A continuación, abre automáticamente la aplicación web del complemento en el explorador predeterminado.

Asimismo, agregamos una capa adicional de seguridad para Grafana y Kiali especificando sus respectivas credenciales anteriormente en este artículo.

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

### <a name="remove-istio-components-and-namespace"></a>Quitar los componentes y el espacio de nombres de Istio

Para quitar Istio del clúster de AKS, use el comando `istioctl manifest generate` con el archivo de especificaciones de plano de control de Istio`istio.aks.yaml`. Se generará el manifiesto implementado, que se canalizará a `kubectl delete` para quitar todos los componentes instalados y el espacio de nombres `istio-system`.

```console
istioctl manifest generate -f istio.aks.yaml -o istio-components-aks --logtostderr --set installPackagePath=./install/kubernetes/operator/charts 

kubectl delete -f istio-components-aks -R
```

### <a name="remove-istio-crds-and-secrets"></a>Eliminación de los CRD y los secretos de Istio

Los comandos anteriores eliminan todos los componentes y el espacio de nombres de Istio, pero aún quedan los secretos de Istio generados. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - remove Istio secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - remove Istio secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - remove Istio secrets](includes/servicemesh/istio/uninstall-powershell.md)]

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

En la siguiente documentación se describe cómo usar Istio para proporcionar enrutamiento inteligente para implantar un lanzamiento controlado:

> [!div class="nextstepaction"]
> [Escenario de enrutamiento inteligente con Istio en AKS][istio-scenario-routing]

Para explorar más opciones de instalación y configuración para Istio, consulte las siguientes guías oficiales de Istio:

- [Istio - Installation Guides][istio-installation-guides] (Istio - Guías de instalación)

También puede seguir otros escenarios mediante:

- [Ejemplo de la aplicación BookInfo de Istio][istio-bookinfo-example]

Para aprender a supervisar la aplicación de AKS con Application Insights e Istio, consulte la siguiente documentación de Azure Monitor:

- [Supervisión de aplicaciones sin instrumentación para aplicaciones hospedadas de Kubernetes][app-insights]

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
[istio-configuration-profiles]: https://istio.io/docs/setup/additional-setup/config-profiles/
[istio-control-plane]: https://istio.io/docs/reference/config/istio.operator.v1alpha1/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/

[istio-feature-stages]: https://istio.io/about/feature-stages/
[istio-feature-sds]: https://istio.io/docs/tasks/traffic-management/ingress/secure-ingress-sds/
[istio-feature-cni]: https://istio.io/docs/setup/additional-setup/cni/

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-feature-sa-projected-volume]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-node-selectors]: https://docs.microsoft.com/azure/aks/concepts-clusters-workloads#node-selectors
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[envoy]: https://www.envoyproxy.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./servicemesh-istio-scenario-routing.md
