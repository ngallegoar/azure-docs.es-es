---
title: Habilitar la afinidad basada en cookies con Application Gateway
description: En este artículo se proporciona información sobre cómo habilitar la afinidad basada en cookies con una instancia de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: adacd805e736524fd7956c4bbc0ad402980b4cd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795989"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Habilitar la afinidad basada en cookies con una instancia de Application Gateway
Como se describe en la [Documentación de Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#http-settings), Application Gateway es compatible con la afinidad basada en cookies, lo que significa que puede dirigir el tráfico posterior de una sesión de usuario hasta el mismo servidor para su procesamiento.

## <a name="example"></a>Ejemplo
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```
