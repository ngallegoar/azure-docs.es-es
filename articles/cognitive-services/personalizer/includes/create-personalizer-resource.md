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
ms.openlocfilehash: 11abd52681d7c9962af4e5bf0728f97b256223c1
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122756"
---
## <a name="create-a-personalizer-azure-resource"></a>Creación de un recurso de Azure para Personalizer

Cree un recurso para Personalizer mediante [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local. También puede:

* Obtener una [clave de prueba](https://azure.microsoft.com/try/cognitive-services) válida durante siete días de forma gratuita. Después de registrarse, estará disponible en el [sitio web de Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/).
* Ver el recurso en [Azure Portal](https://portal.azure.com/)

Después de obtener una clave del recurso o la suscripción de evaluación, cree dos [variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `PERSONALIZER_RESOURCE_KEY` para la clave de recurso.
* `PERSONALIZER_RESOURCE_ENDPOINT` para el punto de conexión de recurso.

En Azure Portal, los valores de clave y punto de conexión están disponibles en la página **Inicio rápido**.
