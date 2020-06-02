---
title: API de suministro de SaaS en el marketplace comercial de Microsoft
description: Introducción a las API de suministro que le permiten integrar su ofertas de SaaS en Microsoft AppSource y Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dsindona
ms.openlocfilehash: ba1b158bc529b148a8e3138d122c13ead19e073e
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858091"
---
# <a name="saas-fulfillment-apis-in-microsoft-commercial-marketplace"></a>API de suministro de SaaS en el marketplace comercial de Microsoft

Las API de suministro de SaaS permiten que fabricantes de software independientes (ISV) integren sus aplicaciones SaaS con Microsoft AppSource y Azure Marketplace. Estas API permiten a las aplicaciones de ISV participar en todos los canales de comercio: directo, dirigidos por asociados (revendedor) y sobre el terreno. Son necesarios para mostrar las ofertas de SaaS comercializables en Microsoft AppSource y Azure Marketplace.

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
