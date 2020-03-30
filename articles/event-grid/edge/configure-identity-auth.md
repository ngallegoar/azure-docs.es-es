---
title: 'Configuración de identidad: Azure Event Grid IoT Edge | Microsoft Docs'
description: Configuración de la identidad del módulo de Event Grid
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/05/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 0aedeea2a6ad08e1627c2d1a6ebde6c91a4d02d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841772"
---
# <a name="configure-identity-for-the-event-grid-module"></a>Configuración de la identidad para el módulo de Event Grid

En este artículo se muestra cómo configurar la identidad de Grid en Edge. De forma predeterminada, el módulo de Event Grid presenta su certificado de identidad tal y como lo configuró el demonio de seguridad de IoT. Event Grid en Microsoft Edge presenta su certificado de identidad con las llamadas salientes cuando entrega eventos. De este modo, un suscriptor puede validar que el módulo de Event Grid envió el evento antes de aceptarlo.

Consulte la guía [Seguridad y autenticación](security-authentication.md) para ver todas las configuraciones posibles.

## <a name="always-present-identity-certificate"></a>Presentar siempre certificado de identidad
A continuación se muestra un ejemplo de configuración para presentar siempre un certificado de identidad en llamadas salientes. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=true",
    "outbound__clientAuth__clientCert__source=IoTEdge"
  ]
}
 ```

## <a name="dont-present-identity-certificate"></a>No presentar certificado de identidad
A continuación se muestra un ejemplo de configuración para no presentar un certificado de identidad en llamadas salientes. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=false"
  ]
}
 ```
