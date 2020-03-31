---
title: 'Configuración de la autenticación de cliente de llamadas entrantes: Azure Event Grid IoT Edge | Microsoft Docs'
description: Configure los protocolos de API expuestos por Event Grid en IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 3363db4557dd19e8d72747ccd62bb535abb7b1e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841798"
---
# <a name="configure-client-authentication-of-incoming-calls"></a>Configuración de la autenticación de cliente de las llamadas entrantes

En esta guía se proporcionan ejemplos de las posibles configuraciones de autenticación de cliente para el módulo de Event Grid. El módulo de Event Grid admite dos tipos de autenticación de cliente:

* Basada en claves de firma de acceso compartido (SAS).
* Basada en certificados.

Consulte la guía [Seguridad y autenticación](security-authentication.md) para ver todas las configuraciones posibles.

## <a name="enable-certificate-based-client-authentication-no-self-signed-certificates"></a>Habilitación de la autenticación de cliente basada en certificados, sin certificados autofirmados

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=false"
  ]
}
 ```

## <a name="enable-certificate-based-client-authentication-allow-self-signed-certificates"></a>Habilitación de la autenticación de cliente basada en certificados, permitir certificados autofirmados

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
```

>[!NOTE]
>Establezca la propiedad **inbound__clientAuth__clientCert__allowUnknownCA** en **true** solo en entornos de prueba, ya que normalmente usará certificados autofirmados. En las cargas de trabajo de producción, se recomienda establecer esta propiedad en **false** y que los certificados provengan de una entidad de certificación (CA).

## <a name="enable-certificate-based-and-sas-key-based-client-authentication"></a>Habilitación de la autenticación de cliente basada en certificados y claves SAS

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=true",
    "inbound__clientAuth__sasKeys__key1=<some-secret1-here>",
    "inbound__clientAuth__sasKeys__key2=<some-secret2-here>",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>La autenticación de cliente basada en claves SAS permite que un módulo que no sea de IoT Edge realice operaciones de administración y tiempo de ejecución, suponiendo que se pueda tener acceso a los puertos de API fuera de la red de IoT Edge.
