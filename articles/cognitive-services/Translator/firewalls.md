---
title: 'Traducción detrás de firewalls: Translator'
titleSuffix: Azure Cognitive Services
description: Translator de Azure Cognitive Services puede traducir detrás de los firewalls con el filtrado de nombres de dominio o de direcciones IP.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: dce41879f77d0bed44daa89c1dabbcc3f92e145e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592532"
---
# <a name="how-to-translate-behind-ip-firewalls-with-translator"></a>Traducción detrás de firewalls de direcciones IP con Translator

Translator puede traducir detrás de los firewalls utilizando el filtrado de nombres de dominio o de direcciones IP. El filtrado de nombres de dominio es el método preferido. **No se recomienda** ejecutar Microsoft Translator desde detrás de un firewall con filtrado de direcciones IP. Es probable que la configuración se interrumpa en el futuro sin previo aviso.

## <a name="translator-ip-addresses"></a>Direcciones IP de Translator
Direcciones IP de api.cognitive.microsofttranslator.com en Translator al 21 de agosto de 2019:

* **Asia Pacífico:** 20.40.125.208, 20.43.88.240, 20.184.58.62, 40.90.139.163, 104.44.89.44
* **Europa:** 40.90.138.4, 40.90.141.99, 51.105.170.64, 52.155.218.251
* **Norteamérica:** 40.90.139.36, 40.90.139.2, 40.119.2.134, 52.224.200.129, 52.249.207.163

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Traducción detrás de firewalls de direcciones IP en Translator](reference/v3-0-translate.md)
