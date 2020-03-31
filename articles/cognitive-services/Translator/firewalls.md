---
title: 'Traducción detrás de los firewalls: Translator Text API'
titleSuffix: Azure Cognitive Services
description: Translator Text API de Azure Cognitive Services puede traducir detrás de los firewalls con el filtrado de nombres de dominio o de direcciones IP.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cd7904fedd3ab3f64315cb6f98d99b8fd12254f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837396"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>Traducción detrás de los firewalls de direcciones IP con Translator Text API

Translator Text API puede traducir detrás de los firewalls utilizando el filtrado de nombres de dominio o de direcciones IP. El filtrado de nombres de dominio es el método preferido. **No se recomienda** ejecutar Microsoft Translator desde detrás de un firewall con filtrado de direcciones IP. Es probable que la configuración se interrumpa en el futuro sin previo aviso.

## <a name="translator-ip-addresses"></a>Direcciones IP de Translator
Direcciones IP de api.cognitive.microsofttranslator.com en API Microsoft Translator Text desde el 21 de agosto de 2019:

* **Asia Pacífico:** 20.40.125.208, 20.43.88.240, 20.184.58.62, 40.90.139.163, 104.44.89.44
* **Europa:** 40.90.138.4, 40.90.141.99, 51.105.170.64, 52.155.218.251
* **Norteamérica:** 40.90.139.36, 40.90.139.2, 40.119.2.134, 52.224.200.129, 52.249.207.163


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Translate behind IP firewalls in your Translator API call](reference/v3-0-translate.md) (Traducción detrás de los firewalls de direcciones IP en la llamada API de Translator)
