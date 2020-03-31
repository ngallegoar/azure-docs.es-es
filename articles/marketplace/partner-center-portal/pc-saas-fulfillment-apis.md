---
title: API de suministro de SaaS | Azure Marketplace
description: Presenta las versiones de las API de suministro que le permiten integrar su ofertas SaaS con Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dsindona
ms.openlocfilehash: 92b1c52457fa92709381124480c05a5f636167f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275737"
---
# <a name="saas-fulfillment-apis"></a>API de suministro de SaaS

Las API de suministro de SaaS permiten que fabricantes de software independientes (ISV) integren sus aplicaciones SaaS con Azure Marketplace. Estas API permiten a las aplicaciones de ISV participar en todos los canales de comercio: directo, dirigidos por asociados (revendedor) y sobre el terreno.  Son un requisito para mostrar ofertas de SaaS que permiten transacciones en Azure Marketplace.

> [!WARNING]
> La versión actual de esta API es la versión 2, que se debe usar para todas las nuevas ofertas de SaaS.  La versión 1 de la API está en desuso y se mantiene para proporcionar soporte a las ofertas existentes.


## <a name="business-model-support"></a>Compatibilidad con el modelo de negocio

Esta API admite las siguientes capacidades del modelo de negocio; puede:

* Especificar varios planes para una oferta. Estos planes tienen funcionalidades diferentes y es posible que precios diferentes.
* Proporcionar una oferta por cada modelo de facturación de usuario o sitio.
* Proporcionar opciones de facturación mensuales y anuales (pagadas por adelantado).
* Proporcionar precios privados para un cliente en función de un acuerdo empresarial negociado.


## <a name="next-steps"></a>Pasos siguientes

Si no lo ha hecho ya, registre su aplicación de SaaS en [Azure Portal](https://ms.portal.azure.com), tal como se explica en [Registro de una aplicación SaaS](./pc-saas-registration.md).  Después, use la versión más reciente de esta interfaz para el desarrollo: [API de suministro de SaaS, versión 2](./pc-saas-fulfillment-api-v2.md).
