---
title: Instalación de Consul en Azure Kubernetes Service (AKS)
description: Aprenda cómo instalar y usar Consul para crear una malla de servicio en un clúster de Azure Kubernetes Service (AKS).
author: dstrebel
ms.topic: article
ms.date: 10/09/2019
ms.author: dastrebe
zone_pivot_groups: client-operating-system
ms.openlocfilehash: ef77037526beba1be2e4e8a834dbd09c8a73310c
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84197227"
---
# <a name="install-and-use-consul-in-azure-kubernetes-service-aks"></a>Instalación y uso de Consul en Azure Kubernetes Service (AKS)

[Consul][consul-github] es una malla de servicio de código abierto que proporciona un conjunto clave de funcionalidades en los microservicios de un clúster de Kubernetes. Estas características incluyen la detección y segmentación de servicios, la comprobación de estado y la observación. Para más información sobre Consul, consulte la documentación oficial en [¿Qué es Consul?][consul-docs-concepts].

En este artículo se explica cómo instalar Consul. Los componentes de Consul se instalan en un clúster de Kubernetes en AKS.

> [!NOTE]
> Estas instrucciones hacen referencia a la versión `1.6.0` de Consul y utilizan al menos la versión `2.14.2` de Helm.
>
> Las versiones `1.6.x` de Consul se pueden ejecutar en las versiones `1.13+` de Kubernetes. Puede encontrar versiones de Consul adicionales en [GitHub: versiones de Consul][consul-github-releases] e información sobre cada una de las versiones en [Consul: notas de la versión][consul-release-notes].

En este artículo aprenderá a:

> [!div class="checklist"]
> * Instalar los componentes de Consul en AKS
> * Validar la instalación de Consul
> * Desinstalar Consul de AKS

## <a name="before-you-begin"></a>Antes de empezar

En los pasos detallados en este artículo se da por hecho que ha creado un clúster de AKS (Kubernetes `1.13` y versiones posteriores, con RBAC habilitado) y que ha establecido una conexión `kubectl` con el clúster. Si necesita ayuda con cualquiera de estos elementos, consulte el [inicio rápido de AKS][aks-quickstart]. Asegúrese de que el clúster tiene al menos tres nodos en el grupo de nodos de Linux.

Necesitará [Helm][helm] para seguir estas instrucciones e instalar Consul. Es recomendable que tenga la versión estable más reciente correctamente instalada y configurada en el clúster. Si necesita ayuda con la instalación de Helm, consulte las [instrucciones de instalación de Helm para AKS][helm-install]. Todos los pods de Consul también deben programarse para ejecutarse en nodos de Linux.

En este artículo, las instrucciones de instalación de Consul se dividen en varios pasos discretos. El resultado final presenta la misma estructura que las [instrucciones][consul-install-k8] de instalación oficiales de Consul.

### <a name="install-the-consul-components-on-aks"></a>Instalar los componentes de Consul en AKS

Comenzaremos por descargar la versión `v0.10.0` del gráfico de Helm de Consul. Esta versión del gráfico incluye la versión `1.6.0` de Consul.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download](includes/servicemesh/consul/download-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [macOS - download](includes/servicemesh/consul/download-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download](includes/servicemesh/consul/download-powershell.md)]

::: zone-end

Use Helm y el gráfico `consul-helm` descargado para instalar los componentes de Consul en el espacio de nombres `consul` del clúster de AKS. 

> [!NOTE]
> **Opciones de instalación**
> 
> Vamos a usar las siguientes opciones como parte de nuestra instalación:
> - `connectInject.enabled=true`: habilita la inserción de proxies en los pods
> - `client.enabled=true`: habilita la ejecución de los clientes de Consul en todos los nodos
> - `client.grpc=true`: habilita el cliente de escucha de gRPC para connectInject
> - `syncCatalog.enabled=true`: sincroniza los servicios de Kubernetes y Consul
>
> **Selectores de nodos**
>
> Actualmente, Consul debe programarse para su ejecución en nodos de Linux. Si tiene nodos de Windows Server en el clúster, debe asegurarse de que los pods de Consul solo se programan para ejecutarse en nodos de Linux. Usaremos [selectores de nodo][kubernetes-node-selectors] para garantizar que los pods se programan en los nodos correctos.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/consul/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/consul/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - install Istio components](includes/servicemesh/consul/install-components-powershell.md)]

::: zone-end

El gráfico de Helm `Consul` implementa una serie de objetos. Puede ver la lista de la salida del comando `helm install` anterior. La implementación de los componentes de Consul puede tardar unos tres minutos, según el entorno del clúster.

En este punto, ya tendrá implementado Consul en su clúster de AKS. Para asegurarse de que ha implementado correctamente Consul, vaya a la siguiente sección para validar su instalación.

## <a name="validate-the-consul-installation"></a>Validar la instalación de Consul

Confirme que los recursos se han creado correctamente. Use los comandos [kubectl get svc][kubectl-get] y [kubectl get pod][kubectl-get] para consultar el espacio de nombres `consul`, donde se instalaron los componentes de Consul con el comando `helm install`:

```console
kubectl get svc --namespace consul --output wide
kubectl get pod --namespace consul --output wide
```

En la siguiente salida de ejemplo se muestran los servicios y pods (programados en nodos de Linux) que deben estar ejecutándose ahora:

```output
NAME                                 TYPE           CLUSTER-IP    EXTERNAL-IP             PORT(S)                                                                   AGE     SELECTOR
consul                               ExternalName   <none>        consul.service.consul   <none>                                                                    38s     <none>
consul-consul-connect-injector-svc   ClusterIP      10.0.98.102   <none>                  443/TCP                                                                   3m26s   app=consul,component=connect-injector,release=consul
consul-consul-dns                    ClusterIP      10.0.46.194   <none>                  53/TCP,53/UDP                                                             3m26s   app=consul,hasDNS=true,release=consul
consul-consul-server                 ClusterIP      None          <none>                  8500/TCP,8301/TCP,8301/UDP,8302/TCP,8302/UDP,8300/TCP,8600/TCP,8600/UDP   3m26s   app=consul,component=server,release=consul
consul-consul-ui                     ClusterIP      10.0.50.188   <none>                  80/TCP                                                                    3m26s   app=consul,component=server,release=consul

NAME                                                              READY   STATUS    RESTARTS   AGE    IP            NODE                            NOMINATED NODE   READINESS GATES
consul-consul-connect-injector-webhook-deployment-99f74fdbcr5zj   1/1     Running   0          3m9s   10.240.0.68   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-jbksc                                               1/1     Running   0          3m9s   10.240.0.44   aks-linux-92468653-vmss000001   <none>           <none>
consul-consul-jkwtq                                               1/1     Running   0          3m9s   10.240.0.70   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-server-0                                            1/1     Running   0          3m9s   10.240.0.91   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-server-1                                            1/1     Running   0          3m9s   10.240.0.38   aks-linux-92468653-vmss000001   <none>           <none>
consul-consul-server-2                                            1/1     Running   0          3m9s   10.240.0.10   aks-linux-92468653-vmss000000   <none>           <none>
consul-consul-sync-catalog-d846b79c-8ssr8                         1/1     Running   2          3m9s   10.240.0.94   aks-linux-92468653-vmss000002   <none>           <none>
consul-consul-tz2t5                                               1/1     Running   0          3m9s   10.240.0.12   aks-linux-92468653-vmss000000   <none>           <none>
```

Todos los pods deben mostrar un estado de `Running`. Si los pods no tienen estos estados, espere un minuto o dos hasta que los tengan. Si algún pod notifica un problema, use el comando [kubectl describe pod][kubectl-describe] para revisar su salida y su estado.

## <a name="accessing-the-consul-ui"></a>Acceso a la interfaz de usuario de Consul

La interfaz de usuario de Consul se instaló en una operación anterior y proporciona la configuración de Consul basada en la interfaz de usuario. La interfaz de usuario de Consul no se expone públicamente a través de una dirección IP externa. Para acceder a la interfaz de usuario de Consul, use el comando [kubectl port-forward][kubectl-port-forward]. Este comando crea una conexión segura el equipo cliente y el pod relevante en el clúster de AKS.

```console
kubectl port-forward -n consul svc/consul-consul-ui 8080:80
```

Ahora puede abrir un explorador y hacer que apunte a `http://localhost:8080/ui` para abrir la interfaz de usuario de Consul. Al abrir la interfaz de usuario, verá lo siguiente:

![Interfaz de usuario de Consul](./media/servicemesh/consul/consul-ui.png)

## <a name="uninstall-consul-from-aks"></a>Desinstalar Consul de AKS

> [!WARNING]
> Eliminar Consul de un sistema en ejecución puede provocar problemas relacionados con el tráfico entre sus servicios. Asegúrese de que su sistema esté bien aprovisionado de forma que pueda funcionar correctamente sin Consul antes de continuar.

### <a name="remove-consul-components-and-namespace"></a>Quitar los componentes y el espacio de nombres de Consul

Para quitar Consul del clúster de AKS, use los siguientes comandos. Los comandos `helm delete` quitarán el gráfico `consul` y el comando `kubectl delete namespace` quitará el espacio de nombres `consul`.

```console
helm delete --purge consul
kubectl delete namespace consul
```

## <a name="next-steps"></a>Pasos siguientes

Para explorar más opciones de instalación y configuración de Consul, consulte los siguientes artículos oficiales:

- [Consul: guía de instalación de Helm][consul-install-k8]
- [Consul: opciones de instalación de Helm][consul-install-helm-options]

También puede seguir otros escenarios mediante:

- [Aplicación de ejemplo de Consul][consul-app-example]
- [Arquitectura de referencia de Kubernetes para Consul][consul-reference]
- [Puertas de enlace de malla de Consul][consul-mesh-gateways]

<!-- LINKS - external -->
[Hashicorp]: https://hashicorp.com
[cosul-github]: https://github.com/hashicorp/consul
[helm]: https://helm.sh

[consul-docs-concepts]: https://www.consul.io/docs/platform/k8s/index.html
[consul-github]: https://github.com/hashicorp/consul
[consul-github-releases]: https://github.com/hashicorp/consul/releases
[consul-release-notes]: https://github.com/hashicorp/consul/blob/master/CHANGELOG.md
[consul-install-download]: https://www.consul.io/downloads.html
[consul-install-k8]: https://learn.hashicorp.com/consul/kubernetes/kubernetes-deployment-guide
[consul-install-helm-options]: https://www.consul.io/docs/platform/k8s/helm.html#configuration-values-
[consul-mesh-gateways]: https://learn.hashicorp.com/consul/kubernetes/mesh-gateways
[consul-reference]: https://learn.hashicorp.com/consul/kubernetes/kubernetes-reference
[consul-app-example]: https://learn.hashicorp.com/consul?track=gs-consul-service-mesh#gs-consul-service-mesh
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[kubernetes-node-selectors]: https://docs.microsoft.com/azure/aks/concepts-clusters-workloads#node-selectors

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[consul-scenario-mtls]: ./consul-mtls.md
[helm-install]: ./kubernetes-helm.md
