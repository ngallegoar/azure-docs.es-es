---
title: Habilitar la afinidad basada en cookies con Application Gateway
description: En este artículo se proporciona información sobre cómo habilitar la afinidad basada en cookies con una instancia de Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: a7806cf9518090539ba540a9a522af1aae2691f0
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397423"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Habilitar la afinidad basada en cookies con una instancia de Application Gateway
Como se describe en la [Documentación de Azure Application Gateway](./application-gateway-components.md#http-settings), Application Gateway es compatible con la afinidad basada en cookies, lo que significa que puede dirigir el tráfico posterior de una sesión de usuario hasta el mismo servidor para su procesamiento.

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