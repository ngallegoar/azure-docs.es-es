---
title: Habilitar la afinidad basada en cookies con Application Gateway
description: En este artículo se proporciona información sobre cómo habilitar la afinidad basada en cookies con una instancia de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 3af2705fedbb9c19d4f128e8e997d3fa73f8b5a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84807967"
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
