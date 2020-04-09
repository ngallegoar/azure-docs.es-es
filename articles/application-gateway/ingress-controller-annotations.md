---
title: Anotaciones del controlador de entrada de Application Gateway
description: Este artículo ofrece documentación sobre las anotaciones específicas del controlador de entrada de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: f54381ddcd11a2e4a24d30d812468da85b5403de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335822"
---
# <a name="annotations-for-application-gateway-ingress-controller"></a>Anotaciones para el controlador de entrada de Application Gateway 

## <a name="introductions"></a>Introducciones

El recurso de entrada de Kubernetes se puede anotar con pares clave-valor arbitrarios. AGIC depende de las anotaciones para programar las características de Application Gateway, que no se pueden configurar a través del YAML de entrada. Las anotaciones de entrada se aplican a todas las configuraciones HTTP, los grupos de back-end y los clientes de escucha derivados de un recurso de entrada.

## <a name="list-of-supported-annotations"></a>Lista de anotaciones admitidas

Para que AGIC observe un recurso de entrada, **se debe anotar** con `kubernetes.io/ingress.class: azure/application-gateway`. Solo entonces AGIC funcionará con el recurso de entrada en cuestión.

| Clave de anotación | Tipo de valor | Valor predeterminado | Valores permitidos
| -- | -- | -- | -- |
| [appgw.ingress.kubernetes.io/backend-path-prefix](#backend-path-prefix) | `string` | `nil` | |
| [appgw.ingress.kubernetes.io/ssl-redirect](#tls-redirect) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/connection-draining](#connection-draining) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/connection-draining-timeout](#connection-draining) | `int32` (segundos) | `30` | |
| [appgw.ingress.kubernetes.io/cookie-based-affinity](#cookie-based-affinity) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/request-timeout](#request-timeout) | `int32` (segundos) | `30` | |
| [appgw.ingress.kubernetes.io/use-private-ip](#use-private-ip) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/backend-protocol](#backend-protocol) | `string` | `http` | `http`, `https` |

## <a name="backend-path-prefix"></a>Prefijo de ruta de acceso de back-end

Esta anotación permite volver a escribir la ruta de acceso de back-end especificada en un recurso de entrada con el prefijo especificado en esta anotación. Esto permite a los usuarios exponer servicios cuyos puntos de conexión son diferentes de los nombres de puntos de conexión usados para exponer un servicio en un recurso de entrada.

### <a name="usage"></a>Uso

```yaml
appgw.ingress.kubernetes.io/backend-path-prefix: <path prefix>
```

### <a name="example"></a>Ejemplo

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-bkprefix
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/backend-path-prefix: "/test/"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```
En el ejemplo anterior, se ha definido un recurso de entrada denominado `go-server-ingress-bkprefix` con una anotación `appgw.ingress.kubernetes.io/backend-path-prefix: "/test/"`. La anotación le indica a la instancia de Application Gateway que cree una configuración de HTTP, que tendrá una invalidación de prefijo de ruta de acceso para la ruta de acceso `/hello` a `/test/`.

> [!NOTE] 
> En el ejemplo anterior, solo se ha definido una regla. Sin embargo, las anotaciones se pueden aplicar a todo el recurso de entrada, por lo que si un usuario hubiera definido varias reglas, se configuraría el prefijo de la ruta de acceso de back-end para cada una de las rutas de acceso especificadas. Por lo tanto, si un usuario desea tener diferentes reglas con prefijos de ruta de acceso diferentes (incluso para el mismo servicio), tendrían que definir diferentes recursos de entrada.

## <a name="tls-redirect"></a>Redirección de TLS

Application Gateway [puede configurarse](https://docs.microsoft.com/azure/application-gateway/application-gateway-redirect-overview) para redirigir automáticamente las direcciones URL HTTP a sus equivalentes HTTPS. Cuando esta anotación está presente y TLS está configurado correctamente, el controlador de entrada de Kubernetes creará una [regla de enrutamiento con una configuración de redireccionamiento](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal#add-a-routing-rule-with-a-redirection-configuration) y aplicará los cambios a su instancia de Application Gateway. La redirección creada será HTTP `301 Moved Permanently`.

### <a name="usage"></a>Uso

```yaml
appgw.ingress.kubernetes.io/ssl-redirect: "true"
```

### <a name="example"></a>Ejemplo

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-redirect
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
   - hosts:
     - www.contoso.com
     secretName: testsecret-tls
  rules:
  - host: www.contoso.com
    http:
      paths:
      - backend:
          serviceName: websocket-repeater
          servicePort: 80
```

## <a name="connection-draining"></a>Purga de la conexión

`connection-draining`: Esta anotación permite a los usuarios especificar si se habilitará la purga de la conexión.
`connection-draining-timeout`: Esta anotación permite a los usuarios especificar un tiempo de espera después del cual Application Gateway finalizará las solicitudes al punto de conexión de back-end de purga.

### <a name="usage"></a>Uso

```yaml
appgw.ingress.kubernetes.io/connection-draining: "true"
appgw.ingress.kubernetes.io/connection-draining-timeout: "60"
```

### <a name="example"></a>Ejemplo

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-drain
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/connection-draining: "true"
    appgw.ingress.kubernetes.io/connection-draining-timeout: "60"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="cookie-based-affinity"></a>Afinidad basada en cookies

Esta anotación permite a los usuarios especificar si se habilitará la afinidad basada en cookies.

### <a name="usage"></a>Uso

```yaml
appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
```

### <a name="example"></a>Ejemplo

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-affinity
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="request-timeout"></a>Tiempo de espera de solicitud

Esta anotación permite especificar el tiempo de espera de la solicitud en segundos después del cual Application Gateway producirá un error en la solicitud si no se recibe respuesta.

### <a name="usage"></a>Uso

```yaml
appgw.ingress.kubernetes.io/request-timeout: "20"
```

### <a name="example"></a>Ejemplo

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/request-timeout: "20"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="use-private-ip"></a>Uso de una dirección IP privada

Esta anotación nos permite especificar si se va a exponer este punto de conexión en la dirección IP privada de Application Gateway.

> [!NOTE]
> * Application Gateway no admite varias direcciones IP en el mismo puerto (por ejemplo: 80/443). Una entrada con una anotación `appgw.ingress.kubernetes.io/use-private-ip: "false"` y otra con `appgw.ingress.kubernetes.io/use-private-ip: "true"` en `HTTP` producirá un error de AGIC al actualizar la instancia de Application Gateway.
> * En el caso de una instancia de Application Gateway sin una dirección IP privada, se omitirán las entradas anotadas con `appgw.ingress.kubernetes.io/use-private-ip: "true"`. Esto se reflejará en los registros del controlador y los eventos de entrada de dichas entradas con la advertencia `NoPrivateIP`.


### <a name="usage"></a>Uso
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

### <a name="example"></a>Ejemplo
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/use-private-ip: "true"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="backend-protocol"></a>Protocolo de back-end

Esta anotación nos permite especificar el protocolo que Application Gateway debe usar al comunicarse con los pods. Protocolos admitidos: `http`, `https`

> [!NOTE]
> * Aunque los certificados autofirmados se admiten en Application Gateway, actualmente, AGIC solo admite `https` cuando los pods usan certificados firmados por una entidad de certificación conocida.
> * Asegúrese de no usar el puerto 80 con HTTPS y el puerto 443 con HTTP en los pods.

### <a name="usage"></a>Uso
```yaml
appgw.ingress.kubernetes.io/backend-protocol: "https"
```

### <a name="example"></a>Ejemplo
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/backend-protocol: "https"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 443
```