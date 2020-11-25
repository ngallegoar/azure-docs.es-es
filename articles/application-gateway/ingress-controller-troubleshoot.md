---
title: Solución de problemas del controlador de entrada de Application Gateway
description: Este artículo ofrece documentación sobre la solución de problemas o preguntas comunes sobre el controlador de entrada de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: troubleshooting
ms.date: 06/18/2020
ms.author: caya
ms.openlocfilehash: f2b9f79f0914e645c736f8a577c46baa42587332
ms.sourcegitcommit: 642988f1ac17cfd7a72ad38ce38ed7a5c2926b6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94874617"
---
# <a name="troubleshoot-common-questions-or-issues-with-ingress-controller"></a>Solución de preguntas o problemas comunes del controlador de entrada

[Azure Cloud Shell](https://shell.azure.com/) es la manera más cómoda de solucionar cualquier problema con la instalación de AKS y AGIC. Inicie Shell desde [shell.azure.com](https://shell.azure.com/) o haga clic en el vínculo:

[![Insertar inicio](https://shell.azure.com/images/launchcloudshell.png "Inicio de Azure Cloud Shell")](https://shell.azure.com)


## <a name="test-with-a-simple-kubernetes-app"></a>Prueba con una aplicación de Kubernetes sencilla

Los siguientes pasos asumen que:
  - Tiene un clúster de AKS con redes avanzadas habilitadas
  - AGIC se ha instalado en el clúster de AKS
  - Ya cuenta con una instancia de Application Gateway en una red virtual compartida con su clúster de AKS

Para comprobar que la instalación de Application Gateway + AKS + AGIC está configurada correctamente, implemente la aplicación más sencilla posible:

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: test-agic-app-pod
  labels:
    app: test-agic-app
spec:
  containers:
  - image: "mcr.microsoft.com/dotnet/core/samples:aspnetapp"
    name: aspnetapp-image
    ports:
    - containerPort: 80
      protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: test-agic-app-service
spec:
  selector:
    app: test-agic-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-agic-app-ingress
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
    - host: test.agic.contoso.com
      http:
        paths:
          - path: /
            backend:
              serviceName: test-agic-app-service
              servicePort: 80
EOF
```

Copie y pegue todas las líneas a la vez desde el script anterior en [Azure Cloud Shell](https://shell.azure.com/). Asegúrese de copiar todo el comando, empezando por `cat` e incluyendo el último `EOF`.

![apply](./media/application-gateway-ingress-controller-troubleshooting/tsg--apply-config.png)

Después de una implementación correcta de la aplicación en su clúster de AKS, tendrá un nuevo pod, servicio y una entrada.

Obtenga la lista de pods con [Cloud Shell](https://shell.azure.com/): `kubectl get pods -o wide`.
Esperamos que se haya creado un pod llamado "test-agic-app-pod". Tendrá una dirección IP. Esta dirección debe estar dentro de la red virtual de Application Gateway, que se usa con AKS.

![Captura de pantalla de la ventana de Bash en Azure Cloud Shell que muestra una lista de pods que incluye test-agic-app-pod en la lista.](./media/application-gateway-ingress-controller-troubleshooting/tsg--get-pods.png)

Obtenga la lista de servicios: `kubectl get services -o wide`. Esperamos ver un servicio llamado "test-agic-app-service".

![Captura de pantalla de la ventana de Bash en Azure Cloud Shell que muestra una lista de servicios que incluye test-agic-app-pod en la lista.](./media/application-gateway-ingress-controller-troubleshooting/tsg--get-services.png)

Obtenga la lista de las entradas: `kubectl get ingress`. Esperamos que se haya creado un recurso de entrada llamado "test-agic-app-ingress". El recurso tendrá el nombre de host "test.agic.contoso.com".

![Captura de pantalla de la ventana de Bash en Azure Cloud Shell que muestra una lista de entradas que incluye test-agic-app-ingress en la lista.](./media/application-gateway-ingress-controller-troubleshooting/tsg--get-ingress.png)

Uno de los pods será AGIC. `kubectl get pods` mostrará una lista de pods, uno de los cuales comenzará con "ingress-azure". Obtenga todos los registros de ese pod con `kubectl logs <name-of-ingress-controller-pod>` para comprobar que se ha realizado correctamente la implementación. Una implementación correcta agregará las siguientes líneas al registro:
```
I0927 22:34:51.281437       1 process.go:156] Applied Application Gateway config in 20.461335266s
I0927 22:34:51.281585       1 process.go:165] cache: Updated with latest applied config.
I0927 22:34:51.282342       1 process.go:171] END AppGateway deployment
```

Como alternativa, desde [Cloud Shell](https://shell.azure.com/) podemos recuperar solo las líneas que indican la configuración correcta de la instancia de Application Gateway con `kubectl logs <ingress-azure-....> | grep 'Applied App Gateway config in'`, donde `<ingress-azure....>` debe ser el nombre exacto del pod de AGIC.

Se aplicará la siguiente configuración a Application Gateway:

- Cliente de escucha: ![listener](./media/application-gateway-ingress-controller-troubleshooting/tsg--listeners.png)

- Regla de enrutamiento: ![routing_rule](./media/application-gateway-ingress-controller-troubleshooting/tsg--rule.png)

- Grupo de back-end:
  - Habrá una dirección IP en el grupo de direcciones de back-end y coincidirá con la dirección IP del pod que hemos observado anteriormente con `kubectl get pods -o wide`
![backend_pool](./media/application-gateway-ingress-controller-troubleshooting/tsg--backendpools.png)


Por último, podemos usar el comando `cURL` desde [Cloud Shell](https://shell.azure.com/) para establecer una conexión HTTP con la aplicación recién implementada:

1. Use `kubectl get ingress` para obtener la dirección IP pública de Application Gateway
2. Use `curl -I -H 'test.agic.contoso.com' <publitc-ip-address-from-previous-command>`

![Captura de pantalla de la ventana de Bash en Azure Cloud Shell que muestra un comando cURL que establece correctamente una conexión HTTP a la aplicación de prueba.](./media/application-gateway-ingress-controller-troubleshooting/tsg--curl.png)

El resultado de `HTTP/1.1 200 OK` indica que el sistema de Application Gateway + AKS + AGIC funciona según lo esperado.


## <a name="inspect-kubernetes-installation"></a>Inspección de la instalación de Kubernetes

### <a name="pods-services-ingress"></a>Pods, servicios, entrada
El controlador de entrada de Application Gateway (AGIC) supervisa continuamente los siguientes recursos de Kubernetes: [Implementación](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#creating-a-deployment) o [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/#what-is-a-pod), [Servicio](https://kubernetes.io/docs/concepts/services-networking/service/), [Entrada](https://kubernetes.io/docs/concepts/services-networking/ingress/)


Para que AGIC funcione según lo esperado, es necesario que:
  1. AKS cuente con uno o más **pods** en correcto estado.
     Compruébelo desde [Cloud Shell](https://shell.azure.com/) con `kubectl get pods -o wide --show-labels`. Si tiene un pod con una `apsnetapp`, el resultado puede ser similar al siguiente:
     ```bash
     delyan@Azure:~$ kubectl get pods -o wide --show-labels

     NAME                   READY   STATUS    RESTARTS   AGE   IP          NODE                       NOMINATED NODE   READINESS GATES   LABELS
     aspnetapp              1/1     Running   0          17h   10.0.0.6    aks-agentpool-35064155-1   <none>           <none>            app=aspnetapp
     ```

  2. Uno o más **servicios** hagan referencia a los pods anteriores a través de etiquetas de `selector` coincidentes.
     Compruébelo desde [Cloud Shell](https://shell.azure.com/) con `kubectl get services -o wide`
     ```bash
     delyan@Azure:~$ kubectl get services -o wide --show-labels

     NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE   SELECTOR        LABELS
     aspnetapp           ClusterIP   10.2.63.254    <none>        80/TCP    17h   app=aspnetapp   <none>     
     ```

  3. La **entrada**, anotada con `kubernetes.io/ingress.class: azure/application-gateway`, haga referencia al servicio anterior. Compruébelo desde [Cloud Shell](https://shell.azure.com/) con `kubectl get ingress -o wide --show-labels`
     ```bash
     delyan@Azure:~$ kubectl get ingress -o wide --show-labels

     NAME        HOSTS   ADDRESS   PORTS   AGE   LABELS
     aspnetapp   *                 80      17h   <none>
     ```

  4. Vea las anotaciones de la entrada anterior: `kubectl get ingress aspnetapp -o yaml` (sustituya `aspnetapp` por el nombre de la entrada)
     ```bash
     delyan@Azure:~$ kubectl get ingress aspnetapp -o yaml

     apiVersion: extensions/v1beta1
     kind: Ingress
     metadata:
       annotations:
         kubernetes.io/ingress.class: azure/application-gateway
       name: aspnetapp
     spec:
       backend:
         serviceName: aspnetapp
         servicePort: 80
     ```

     El recurso de entrada se debe anotar con `kubernetes.io/ingress.class: azure/application-gateway`.
 

### <a name="verify-observed-namespace"></a>Comprobar el espacio de nombres observado

* Obtenga los espacios de nombres existentes en el clúster de Kubernetes. ¿En qué espacio de nombres se ejecuta la aplicación? ¿AGIC supervisa ese espacio de nombres? Consulte la documentación sobre la [compatibilidad con varios espacios de nombres](./ingress-controller-multiple-namespace-support.md#enable-multiple-namespace-support) para conocer cómo configurar correctamente los espacios de nombres observados.

    ```bash
    # What namespaces exist on your cluster
    kubectl get namespaces
    
    # What pods are currently running
    kubectl get pods --all-namespaces -o wide
    ```


* El pod de AGIC debe estar en el espacio de nombres `default` (vea la columna `NAMESPACE`). Un pod correcto tendrá `Running` en la columna `STATUS`. Debe haber al menos un pod de AGIC.

    ```bash
    # Get a list of the Application Gateway Ingress Controller pods
    kubectl get pods --all-namespaces --selector app=ingress-azure
    ```


* Si el pod de AGIC no es correcto (la columna `STATUS` del comando anterior no se muestra como `Running`):
  - obtenga los registros para comprender el por qué: `kubectl logs <pod-name>`
  - para la instancia anterior del pod: `kubectl logs <pod-name> --previous`
  - describa el pod para obtener más contexto: `kubectl describe pod <pod-name>`


* ¿Tiene un [servicio](https://kubernetes.io/docs/concepts/services-networking/service/) y recursos de [entrada](https://kubernetes.io/docs/concepts/services-networking/ingress/) de Kubernetes?
    
    ```bash
    # Get all services across all namespaces
    kubectl get service --all-namespaces -o wide
    
    # Get all ingress resources across all namespaces
    kubectl get ingress --all-namespaces -o wide
    ```


* ¿La [entrada](https://kubernetes.io/docs/concepts/services-networking/ingress/) está anotada con: `kubernetes.io/ingress.class: azure/application-gateway`? AGIC solo inspeccionará los recursos de entrada de Kubernetes que tengan esta anotación.
    
    ```bash
    # Get the YAML definition of a particular ingress resource
    kubectl get ingress --namespace  <which-namespace?>  <which-ingress?>  -o yaml
    ```


* AGIC emite eventos de Kubernetes para determinados errores críticos. Puede verlos:
  - en el terminal mediante `kubectl get events --sort-by=.metadata.creationTimestamp`
  - en el explorador con la [interfaz de usuario web de Kubernetes (panel)](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)


## <a name="logging-levels"></a>Niveles de registro

AGIC cuenta con 3 niveles de registro. El nivel 1 es el predeterminado y muestra un número mínimo de líneas de registro.
Por otro lado, el nivel 5 mostrará todos los registros, incluido el contenido saneado de la configuración que se aplica a ARM.

La comunidad de Kubernetes ha establecido 9 niveles de registro para la herramienta [kubectl](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-output-verbosity-and-debugging). En este repositorio usamos 3 de ellos, con una semántica similar:


| Nivel de detalle | Descripción |
|-----------|-------------|
|  1        | Nivel de registro predeterminado; muestra los detalles de inicio, las advertencias y los errores |
|  3        | Información ampliada sobre eventos y cambios; listas de objetos creados |
|  5        | Registra objetos serializados; muestra la configuración de JSON saneada que se aplica a ARM |


Los niveles de detalle se pueden ajustar a través de la variable `verbosityLevel` del archivo [helm-config.yaml](#sample-helm-config-file). Aumente el nivel de detalle a `5` para obtener la configuración de JSON enviada a [ARM](../azure-resource-manager/management/overview.md):
  - agregue `verbosityLevel: 5` en una línea en [helm-config.yaml](#sample-helm-config-file) y vuelva a instalarlo
  - obtenga registros con `kubectl logs <pod-name>`

### <a name="sample-helm-config-file"></a>Archivo de ejemplo de configuración de Helm
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
# Specify if the cluster is Kubernetes RBAC enabled or not
rbac:
    enabled: false # true/false

# Specify aks cluster related information. THIS IS BEING DEPRECATED.
aksClusterConfiguration:
    apiServerAddress: <aks-api-server-address>
```