---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 01/15/2020
ms.author: diberry
ms.openlocfilehash: b1013b261e9449aef45f629c729579f4c87c1f6b
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2020
ms.locfileid: "85378496"
---
## <a name="create-a-personalizer-azure-resource"></a>Creación de un recurso de Azure para Personalizer

Cree un recurso para Personalizer mediante [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local. 

Después de obtener una clave del recurso, cree dos [variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_RESOURCE_KEY` para la clave de recurso.
* `PERSONALIZER_RESOURCE_ENDPOINT` para el punto de conexión de recurso.

En Azure Portal, los valores de clave y punto de conexión están disponibles en la página **Inicio rápido**.
