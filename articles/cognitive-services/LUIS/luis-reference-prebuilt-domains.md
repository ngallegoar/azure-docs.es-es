---
title: 'Referencia del dominio precompilado: LUIS'
titleSuffix: Azure Cognitive Services
description: La referencia de dominios creados previamente, que son colecciones creadas previamente de intenciones y entidades de Language Understanding Intelligent Services (LUIS).
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.openlocfilehash: e93822f4a2992f619b598474082caa56562b6e36
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91538371"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Referencia de dominio creado previamente para la aplicación de LUIS
Esta referencia ofrece información sobre los [dominios creados previamente](luis-how-to-use-prebuilt-domains.md), que son colecciones creadas previamente de intenciones y entidades que LUIS ofrece.

Los [dominios personalizados](luis-how-to-start-new-app.md), por el contrario, se inician sin intenciones ni modelos. Puede agregar cualquier intención y entidad de dominios creados previamente a un modelo personalizado.

## <a name="prebuilt-domains-per-language"></a>Dominios pregenerados por idioma

En la tabla siguiente se resumen los dominios admitidos actualmente. La compatibilidad con el inglés suele ser más completa que otras.

| Tipo de entidad       | EN-US      | ZH-CN   | DE    | VF     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:|
| Calendario  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
|Comunicación  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Email     | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| HomeAutomation          | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Notas     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Lugares   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| RestaurantReservation  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Sectores públicos      | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Tiempo        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Web    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

Los dominios pregenerados **no se admiten** en:

* Francés (canadiense)
* Hindi
* Español (México)

## <a name="next-steps"></a>Pasos siguientes

Conozca la [entidad simple](reference-entity-simple.md).
