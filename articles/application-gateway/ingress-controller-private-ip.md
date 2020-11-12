---
title: Uso de una dirección IP privada para el enrutamiento interno de un punto de conexión de entrada
description: Este artículo ofrece información sobre el uso de direcciones IP privadas para el enrutamiento interno y, por tanto, para exponer el punto de conexión de entrada dentro de un clúster al resto de la red virtual.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 8be37ed1da0da4da3db43ef4c1cd01ed962f24ed
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397315"
---
# <a name="use-private-ip-for-internal-routing-for-an-ingress-endpoint"></a>Uso de una dirección IP privada para el enrutamiento interno de un punto de conexión de entrada 

Esta característica permite exponer el punto de conexión de entrada en el `Virtual Network` mediante una dirección IP privada.

## <a name="pre-requisites"></a>Requisitos previos  
Instancia de Application Gateway con una [configuración de IP privada](./configure-application-gateway-with-private-frontend-ip.md)

Hay dos maneras de configurar el controlador para que use una dirección IP privada para la entrada,

## <a name="assign-to-a-particular-ingress"></a>Asignar a una entrada determinada
Para exponer una entrada determinada a través de una dirección IP privada, use la anotación [`appgw.ingress.kubernetes.io/use-private-ip`](./ingress-controller-annotations.md#use-private-ip) para la entrada.

### <a name="usage"></a>Uso
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

En el caso de una instancia de Application Gateway sin una dirección IP privada, se omitirán las entradas anotadas con `appgw.ingress.kubernetes.io/use-private-ip: "true"`. Esto se indicará en el evento de entrada y en el registro del pod de AGIC.

* Error tal como se indica en el evento de entrada

    ```bash
    Events:
    Type     Reason       Age               From                                                                     Message
    ----     ------       ----              ----                                                                     -------
    Warning  NoPrivateIP  2m (x17 over 2m)  azure/application-gateway, prod-ingress-azure-5c9b6fcd4-bctcb  Ingress default/hello-world-ingress requires Application Gateway 
    applicationgateway3026 has a private IP address
    ```

* Error tal como se indica en los registros de AGIC

    ```bash
    E0730 18:57:37.914749       1 prune.go:65] Ingress default/hello-world-ingress requires Application Gateway applicationgateway3026 has a private IP address
    ```


## <a name="assign-globally"></a>Asignar globalmente
En caso de que sea necesario restringir todas las entradas para que se expongan a través de una dirección IP privada, use `appgw.usePrivateIP: true` en la configuración de `helm`.

### <a name="usage"></a>Uso
```yaml
appgw:
    subscriptionId: <subscriptionId>
    resourceGroup: <resourceGroupName>
    name: <applicationGatewayName>
    usePrivateIP: true
```

Esto hará que el controlador de entrada filtre las configuraciones de direcciones IP para una dirección IP privada al configurar los clientes de escucha de front-end en la instancia de Application Gateway.
Se producirá un error en AGIC si no se asigna `usePrivateIP: true` y una dirección IP privada.

> [!NOTE]
> La SKU de Application Gateway v2 requiere una dirección IP pública. Si necesita que la instancia de Application Gateway sea privada, conecte un [`Network Security Group`](../virtual-network/network-security-groups-overview.md) a la subred de Application Gateway para restringir el tráfico.