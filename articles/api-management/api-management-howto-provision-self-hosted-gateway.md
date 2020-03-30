---
title: Aprovisionamiento de una puerta de enlace autohospedada en Azure API Management | Microsoft Docs
description: Aprenda a aprovisionar una puerta de enlace autohospedada en Azure API Management.
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
ms.openlocfilehash: d33c5f75234ad7165a9062ecc3bb2a00d502f8c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74075287"
---
# <a name="provision-a-self-hosted-gateway-in-azure-api-management"></a>Aprovisionamiento de una puerta de enlace autohospedada en Azure API Management

El aprovisionamiento de un recurso de puerta de enlace en su instancia de Azure API Management es un requisito previo para implementar una puerta de enlace autohospedada. En este artículo se describen los pasos necesarios para aprovisionar un recurso de puerta de enlace en API Management.

> [!NOTE]
> La característica de puerta de enlace autohospedada se encuentra en versión preliminar. Durante la versión preliminar, la puerta de enlace autohospedada solo está disponible en los niveles Desarrollador y Premium sin cargo adicional. El nivel de Desarrollador está limitado a una única implementación de puerta de enlace autohospedada.

## <a name="prerequisites"></a>Prerequisites

Completar la guía de inicio rápido siguiente: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="provision-a-self-hosted-gateway"></a>Aprovisionamiento de una puerta de enlace autohospedada

1. Seleccione **Puertas de enlace** en **Configuración**.
2. Haga clic en **+ Agregar**.
3. Escribe el **Nombre** y la **Región** de la puerta de enlace.
> [!TIP]
> La **Región** especifica la ubicación prevista de los nodos de puerta de enlace que se asociarán a este recurso de puerta de enlace. Es semánticamente equivalente a una propiedad similar asociada a cualquier recurso de Azure, pero se le puede asignar un valor de cadena arbitrario.

4. Opcionalmente, puede escribir una **Descripción** del recurso de puerta de enlace.
5. Opcionalmente, puede seleccionar **+** en **API** para asociar una o más API a este recurso de puerta de enlace.
> [!TIP]
> Puede asociar y quitar una API de una puerta de enlace en la pestaña **Configuración** de la API.

> [!IMPORTANT]
> De forma predeterminada, ninguna de las API existentes se asociará con el nuevo recurso de puerta de enlace. Por lo tanto, los intentos de invocarlas a través de la nueva puerta de enlace producirán la respuesta `404 Resource Not Found`.

6. Haga clic en **Agregar**.

Ahora el recurso de puerta de enlace se ha aprovisionado en su instancia de API Management. Puede continuar con la implementación de la puerta de enlace.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la puerta de enlace autohospedada, consulte [Introducción a la puerta de enlace autohospedada de Azure API Management](self-hosted-gateway-overview.md)
* Obtenga más información sobre el procedimiento de [Implementación de una puerta de enlace autohospedada en Kubernetes](api-management-howto-deploy-self-hosted-gateway-to-k8s.md)
* Obtenga más información sobre el procedimiento de [Implementación de una puerta de enlace autohospedada en Docker](api-management-howto-deploy-self-hosted-gateway-to-docker.md)
