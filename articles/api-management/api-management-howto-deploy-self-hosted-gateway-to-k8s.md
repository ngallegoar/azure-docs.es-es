---
title: Implementación de una puerta de enlace autohospedada de Azure API Management en Kubernetes | Microsoft Docs
description: Aprenda a implementar una puerta de enlace autohospedada de Azure API Management en Kubernetes
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: 109316acb73d3c5f00186298c1f8840c516e5790
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73510593"
---
# <a name="deploy-a-self-hosted-azure-api-management-gateway-to-kubernetes"></a>Implementación de una puerta de enlace autohospedada de Azure API Management en Kubernetes

En este artículo se detallan los pasos para implementar una puerta de enlace autohospedada de Azure API Management en un clúster de Kubernetes.

> [!NOTE]
> La característica de puerta de enlace autohospedada se encuentra en versión preliminar. Durante la versión preliminar, la puerta de enlace autohospedada solo está disponible en los niveles Desarrollador y Premium sin cargo adicional. El nivel de Desarrollador está limitado a una única implementación de puerta de enlace autohospedada.


## <a name="prerequisites"></a>Prerequisites

- Completar la guía de inicio rápido siguiente: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)
- Cree un clúster de Kubernetes. [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) es una buena opción para fines de desarrollo y evaluación. En el caso de las cargas de trabajo de producción, puede usar [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) o un clúster de Kubernetes en una nube externa.
- [Aprovisione un recurso de puerta de enlace en la instancia de API Management](api-management-howto-provision-self-hosted-gateway.md).

## <a name="deploy-the-gateway-to-kubernetes"></a>Implementar la puerta de enlace en Kubernetes

1. Seleccione **Puertas de enlace** en **Configuración**.
2. Seleccione el recurso de puerta de enlace que desea implementar.
3. Seleccione **Implementación**.
4. Tenga en cuenta que en el cuadro de texto **Token** se generará automáticamente un nuevo token con los valores predeterminados de **Expiración** y **Clave secreta**. Ajuste uno o ambos si lo desea y seleccione **Generar** para crear un nuevo token.
5. Asegúrese de que **Kubernetes** esté seleccionado en **Scripts de implementación**.
6. Seleccione el vínculo al archivo **<gateway-name>.yml** junto a **Implementación** para descargar el archivo.
7. Ajuste las asignaciones de puerto y el nombre del contenedor en el archivo yml según sea necesario.
8. Seleccione el icono **copiar** situado en el extremo derecho del cuadro de texto **Implementar** para guardar el comando `kubectl` en el portapapeles. 
9. Pegue el comando en la ventana de terminal (o comando). Tenga en cuenta que el comando espera que el archivo de entorno descargado esté presente en el directorio actual.
```console
    kubectl apply -f <gateway-name>.yaml
```
10. Ejecute el comando. El comando indica al clúster de Kubernetes que ejecute el contenedor mediante la imagen de la puerta de enlace autohospedada descargada de Microsoft Container Registry y que configure el contenedor para exponer los puertos HTTP (8080) y HTTPS (443).
11. Ejecute el siguiente comando para comprobar que el pod de puerta de enlace se está ejecutando. Tenga en cuenta que el nombre del pod será diferente. 
```console
kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
local-gateway-55f774f844-bv9wt    1/1     Running   0          1m
```
12. Ejecute el siguiente comando para comprobar que el servicio de puerta de enlace se está ejecutando. Tenga en cuenta que el nombre del servicio será diferente. 
```console
kubectl get services
NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
localgateway     NodePort    10.110.230.87   <none>        80:32504/TCP,443:30043/TCP   1m
```
13. Vuelva a Azure Portal y confirme que el nodo de puerta de enlace que acaba de implementar notifique que su estado sea correcto.

![estado de la puerta de enlace](media/api-management-howto-deploy-self-hosted-gateway-to-k8s/status.png)

> [!TIP]
> Use el comando <code>kubectl logs <gateway-pod-name></code> para ver una instantánea del registro de la puerta de enlace autohospedada.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la puerta de enlace autohospedada, consulte [Introducción a la puerta de enlace autohospedada de Azure API Management](self-hosted-gateway-overview.md)
* Más información acerca de [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/intro-kubernetes)


