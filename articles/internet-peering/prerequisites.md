---
title: Requisitos previos para configurar el emparejamiento con Microsoft
titleSuffix: Azure
description: Requisitos previos para configurar el emparejamiento con Microsoft
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 3c820a7be561aeef9b7e50fd0ac0cf4dee721af8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "75774007"
---
# <a name="prerequisites-to-set-up-peering-with-microsoft"></a>Requisitos previos para configurar el emparejamiento con Microsoft

Asegúrese de que se cumplen los requisitos siguientes antes de solicitar un nuevo emparejamiento o de convertir un emparejamiento heredado en un recurso de Azure.

## <a name="azure-related-prerequisites"></a>Requisitos previos relacionados con Azure
* **Cuenta de Microsoft Azure:** cree una [cuenta de Microsoft Azure](https://azure.microsoft.com/free) si no tiene ninguna. Se requiere una suscripción de Microsoft Azure válida y activa para configurar el emparejamiento, ya que los emparejamientos se modelan como recursos en las suscripciones de Azure. Es importante tener en cuenta lo siguiente:
    * Los tipos de recursos de Azure que se usan para configurar el emparejamiento son siempre productos de Azure gratuitos (es decir, no se le cobra por la creación de una cuenta de Azure ni por la creación de una suscripción o el acceso a los recursos de Azure **PeerAsn** y **Emparejamiento** para configurar el emparejamiento). No se debe confundir con el acuerdo de emparejamiento para el emparejamiento directo entre usted y Microsoft, los términos del cual se describen explícitamente con nuestro equipo de emparejamiento. Póngase en contacto con el [emparejamiento de Microsoft](mailto:peering@microsoft.com) si tiene alguna pregunta en este sentido.
    * Puede usar la misma suscripción de Azure para acceder a otros productos o servicios en la nube de Azure, que pueden ser gratuitos o de pago. Al acceder a un producto de pago, se le aplicarán cargos.
    * Si crea una nueva cuenta o suscripción de Azure, es posible que pueda optar a crédito de Azure gratuito durante un período de prueba, que puede usar para probar los servicios Azure Cloud. Si le interesa, visite la página sobre las [cuentas de Microsoft Azure](https://azure.microsoft.com/free) para obtener más información.

* **Asociar ASN del mismo nivel:** antes de solicitar el emparejamiento, asocie primero su información de contacto y ASN a su suscripción. Siga las instrucciones de [Asociación de un ASN del mismo nivel a una suscripción de Azure](howto-subscription-association-powershell.md).

## <a name="other-prerequisites"></a>Otros requisitos previos
* **Perfil de PeeringDB:** se espera que los elementos del mismo nivel tengan un perfil completo y actualizado en [PeeringDB](https://www.peeringdb.com). Usamos esta información en nuestro sistema de registro para validar los detalles del elemento del mismo nivel, como su información de NOC, sus datos de contacto técnico, su presencia en las instalaciones de emparejamiento, etc.

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md)
* [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md)
* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md)