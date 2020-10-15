---
title: Habilitar la compatibilidad con varios espacios de nombres para el controlador de entrada de Application Gateway
description: Este artículo ofrece información sobre cómo habilitar la compatibilidad con varios espacios de nombres en un clúster de Kubernetes con un controlador de entrada de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 953430421bd30aaa1df352451b549994aeaa1a70
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85556159"
---
# <a name="enable-multiple-namespace-support-in-an-aks-cluster-with-application-gateway-ingress-controller"></a>Habilitar la compatibilidad con varios espacios de nombres en un clúster de AKS con un controlador de entrada de Application Gateway

## <a name="motivation"></a>Motivación
Los [espacios de nombres](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) de Kubernetes permiten que un clúster de Kubernetes se divida en particiones y se asigne a subgrupos de un equipo mayor. Posteriormente, estos subequipos pueden implementar y administrar la infraestructura con controles más precisos de recursos, seguridad, configuración, etc. Kubernetes permite definir uno o varios recursos de entrada de forma independiente dentro de cada espacio de nombres.

A partir de la versión 0.7, el [Controlador de entrada de Azure Application Gateway para Kubernetes](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/master/README.md) (AGIC) puede ingerir eventos y observar varios espacios de nombres. Si el administrador de AKS decide usar [App Gateway](https://azure.microsoft.com/services/application-gateway/) como entrada, todos los espacios de nombres usarán la misma instancia de Application Gateway. Una sola instalación del controlador de entrada supervisará los espacios de nombres accesibles y configurará la instancia de Application Gateway a la que está asociado.

La versión 0.7 de AGIC seguirá observando el espacio de nombres `default` de forma exclusiva, a menos que se cambie explícitamente a uno o varios espacios de nombres diferentes en la configuración de Helm (consulte la siguiente sección).

## <a name="enable-multiple-namespace-support"></a>Habilitar la compatibilidad con varios espacios de nombres
Para habilitar la compatibilidad con varios espacios de nombres:
1. modifique el archivo [helm-config.yaml](#sample-helm-config-file) de una de las siguientes maneras:
   - elimine la clave `watchNamespace` completamente de [helm-config.yaml](#sample-helm-config-file): AGIC observará todos los espacios de nombres
   - establezca `watchNamespace` en una cadena vacía: AGIC observará todos los espacios de nombres
   - agregue varios espacios de nombres separados por una coma (`watchNamespace: default,secondNamespace`): AGIC observará estos espacios de nombres de forma exclusiva
2. aplique los cambios a la plantilla de Helm con: `helm install -f helm-config.yaml application-gateway-kubernetes-ingress/ingress-azure`

Una vez que se implemente con la capacidad de observar varios espacios de nombres, AGIC:
  - enumerará los recursos de entrada de todos los espacios de nombres accesibles
  - filtrará los recursos de entrada anotados con `kubernetes.io/ingress.class: azure/application-gateway`
  - combinará la [configuración de Application Gateway](https://github.com/Azure/azure-sdk-for-go/blob/37f3f4162dfce955ef5225ead57216cf8c1b2c70/services/network/mgmt/2016-06-01/network/models.go#L1710-L1744)
  - aplicará la configuración a la instancia de Application Gateway asociada a través de [ARM](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)

## <a name="conflicting-configurations"></a>Configuraciones conflictivas
Los [recursos de entrada](https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource) de varios espacios de nombres pueden indicar a AGIC que cree configuraciones conflictivas para una sola instancia de Application Gateway. (Dos entradas reclaman el mismo dominio por instancia).

En la parte superior de la jerarquía: los **clientes de escucha** (dirección IP, puerto y host) y las **reglas de enrutamiento** (cliente de escucha de enlace, grupo de back-end y configuración HTTP) pueden crearse y compartirse con varios espacios de nombres o entradas.

Por otra parte, las rutas de acceso, los grupos de back-end, la configuración HTTP y los certificados TLS solo se pueden crear mediante un espacio de nombres y los duplicados se eliminarán.

Por ejemplo, considere los siguientes espacios de nombres definidos de recursos de entrada duplicados `staging` y `production` para `www.contoso.com`:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: websocket-ingress
  namespace: staging
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: www.contoso.com
      http:
        paths:
          - backend:
              serviceName: web-service
              servicePort: 80
```

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: websocket-ingress
  namespace: production
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: www.contoso.com
      http:
        paths:
          - backend:
              serviceName: web-service
              servicePort: 80
```

A pesar de los dos recursos de entrada que exigen tráfico para que `www.contoso.com` se enrute a los respectivos espacios de nombres de Kubernetes, solo un back-end puede atender el tráfico. AGIC crearía una configuración basada en el orden de llegada de uno de los recursos. Si se crean dos recursos de entrada al mismo tiempo, el primero en el alfabeto tendrá prioridad. En el ejemplo anterior, solo se podrá crear la configuración para la entrada de `production`. Application Gateway se configurará con los siguientes recursos:

  - Cliente de escucha: `fl-www.contoso.com-80`
  - Regla de enrutamiento: `rr-www.contoso.com-80`
  - Grupo de back-end: `pool-production-contoso-web-service-80-bp-80`
  - Configuración de HTTP: `bp-production-contoso-web-service-80-80-websocket-ingress`
  - Sondeo de estado: `pb-production-contoso-web-service-80-websocket-ingress`

Tenga en cuenta que, a excepción del *cliente de escucha* y la *regla de enrutamiento*, los recursos de Application Gateway creados incluyen el nombre del espacio de nombres (`production`) para el que se crearon.

Si los dos recursos de entrada se introducen en el clúster de AKS en diferentes momentos, es probable que AGIC acabe en un escenario en el que vuelva a configurar Application Gateway y redirija el tráfico del `namespace-B` al `namespace-A`.

Por ejemplo, si agregó `staging` primero, AGIC configurará Application Gateway para enrutar el tráfico al grupo de back-end provisional. En una fase posterior, la introducción de la entrada de `production`, provocará que AGIC se reprograme Application Gateway, lo que iniciará el enrutamiento del tráfico al grupo de back-end de `production`.

## <a name="restrict-access-to-namespaces"></a>Restringir el acceso a los espacios de nombres
De forma predeterminada, AGIC configurará Application Gateway en función de la entrada anotada dentro de cualquier espacio de nombres. Si desea limitar este comportamiento, tiene las siguientes opciones:
  - limitar los espacios de nombres, definiendo explícitamente los espacios de nombres que AGIC debe observar a través de la clave de YAML `watchNamespace` en [helm-config.yaml](#sample-helm-config-file)
  - usar [Role/RoleBinding](https://docs.microsoft.com/azure/aks/azure-ad-rbac) para limitar AGIC a espacios de nombres específicos

## <a name="sample-helm-config-file"></a>Archivo de ejemplo de configuración de Helm

```yaml
    # This file contains the essential configs for the ingress controller helm chart

    # Verbosity level of the App Gateway Ingress Controller
    verbosityLevel: 3
    
    ################################################################################
    # Specify which application gateway the ingress controller will manage
    #
    appgw:
        subscriptionId: <subscriptionId>
        resourceGroup: <resourceGroupName>
        name: <applicationGatewayName>
    
        # Setting appgw.shared to "true" will create an AzureIngressProhibitedTarget CRD.
        # This prohibits AGIC from applying config for any host/path.
        # Use "kubectl get AzureIngressProhibitedTargets" to view and change this.
        shared: false
    
    ################################################################################
    # Specify which kubernetes namespace the ingress controller will watch
    # Default value is "default"
    # Leaving this variable out or setting it to blank or empty string would
    # result in Ingress Controller observing all acessible namespaces.
    #
    # kubernetes:
    #   watchNamespace: <namespace>
    
    ################################################################################
    # Specify the authentication with Azure Resource Manager
    #
    # Two authentication methods are available:
    # - Option 1: AAD-Pod-Identity (https://github.com/Azure/aad-pod-identity)
    armAuth:
        type: aadPodIdentity
        identityResourceID: <identityResourceId>
        identityClientID:  <identityClientId>
    
    ## Alternatively you can use Service Principal credentials
    # armAuth:
    #    type: servicePrincipal
    #    secretJSON: <<Generate this value with: "az ad sp create-for-rbac --subscription <subscription-uuid> --sdk-auth | base64 -w0" >>
    
    ################################################################################
    # Specify if the cluster is RBAC enabled or not
    rbac:
        enabled: false # true/false
    
    # Specify aks cluster related information. THIS IS BEING DEPRECATED.
    aksClusterConfiguration:
        apiServerAddress: <aks-api-server-address>
```

